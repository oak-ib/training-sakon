# MS SQL
Install sql studio 
Connect hms

sql management studio ???
```
SELECT
AS
CONCAT (CONVERT(varchar(10),LabNumber))
LIMIT (TOP)
ISNULL(QNUMBER,0)
Operator +, -, *, /
RTRIM
CASE WHEN THEN ELSE END
	- CASE WHEN aaa IN('2','3') THEN 

FORMAT(VSDate, 'yyyy-MM-dd') AS visitDate
GETDATE()
FORMAT(GETDATE(), 'yyyy-MM-dd')

SUBSTRING start index 1


WHERE AND OR IN LIKE BETWEEN
แปลงค่าก่อน where

SELELCT DISTINCT
GROUP BY
COUNT
MAX, MIN

UNION
SUB QUERY
```

```
SELECT vs.*,diag.*
FROM Visitopd AS vs
LEFT JOIN ptdiag10 AS diag ON(vs.Vsid=diag.ptVsid)
WHERE vs.VSDate 
```
