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
# Sql
```
-- 
-- 

SELECT TOP 10 hn,fname AS 'ชื่อ',lname AS 'สกุล',
RTRIM(fname) + ' ' + RTRIM(lname) AS fullname,
ISNULL(LockProfile,'ไม่พบ') AS lock,
((2 + 3)/5) * 2 AS cal,*
FROM Patient
WHERE pname IN('003','004','001')
AND birthdate > '1989-01-05'

SELECT TOP 1000 O_AMOUNT,O_ItemPriceSr,
O_AMOUNT * O_ItemPriceSr AS total,
O_ItemPriceSr / ISNULL(O_AMOUNT, 1) AS unitPrice,

CASE WHEN O_ITemId IN(1998, 43282)
THEN 'มาก' ELSE 'น้อย' END AS checkPrice,

CASE O_ITemId 
WHEN 1998 THEN 'ค่าห้าสิบบาท' 
WHEN 43282 THEN 'XXA' 
WHEN 3212 THEN 'EEEE'
ELSE 'ไม่รู้ไม่รู้' END AS checko,
O_Orderdate,FORMAT(O_Orderdate, 'dd/MM/yyyy') AS dateA,
O_Orderdate,FORMAT(O_Orderdate, 'hh:mm:ss') AS dateA,
FORMAT(O_Orderdate, 'dd/MM/') + 
CONVERT(varchar(100),(SUBSTRING(FORMAT(O_Orderdate, 'yyyy-MM-dd'), 1, 4) + 543)) AS xx,
FORMAT(O_Orderdate, 'dd') as day,
FORMAT(O_Orderdate, 'MM') as month,
FORMAT(O_Orderdate, 'yyy') as year
FROM SR_OrderDetailM
WHERE O_Orderdate='2022-07-16' AND O_VSID=959321180;


SELECT vs.Vsid,vs.Vshn,diag.pticd,r.Torname,vs.VsopdR
FROM Visitopd AS vs
LEFT JOIN ptdiag10 AS diag ON(vs.Vsid=diag.ptVsid)
LEFT JOIN Topdroom AS r ON(r.Torid=vs.VsopdR)
WHERE vs.VSDate BETWEEN '2022-07-01' AND '2022-07-02'
AND diag.pticd IS NOT NULL AND r.Torname='อายุรกรรมทั่วไป'
AND Vshn=1158025
ORDER BY r.Torname ASC,vs.Vshn DESC;

SELECT
 STUFF(
 (
 SELECT ', ' + RTRIM(DCDiagIPD.DIPDDiag)
FROM DCDiagIPD 
INNER JOIN DischargeData ON DCDiagIPD.DIPDDCREC = DischargeData.DcRID
WHERE (DischargeData.DcADRID = 102658088) AND (DCDiagIPD.DIPDTYP > '1')
FOR XML PATH('')

), 1, 1, '') As SdxList

SELECT hn,sex,birthdate,
(SELECT TOP 1 VSDate FROM Visitopd WHERE Visitopd.vshn=p.hn) AS vsDateee
FROM Patient p
WHERE p.hn=10396

SELECT Vshn,VSDate,
(SELECT TOP 1 birthdate FROM Patient WHERE Patient.hn=vs.Vshn ORDER BY ) AS bdate
FROM Visitopd vs
WHERE vs.VShn=10396

SELECT    AdmitData.ADSection, WardSection.SWdesc,count(distinct AdmitData.ADRID) as 'referout',
CASE WHEN DatePart(Month, AdmitData.ADdate) >= 10 THEN DatePart(Year, Admitdata.ADdate) + 1 ELSE DatePart(Year, 
AdmitData.ADdate) END AS Fiscal_Year
FROM            AdmitData INNER JOIN
                         DischargeData ON AdmitData.ADRID = DischargeData.DcADRID INNER JOIN
                         Referout ON DischargeData.DcADRID = Referout.RFAdrid INNER JOIN
                         WardSection ON AdmitData.ADSection = WardSection.SWID
where AdmitData.ADdate BETWEEN '2016-10-01' AND '2021-09-30'
group by AdmitData.ADSection, WardSection.SWdesc,
CASE WHEN DatePart(Month, AdmitData.ADdate) >= 10 THEN DatePart(Year, Admitdata.ADdate) + 1 ELSE DatePart(Year, 
AdmitData.ADdate) END;

select * from (
SELECT    AdmitData.ADSection, WardSection.SWdesc,count(distinct AdmitData.ADRID) as 'referout',
CASE WHEN DatePart(Month, AdmitData.ADdate) >= 10 THEN DatePart(Year, Admitdata.ADdate) + 1 ELSE DatePart(Year, 
AdmitData.ADdate) END AS Fiscal_Year
FROM            AdmitData INNER JOIN
                         DischargeData ON AdmitData.ADRID = DischargeData.DcADRID INNER JOIN
                         Referout ON DischargeData.DcADRID = Referout.RFAdrid INNER JOIN
                         WardSection ON AdmitData.ADSection = WardSection.SWID
where AdmitData.ADdate BETWEEN '2016-10-01' AND '2021-09-30'
group by AdmitData.ADSection, WardSection.SWdesc,
CASE WHEN DatePart(Month, AdmitData.ADdate) >= 10 THEN DatePart(Year, Admitdata.ADdate) + 1 ELSE DatePart(Year, 
AdmitData.ADdate) END
) as t
pivot(sum(referout)
for Fiscal_Year in ([2017],[2018],[2019],[2020],[2021])
) as p1
order by ADSection





SELECT DISTINCT
SR_CurrentOrderM.C_OrderID AS orderID,Visitopd.Vsid AS visitID,
Vshn+FORMAT(Visitopd.VSDate, 'yyyyMMdd') AS visit_number,Visitopd.Vshn AS hn,
FORMAT(SR_OrderDetailM.O_Orderdate, 'yyyy-MM-dd') AS order_date,FORMAT(SR_OrderDetailM.O_Orderdate, 'yyyy-MM-dd')+' '+RTRIM(SR_OrderDetailM.O_OrderTime)+':00' AS order_time,
SR_OrderDetailM.O_ITemId AS itemID,InventoryItems.ItemCodeNHSO AS claim_item_code,
InventoryItems.Itemname AS item_desc,SR_OrderDetailM.O_AMOUNT AS amount,
SR_OrderDetailM.O_ItemPriceSr AS price,(SR_OrderDetailM.O_ItemPriceSr/SR_OrderDetailM.O_AMOUNT) AS unit_price,
ISNULL(InventoryGroup.inventoryCatID,'') AS type,ISNULL(InventorySubgroup.inventorySubgroupName,'') AS group_name,
'' AS use_text,'' AS nedReason,InventoryItems.Service_ID AS service_id
FROM Visitopd
INNER JOIN SR_CurrentOrderM ON(Visitopd.Vsid=SR_CurrentOrderM.C_VSID)
INNER JOIN SR_OrderDetailM ON(SR_OrderDetailM.O_VSID=SR_CurrentOrderM.C_VSID)
INNER JOIN SRIvenChartOrder ON(SR_CurrentOrderM.C_OrderID=SRIvenChartOrder.OrderID)
INNER JOIN InventoryItems ON(SR_OrderDetailM.O_ITemId=InventoryItems.itemsid)
LEFT JOIN InventorySubgroup ON(InventorySubgroup.inventorySubgroupID=InventoryItems.SubgroupID)
LEFT JOIN InventoryGroup ON(InventoryGroup.inventoryGroupID=InventorySubgroup.inventoryGroupID)
WHERE Visitopd.Vsid=809620675 

```
