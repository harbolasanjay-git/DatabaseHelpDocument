1.	 Overall, 5 different regions and different servers:
    1.	US
    2.	CA
    3.	UK
    4.	CN
    5.	MX

2.	US has two servers, the same as CA with replication to build connections.







3.	OLAP stands for Online Analytical Process.

4.	OLTP stands for Online Transaction Process.

5.	OLTP: US CORP/CA CORP

6.	OLAP: US DWS/CA DWS

7.	HYVE US PRODUK PROD/HYCN PROD/MX PROD are 4 independent servers for 4 different regions.

      a.	XD PROD is for xerox company.
      b.	They 5 don't have severs to be separated to 2 servers like US and CA. They only have 1 serunt named PROD
      c.	 But PROD still has both OLAP and OLTP processes.
      d.	 In PROD, OLTP also uses replication to build connection with OLAP.
      e.	You can understand that OLAP & OLTP (CORP and DWS) are in the same server named PROD.




US CORP
    1.	CORP is OLTP (Online Transaction Process).
    
    2.	CIS:
          a.	CIS is read-only for DWS.
          b.	CIS for CORP is from kind of business support system, which is not read only.

3.	HIS is archive for CIS.

4.	INT is a temporary database. Data will not change when the database server restarts.

5.	We have the other temporary database named tempah. The difference is that tables in tempdb will lose data or be dropped when server restarts.

Replication frequency
      1.	Real time for CIS.
      2.	Weekly for HIS.

US DWS
    1.	DWS is OLAP
    2.	CIS for DWS is read-only.
    3.	DW_PROD

a.	DW_PROD is its unique database.
b.	Data in DW_PROD is based on data of CIS through CRON (stored procedures and shell scripts)

c.	Data in DW_PROD can be modified.
4.	Data from CIS to DW PROD is Nightly loading.



Deployment Process
Rules to create table and Stored Procedure
1.	If the old SP is not formatted, the first step is to format old SP and submit to CVS. Then add new changes and submit again.

2.	Fill DMR information, which is only valid for the table.

3.	Deploy to UAT by One Tools.

4.	Deployment time window:
•	Monday, Tuesday, Thursday, the last week of each month cannot be deployed.
•	the 25th of each month (the billing day) is blackout.
Design and preparation
1.	Confirm the need to create a table/Business data or report data/if must create table on CIS.

2.	Code and test on DEV.

3.	Format code with Notepad++ and prepare DDL code.

4.	Fill DMR information.
One Tool Process
1.	Create DP (Deployment Plan) based on backlog or release.

2.	Create DO Submit tested code and apply.

3.	Integrity Review.

4.	DM Review.

5.	IT Leader Approve.

6.	QC and Exec Approve

7.	Deploy.

How to know data delay between primary DB and replicate DB?
Exec CIS..reptime;
Create table in STECH DB
1.	Judge before deleting objects
2.	Start with letter
3.	It is separated by "_" and consists of lowercase letters and numbers
4.	Table name cannot exceed 27 characters
5.	Create a fixed temporary table in INT
6.	Include timestamp (entry date) and operation source (entry id)
7.	In order to avoid the jump of identity column, the gap value should be set
Index name in table
8.	Index must be named by TablenameI[1,2,3]
9.	The first index uses business fields/field combinations that represent the uniqueness of the table as much as possible
10.	 Identity field can't be index field
11.	 Long strings (>50) cannot be used as keys for indexes.
12.	One table only has one clustered index.

