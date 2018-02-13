# Solution for [Week 2](Challenge)
## Author: [Vault_Dweller](https://what.thedailywtf.com/user/Vault_Dweller)

<a name="TSQL"></a>
## Language: TSQL

Regular solution
```
set nocount on
declare @limit as int = 10000000

if OBJECT_ID('tempdb.dbo.#num') is not null
	drop table #num

select top(@limit-1) ROW_NUMBER() over (order by (select 1))+1 N
into #num
from sys.all_columns a, sys.all_columns b


declare @i as int = 2
while @i <= FLOOR(SQRT(@limit))
begin

	delete from #num
	where N > @i and N % @i = 0

	set @i = (select MIN(N) from #num where N > @i)
end

select SUM(N)
from #num
```

Bonus #2 solution:
```
declare @N as int = 1000

/*
Uncomment this to allow http requests. This only needs to be run once on the server
FRIENDLY WARNING: This is insecure and should probably not be done on your company's servers
*/

--EXEC sp_configure 'Ole Automation Procedures', 1;
--GO

--RECONFIGURE;
--GO

if object_id('tempdb.dbo.#out') is not null
	drop table #out

CREATE TABLE #out(html varchar(max))

--########################################################
--This part is shamelessly stolen from the interwebz
--########################################################
DECLARE @responseText NVARCHAR(2000);
DECLARE @ret INT;
DECLARE @status NVARCHAR(32);
DECLARE @statusText NVARCHAR(32);
DECLARE @token INT;
DECLARE @url NVARCHAR(256);

SET @url = 'http://www.primos.mat.br/primeiros_10000_primos.txt'

-- Open the connection.
EXEC @ret = sp_OACreate 'MSXML2.ServerXMLHTTP', @token OUT;
IF @ret <> 0 RAISERROR('Unable to open HTTP connection.', 10, 1);

-- Send the request.
EXEC @ret = sp_OAMethod @token, 'open', NULL, 'GET', @url, 'false';
EXEC @ret = sp_OAMethod @token, 'send'

-- Handle the response.
EXEC @ret = sp_OAGetProperty @token, 'status', @status OUT;
EXEC @ret = sp_OAGetProperty @token, 'statusText', @statusText OUT;

insert into #out
EXEC @ret = sp_OAGetProperty @token, 'responseText'

-- Close the connection.
EXEC @ret = sp_OADestroy @token;
IF @ret <> 0 RAISERROR('Unable to close HTTP connection.', 10, 1);

--########################################################
--End of stolen code
--########################################################

--Split response by newlines
;with newlines_cte
as (
	select LEFT(html,CHARINDEX(CHAR(13)+CHAR(10),html)-1) OutString, SUBSTRING(html,CHARINDEX(CHAR(13)+CHAR(10),html)+2,LEN(html)) Remainder
	from #out

	union all

	select LEFT(Remainder,CHARINDEX(CHAR(13)+CHAR(10),Remainder)-1) OutString, SUBSTRING(Remainder,CHARINDEX(CHAR(13)+CHAR(10),Remainder)+2,LEN(Remainder))
	from newlines_cte
	where CHARINDEX(CHAR(13)+CHAR(10),Remainder) > 0

	union all

	select Remainder OutString, NULL Remainder
	from newlines_cte
	where Remainder <> ''
		and CHARINDEX(CHAR(13)+CHAR(10),Remainder) = 0
),

--Split result by tabs
tabs_cte
as (
	select LEFT(OutString,CHARINDEX(CHAR(9),OutString)-1) N, SUBSTRING(OutString,CHARINDEX(CHAR(9),OutString)+1,LEN(OutString)) Remainder
	from newlines_cte

	union all

	select LEFT(Remainder,CHARINDEX(CHAR(9),Remainder)-1) N, SUBSTRING(Remainder,CHARINDEX(CHAR(9),Remainder)+1,LEN(Remainder)) Remainder
	from tabs_cte
	where CHARINDEX(CHAR(9),Remainder) > 0

	union all

	select Remainder N, NULL Remainer
	from tabs_cte
	where Remainder <> ''
		and CHARINDEX(CHAR(9),Remainder) = 0
)

select SUM(CAST(N as int))
from tabs_cte
where N <= @N
option (maxrecursion 0)
```