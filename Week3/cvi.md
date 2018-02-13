# Solution for [Week 3](README)
## Author: [cvi](https://what.thedailywtf.com/user/cvi)

<a name="Squirrel"></a>
## Language: Squirrel

```
function compute_change( aAmount, aDenominations )
{
	// Hack: deal with floating point inaccuracy.
	local epsilon = 1e-6;
	local is_mostly_zero_ = @(x) (fabs(x) <= epsilon);

	// Initial greedy guess
	local amount = aAmount;
	local guess = aDenominations.map( function(x) {
		local i = floor((amount+epsilon) / x);
		amount -= x*i;
		return i;
	} );

	if( !is_mostly_zero_(amount) )
		guess = null;

	// Prune unviable branches early (i.e., more than "minimum" coins required)
	local minimum = guess ? guess.reduce( @(x,y) x+y ) : 1.0/0.0;

	// Recursive search
	function recurse_( aAmount, aI, aDenom, aGuessCount, aExact )
	{
		if( is_mostly_zero_(aAmount) ) return aDenom.map( @(x) 0 );
		if( aI >= aDenom.len() ) 
		{
			if( !aExact && aAmount < aDenom[aDenom.len()-1] )
			{
				local r = aDenom.map( @(x) 0 );
				r[aDenom.len()-1] = 1;
				return r;
			}
			return null;
		}

		local res = null;
		local den = aDenom[aI];
		for( local i = floor((aAmount+epsilon)/den); i >= 0; --i )
		{
			if( aGuessCount+i < minimum )
			{
				local s = recurse_( aAmount-i*den, aI+1, aDenom, aGuessCount+i, aExact );
				if( s )
				{
					res = s;
					res[aI] += i;
					minimum = aGuessCount + res.reduce( @(x,y) x+y );
				}
			}
		}

		return res;
	}
	
	local r = recurse_( aAmount, 0, aDenominations, 0, true );
	if( !r ) r = recurse_( aAmount, 0, aDenominations, 0, false );
	return r ? r : guess;
}



function printf( ... )
{
	local aa = [null]; // extra "this" argument.
	aa.extend(vargv);
	::print( ::format.acall( aa ) );
}

tests <- [
	[4.99,  [1, 0.5, 0.25, 0.1, 0.05, 0.01],   [4, 1, 1, 2, 0, 4]],
	[4.89,  [1, 0.5, 0.25, 0.1, 0.05],         [4, 1, 1, 1, 1]],
	[0.66,  [1, 0.5, 0.25, 0.1, 0.05, 0.01],   [0, 1, 0, 1, 1, 1]],
	[16.01, [5, 2, 0.5, 0.25, 0.1, 0.05],      [2, 3, 0, 0, 0, 1]], // Also: 3,0,2,0,0,1
	[0.3,   [0.5, 0.25, 0.1],                  [0, 0, 3]],
	[0.25,  [0.5, 0.25, 0.1],                  [0, 1, 0]],
	[12,    [5, 4, 0.5],                       [0, 3, 0]],
	[12.3,  [5, 4, 0.5, 0.3, 0.2],             [0, 3, 0 1, 0]]
];

foreach( testIdx,test in tests )
{
	printf( "## Test %d: %.2f in [ ", testIdx, test[0] );
	foreach( x in test[1] )
		printf( "%.2f ", x );
	printf( "]: " );

	local res = compute_change( test[0], test[1] );

	local equal = function() {
		if( res.len() != test[2].len() )
			return false;

		foreach( i,x in test[2] )
		{
			if( x != res[i] )
				return false;
		}
		return true;
	}();

	if( equal ) printf( "OK\n" ); 
	else printf( "FAIL\n" );

	if( res )
	{
		local acc = 0.0;
		foreach( i,x in res )
		{
			if( x )
			{
				printf( "%d*%.2f ", x, test[1][i] );
				acc += x*test[1][i];
			}
		}
		printf( " = %.2f\n", acc );
	}
	else printf( "Not possible\n" );

	printf( "\n" );
}
```