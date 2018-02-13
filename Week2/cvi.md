# Solution for [Week 2](Challenge)
## Author: [cvi](https://what.thedailywtf.com/user/cvi)

<a name="CPlusPlus"></a>
## Language: C++ (CUDA)

```
#include <limits>

namespace
{
	// Helpers for dealing with the sieve
	// For each odd number, there is a corresponding bit in the sieve. Bits for
	// non-primes are set to one; potential primes remain set to zero.
	//
	// Asserting a bit (bit_set()) needs to use atomics to avoid race conditions
	// as multiple threads will attempt to assert bits at the same time. Note 
	// that bit_test() may return false negatives (=falsly potentially prime)
	// unless preceeded by a full memory barrier.
	template< typename tUInt > __host__ __device__ inline
	tUInt bit_word( tUInt aNum )
	{
		return aNum / 2 / std::numeric_limits<tUInt>::digits;
	}
	template< typename tUInt > __host__ __device__ inline
	tUInt bit_pos( tUInt aNum )
	{
		return aNum / 2 % std::numeric_limits<tUInt>::digits;
	}

	template< typename tUInt > __host__ __device__ inline
	bool bit_test( tUInt const* aSieve, tUInt aNum )
	{
		return !!(aSieve[bit_word(aNum)] & (tUInt(1) << bit_pos(aNum)));
	}
	template< typename tUInt > __device__ inline
	void bit_set( tUInt* aSieve, tUInt aNum )
	{
		// This works around the issue of atomicOr() not being defined for 
		// uint64_t on Linux. Uint64_t is here a "unsigned long", but atomicOr()
		// is only defined for "unsigned int" and "unsigned long long".
		using ULL_ = unsigned long long int;

		atomicOr( (ULL_*)aSieve+bit_word(aNum), ULL_(tUInt(1) << bit_pos(aNum)) );
	}

	// KERNEL: sieve
	// Warps cooperate on a single number; each warp attempts different numbers.
	template< typename tUInt > __global__
	void sieve( tUInt* aBuffer, tUInt aLimit, tUInt aLimitSqrt )
	{
		auto const numWarps = blockDim.y*gridDim.x;
		auto const wid = blockIdx.x*blockDim.y + threadIdx.y;

		for( tUInt i = 3 + 2*wid; i < aLimitSqrt; i += 2*numWarps )
		{
			if( !bit_test( aBuffer, i ) )
			{
				for( tUInt j = i*i + 2*i*threadIdx.x; j < aLimit; j += 2*i*blockDim.x )
					bit_set( aBuffer, j );
			}
		}
	}

	// Helper: intra-warp reduction. RTFM.
	template< typename tUInt > __device__ inline
	tUInt reduce0( tUInt aValue )
	{
		aValue += __shfl_down_sync( ~0u, aValue, 16 );
		aValue += __shfl_down_sync( ~0u, aValue, 8 );
		aValue += __shfl_down_sync( ~0u, aValue, 4 );
		aValue += __shfl_down_sync( ~0u, aValue, 2 );
		aValue += __shfl_down_sync( ~0u, aValue, 1 );
		return aValue;
	}

	// KERNEL: accumulate
	// Iterate over all entries in the sieve in parallel. Each thread accumulates
	// a sum of all primes it has seen. 
	//
	// The per-thread sums are then reduced into a single per-warp value. Warps
	// in each block use shared memory to transfer their sums into the first 
	// warp of the block, which uses another intra-warp reduction to compute the
	// per-block total. Each block outputs the total; the final sum is performed
	// on the host.
	template< typename tUInt > __global__
	void accum( tUInt* aBuffer, tUInt aLimit )
	{
		// Iterate over the sieve using all threads.
		auto const numThreads = blockDim.x*blockDim.y*gridDim.x;
		auto const tid = blockIdx.x*blockDim.x*blockDim.y + threadIdx.y*blockDim.x + threadIdx.x;

		tUInt sum = 0;
		for( tUInt i = 3 + 2*tid; i < aLimit; i += 2*numThreads )
		{
			if( !bit_test( aBuffer, i ) )
				sum += i;
		}

		// intra-warp reduction
		auto const r = reduce0( sum );

		// transfer results to warp zero with shared memory
		__shared__ tUInt totals[32];
		if( 0 == threadIdx.y )
			totals[threadIdx.x] = 0;

		__syncthreads();

		if( 0 == threadIdx.x )
			totals[threadIdx.y] = r;

		__syncthreads();

		// warp zero performs a final reduction and stores the result in gmem
		if( 0 == threadIdx.y )
		{
			auto const t = totals[threadIdx.x];
			auto const q = reduce0( t );

			if( 0 == threadIdx.x )
				aBuffer[blockIdx.x] = q;
		}
	}
}


#include <vector>
#include <algorithm>

#include <cmath>
#include <cstdio>
#include <cstdint>

#include <inttypes.h>


int main( int aArgc, char* aArgv[] )
{
	using std::uint64_t;

	// Settings + constants
	constexpr uint64_t bitsPerWord = std::numeric_limits<uint64_t>::digits;
	constexpr uint64_t numsPerWord = bitsPerWord * 2;

	constexpr int sieveWarps = 16, sieveBlocks = 128;
	constexpr int reduceWarps = 16, reduceBlocks = 128;

	// Parse command line
	char dummy; 
	std::uint64_t upper;

	if( 2 != aArgc || 1 != std::sscanf( aArgv[1], "%" SCNu64 "%c", &upper, &dummy ) )
	{
		std::fprintf( stderr, "Synopsis: %s <number>\n", aArgv[0] );
		return 1;
	}

	// Allocate GPU memory
	// Uses a single buffer for both the sieve and the per-block results from
	// the reduction.
	auto const words = std::max<std::size_t>( (upper + numsPerWord -1)/numsPerWord, reduceBlocks );

	uint64_t* devBuf = nullptr;
	int r = cudaMalloc( &devBuf, sizeof(uint64_t)*words );
	int q = cudaMemset( devBuf, 0, sizeof(uint64_t)*words );

	// CUDA events for timing the individual kernel invocations
	cudaEvent_t sieveBeg, sieveEnd, redBeg, redEnd;
	cudaEventCreate( &sieveBeg, 0 );
	cudaEventCreate( &sieveEnd, 0 );
	cudaEventCreate( &redBeg, 0 );
	cudaEventCreate( &redEnd, 0 );

	// KERNEL: sieve
	cudaEventRecord( sieveBeg );
	{
		auto const upperSqrt = uint64_t(std::ceil(std::sqrt(upper)));

		dim3 b( sieveBlocks, 1, 1 ), t( 32, sieveWarps, 1 );
		sieve<<<b,t>>>( devBuf, upper, upperSqrt );
	}
	cudaEventRecord( sieveEnd );

	// KERNEL: reduction
	cudaEventRecord( redBeg );
	{
		dim3 b( reduceBlocks, 1, 1 ), t( 32, reduceWarps, 1 );
		accum<<<b,t>>>( devBuf, upper );
	}
	cudaEventRecord( redEnd );

	// Copy results to the host
	// Each block in the reduction returns a partial sum.
	std::vector<std::uint64_t> hostBuf( reduceBlocks );
	cudaMemcpy( hostBuf.data(), devBuf, sizeof(std::uint64_t)*reduceBlocks, cudaMemcpyDeviceToHost );

	// Sum the per-block sums
	// Add two because we've skipped two (the only even prime) until now.
	std::uint64_t sum = 2;
	for( std::uint64_t i = 0; i < reduceBlocks; ++i )
		sum += hostBuf[i];

	// Output.
	printf( "Sum: %" PRIu64 "\n", sum );

	float sieveMs, redMs;
	cudaEventElapsedTime( &sieveMs, sieveBeg, sieveEnd );
	cudaEventElapsedTime( &redMs, redBeg, redEnd );

	printf( "GPU time: sieve = %fms; reduction = %fms\n", sieveMs, redMs );

	return 0;
}
```