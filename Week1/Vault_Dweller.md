# Solution for [Week 1](Index)
## Author: [Vault_Dweller](https://what.thedailywtf.com/user/Vault_Dweller)

<a name="TSQL"></a>
## Language: TSQL

```
declare @n as int = 20

if object_id('tempdb.dbo.#main') is not null
	drop table #main

--Generates the decimal numbers we will use (from 0 to n)
select number N
into #main
from master.dbo.spt_values
where type = 'P'
	and number <= @n

if object_id('tempdb.dbo.#bin') is not null
	drop table #bin

--Generates the sequence of 2^n that will be used to derive the binary representation of each decimal number
select number N,
	POWER(2, number) Pwr
into #bin
from master.dbo.spt_values
where type = 'P'
	and number <= @n - 1

--rec_cte (recursive cte), which generates the binary representation of each decimal number, with 1 digit per row (easier to calculate the "runs" this way)
;with rec_cte
as (
	select m.N, --The decimal number
		MAX(b.Pwr) Pwr, --2^n,
		m.N - MAX(b.pwr) Remain, --remainder after subtracting 2^n
		1 Bin, --the binary representation
		1 RunNo --The "run" number
	from #main m
		inner join #bin b on b.pwr <= m.N
	group by m.N

	union all

	select r.N,
		b.Pwr,
		case when r.Remain >= b.Pwr then r.Remain - b.Pwr else r.Remain end Remain,
		case when r.Remain >= b.Pwr then 1 else 0 end,
		case when r.Bin = (case when r.Remain >= b.Pwr then 1 else 0 end) then r.RunNo else r.RunNo + 1 end Bin
	from rec_cte r
		inner join #bin b on r.Pwr/2 = b.Pwr and r.Pwr >= 2
),

--Group the result by the run numbers (for 0's only), and return the values for which the lenght of the run number is odd
candidate_runs_cte
as (
	select  N,
		RunNo,
		COUNT(*) Cnt
	from rec_cte
	where Bin = 0
	group by N, RunNo
	having COUNT(*) % 2 = 1
),

--Calculate the Baum-Sweet number
final_cte
as (
	select m.N,
		1 BS_Nbr
	from #main m
	where not exists (select 1 from candidate_runs_cte c where m.N = c.N)

	union all

	select m.N,
		0 BS_Nbr
	from #main m
	where exists (select 1 from candidate_runs_cte c where m.N = c.N)
),

--Formatting
format_cte
as (
	select CAST(BS_Nbr as varchar(8000)) OutString, N
	from final_cte
	where N = 0

	union all

	select fm.OutString + ', ' + CAST(fn.BS_Nbr as varchar(8000)), fn.N
	from format_cte fm
		inner join final_cte fn on fm.N + 1 = fn.N
)


select OutString
from format_cte
where N = @n
```

## Challenge Output for n = 20:
```
1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0
```