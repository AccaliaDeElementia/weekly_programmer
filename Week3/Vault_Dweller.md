# Solution for [Week 3](./)
## Author: [Vault_Dweller](https://what.thedailywtf.com/user/Vault_Dweller)

<a name="TSQL"></a>
## Language: TSQL

```
--#####################################
--Input
--#####################################
declare @coins as varchar(50) = '5 2 .5 .25 .1 .05'
declare @change as decimal(18,2) = '16.01'

--#####################################
--Split the coin values into seperate columns
--#####################################
if OBJECT_ID('tempdb.dbo.#coins') is not null
	drop table #coins

if OBJECT_ID('tempdb.dbo.#split') is not null
	drop table #split

create table #coins (
	Coin1 decimal(18,2),
	Coin2 decimal(18,2),
	Coin3 decimal(18,2),
	Coin4 decimal(18,2),
	Coin5 decimal(18,2),
	Coin6 decimal(18,2),
	Coin7 decimal(18,2),
	Coin8 decimal(18,2),
	Coin9 decimal(18,2),
	Coin10 decimal(18,2)
)

;with split_cte
as (
	select LEFT(@coins,CHARINDEX(' ',@coins)-1) Val, SUBSTRING(@coins,CHARINDEX(' ',@coins)+1,999) Remain, 1 Seq
	where CHARINDEX(' ',@coins) > 0

	union all

	select @coins, NULL, 1 Seq
	where CHARINDEX(' ',@coins) = 0

	union all

	select LEFT(Remain,CHARINDEX(' ',Remain)-1), SUBSTRING(Remain,CHARINDEX(' ',Remain)+1,999), Seq + 1
	from split_cte
	where CHARINDEX(' ',Remain) > 0

	union all

	select Remain, NULL, Seq + 1
	from split_cte
	where CHARINDEX(' ',Remain) = 0
)

insert into #coins(Coin1, Coin2, Coin3, Coin4, Coin5, Coin6, Coin7, Coin8, Coin9, Coin10) 
select MAX(case when Seq = 1 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 2 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 3 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 4 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 5 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 6 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 7 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 8 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 9 then CAST(Val as decimal(18,2)) else 0.0 end),
	MAX(case when Seq = 10 then CAST(Val as decimal(18,2)) else 0.0 end)
from split_cte

--#####################################
--Try to find an exact match
--#####################################
if OBJECT_ID('tempdb.dbo.#tally') is not null
	drop table #tally

select CAST(number as int) N
into #tally
from master.dbo.spt_values
where type = 'P'
	and number between 0 and 10

if OBJECT_ID('tempdb.dbo.#possible') is not null
	drop table #possible

select c.*, t1.N N1, t2.N N2, t3.N N3, t4.N N4, t5.N N5, t6.N N6, t7.N N7, t8.N N8, t9.N N9, t10.N N10, ISNULL(t1.N,0)
	+ ISNULL(t2.N,0)
	+ ISNULL(t3.N,0)
	+ ISNULL(t4.N,0)
	+ ISNULL(t5.N,0)
	+ ISNULL(t6.N,0)
	+ ISNULL(t7.N,0)
	+ ISNULL(t8.N,0)
	+ ISNULL(t9.N,0)
	+ ISNULL(t10.N,0) CoinCnt
into #possible
from #coins c
	left outer join #tally t1 on t1.N * Coin1 <= @change and Coin1 > 0
	left outer join #tally t2 on t2.N * Coin2 <= @change and Coin2 > 0
	left outer join #tally t3 on t3.N * Coin3 <= @change and Coin3 > 0
	left outer join #tally t4 on t4.N * Coin4 <= @change and Coin4 > 0
	left outer join #tally t5 on t5.N * Coin5 <= @change and Coin5 > 0
	left outer join #tally t6 on t6.N * Coin6 <= @change and Coin6 > 0
	left outer join #tally t7 on t7.N * Coin7 <= @change and Coin7 > 0
	left outer join #tally t8 on t8.N * Coin8 <= @change and Coin8 > 0
	left outer join #tally t9 on t9.N * Coin9 <= @change and Coin9 > 0
	left outer join #tally t10 on t10.N * Coin10 <= @change and Coin10> 0
where ISNULL(t1.N,0) * Coin1
	+ ISNULL(t2.N,0) * Coin2
	+ ISNULL(t3.N,0) * Coin3
	+ ISNULL(t4.N,0) * Coin4
	+ ISNULL(t5.N,0) * Coin5
	+ ISNULL(t6.N,0) * Coin6
	+ ISNULL(t7.N,0) * Coin7
	+ ISNULL(t8.N,0) * Coin8
	+ ISNULL(t9.N,0) * Coin9
	+ ISNULL(t10.N,0) * Coin10 = @change

if OBJECT_ID('tempdb.dbo.#winner') is not null
	drop table #winner

select top 1 p.*
into #winner
from #possible p
	inner join (select MIN(CoinCnt) CoinCnt from #possible) p2 on p.CoinCnt = p2.CoinCnt
	
--#####################################
--If no match found, find closest
--#####################################
if OBJECT_ID('tempdb.dbo.#possible2') is not null
	drop table #possible2

if (select COUNT(*) from #winner) = 0
begin

	select c.*, t1.N N1, t2.N N2, t3.N N3, t4.N N4, t5.N N5, t6.N N6, t7.N N7, t8.N N8, t9.N N9, t10.N N10, ISNULL(t1.N,0)
		+ ISNULL(t2.N,0)
		+ ISNULL(t3.N,0)
		+ ISNULL(t4.N,0)
		+ ISNULL(t5.N,0)
		+ ISNULL(t6.N,0)
		+ ISNULL(t7.N,0)
		+ ISNULL(t8.N,0)
		+ ISNULL(t9.N,0)
		+ ISNULL(t10.N,0) CoinCnt,
		ISNULL(t1.N,0) * Coin1
		+ ISNULL(t2.N,0) * Coin2
		+ ISNULL(t3.N,0) * Coin3
		+ ISNULL(t4.N,0) * Coin4
		+ ISNULL(t5.N,0) * Coin5
		+ ISNULL(t6.N,0) * Coin6
		+ ISNULL(t7.N,0) * Coin7
		+ ISNULL(t8.N,0) * Coin8
		+ ISNULL(t9.N,0) * Coin9
		+ ISNULL(t10.N,0) * Coin10 Change
	into #possible2
	from #coins c
		left outer join #tally t1 on t1.N * Coin1 <= @change and Coin1 > 0
		left outer join #tally t2 on t2.N * Coin2 <= @change and Coin2 > 0
		left outer join #tally t3 on t3.N * Coin3 <= @change and Coin3 > 0
		left outer join #tally t4 on t4.N * Coin4 <= @change and Coin4 > 0
		left outer join #tally t5 on t5.N * Coin5 <= @change and Coin5 > 0
		left outer join #tally t6 on t6.N * Coin6 <= @change and Coin6 > 0
		left outer join #tally t7 on t7.N * Coin7 <= @change and Coin7 > 0
		left outer join #tally t8 on t8.N * Coin8 <= @change and Coin8 > 0
		left outer join #tally t9 on t9.N * Coin9 <= @change and Coin9 > 0
		left outer join #tally t10 on t10.N * Coin10 <= @change and Coin10 > 0
	where ISNULL(t1.N,0) * Coin1
		+ ISNULL(t2.N,0) * Coin2
		+ ISNULL(t3.N,0) * Coin3
		+ ISNULL(t4.N,0) * Coin4
		+ ISNULL(t5.N,0) * Coin5
		+ ISNULL(t6.N,0) * Coin6
		+ ISNULL(t7.N,0) * Coin7
		+ ISNULL(t8.N,0) * Coin8
		+ ISNULL(t9.N,0) * Coin9
		+ ISNULL(t10.N,0) * Coin10 >= @change

	declare @newChange as decimal(18,2)
	select @newChange = MIN(Change) from #possible2

	insert into #winner(Coin1,Coin2,Coin3,Coin4,Coin5,Coin6,Coin7,Coin8,Coin9,Coin10,N1,N2,N3,N4,N5,N6,N7,N8,N9,N10,CoinCnt)
	select top 1 p.Coin1,p.Coin2,p.Coin3,p.Coin4,p.Coin5,p.Coin6,p.Coin7,p.Coin8,p.Coin9,p.Coin10,p.N1,p.N2,p.N3,p.N4,p.N5,p.N6,p.N7,p.N8,p.N9,p.N10,p.CoinCnt
	from #possible2 p
		inner join (select MIN(CoinCnt) CoinCnt from #possible2 where Change = @newChange) p2 on p.CoinCnt = p2.CoinCnt
	where p.Change = @newChange

end

--#####################################
--Output the result into correct format
--#####################################
;with process_cte
as (
	select Coin1 Coin
	from #winner
		inner join #tally t on t.N between 1 and N1

	union all

	select Coin2
	from #winner
		inner join #tally t on t.N between 1 and N2

		union all

	select Coin3
	from #winner
		inner join #tally t on t.N between 1 and N3

		union all

	select Coin4
	from #winner
		inner join #tally t on t.N between 1 and N4

		union all

	select Coin5
	from #winner
		inner join #tally t on t.N between 1 and N5

		union all

	select Coin6
	from #winner
		inner join #tally t on t.N between 1 and N6

		union all

	select Coin7
	from #winner
		inner join #tally t on t.N between 1 and N7

		union all

	select Coin8
	from #winner
		inner join #tally t on t.N between 1 and N8

		union all

	select Coin9
	from #winner
		inner join #tally t on t.N between 1 and N9

	union all

	select Coin10
	from #winner
		inner join #tally t on t.N between 1 and N10
),

seq_cte
as (
	select *, ROW_NUMBER() over (order by Coin desc) Seq
	from process_cte
),

final_cte
as (
	select CAST(Coin as varchar(8000)) OutString, Seq
	from seq_cte
	where Seq = 1

	union all

	select OutString + ' ' + CAST(Coin as varchar(8000)), s.Seq
	from final_cte f
		inner join seq_cte s on f.Seq + 1 = s.Seq
)

select MAX(OutString) OutString
from final_cte
```