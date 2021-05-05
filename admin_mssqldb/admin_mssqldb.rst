.. _admin_mssqldb:

--------------------------------
Database Administration with Era
--------------------------------

We will now see how to perform normal database admin task with Era.

In this lab, you will administer your Microsoft SQL DB.

Explore Your Database
++++++++++++++++++++++

#. In **Era**, select **Databases** from the drop-down menu, and then **Sources** from the left-hand menu.

   .. figure:: images/1.png

#. Click **+ Register > Microsoft SQL Server > Database** and fill out the following fields:

   - **Database is on a Server VM that is:** - Registered
   - **Registered Database Servers** - *USERXX*\ **-MSSQLSourceVM**

   .. figure:: images/2.png

#. Click **Next**

   - **Unregistered Databases** - SampleDB
   - **Database Name in Era** - *USERXX*\ -LABSQLDB

   .. figure:: images/3.png

#. Click **Next**.

   - **Recovery Model** - Full
   - **Manage Log Backups with** - Era
   - **Name** - *USERXX*\ -LABSSQLDB_TM
   - **SLA** - DEFAULT_OOB_BRASS_SLA (no continuous replay)

   .. figure:: images/4.png

#. Click **Register**.

#. Select **Operations** from the drop-down menu to monitor the progress. This process should take approximately 3-5 minutes.

   .. figure:: images/4a.png

Snapshot Your Database
++++++++++++++++++++++

Before we take a manual snapshot of our database, lets write a new table into SampleDB.

Write New Table Into Database
.............................

#. RDP/Console into your *USERXX*\ **-MSSQLSourceVM**.

#. Open SQL Server Managment Studio (SSMS), and **Connect** using Windows Authentication.

#. Right-Click on **SampleDB** and Select **New Query**.

   .. figure:: images/5.png

#. **Execute** the following SQL Query:

   .. code-block:: Bash

      select * into dbo.testlabtable from sales.orders;

   .. figure:: images/6.png

#. Verify the new table is there by doing a refresh on **Tables**.

Take Manual Snapshot of Database
................................

#. In **Era**, select **Databases** from the drop-down menu, and then **Sources** from the left-hand menu.

#. Click on the Time Machine for your Database (ex. *USERXX*\ -LABSQLDB_TM).

   .. figure:: images/7.png

#. Click **Actions > Snapshot**.

   .. figure:: images/8.png

   - **Snapshot Name** - *USERXX*\ -MSSQL-1st-Snapshot

   .. figure:: images/9.png

#. Click **Create**

#. Select **Operations** from the drop-down menu to monitor the registration. This process should take approximately 2-5 minutes.

   .. figure:: images/9a.png

Clone Your Database Server & Database
+++++++++++++++++++++++++++++++++++++

#. In **Era**, select **Time Machines** from the drop-down menu, and then select *USERXX*\ -LABSQLDB_TM.

#. Click **Actions > Create Database Clone > Database**.

   - **Snapshot** - *USERXX*\ -MSSQL-1st-Snapshot (Date Time)

   .. figure:: images/10.png

#. Click **Next**.

   - **Database Server** - Create New Server
   - **Database Server Name** - *USERXX*\ -MSSQL_Clone1
   - **Compute Profile** - DEFAULT_OOB_COMPUTE
   - **Network Profile** - Primary-MSSQL-Network
   - **Administrator Password** - Nutanix/4u

   .. figure:: images/11.png

   - **Clone Name** - *USERXX*\ -LABSQLDB_Clone1
   - **Database Name on VM** - SampleDB_Clone1
   - **Instance Name** - MSSQLSERVER

   .. figure:: images/12.png

#. Click **Clone**

#. Select **Operations** from the drop-down menu to monitor the progress. This process should take approximately 10-15 minutes.

Delete Table and Clone Refresh
++++++++++++++++++++++++++++++

There are times when a table or other data gets deleted (by accident), and you would like to get it back. Here we will delete a table and use the Era Clone Refresh action from the last snapshot we took.

Delete Table
............

#. RDP/Console into your *USERXX*\ -MSSQL_Clone1 VM

#. Open SQL Server Managment Studio (SSMS), and **Connect** using Windows Authentication.

#. Expand **SampleDB_Clone1 > Tables**.

#. Right-Click on **dbo.testlabtable**, select **Delete**, and then **OK**.

Clone Refresh
.............

#. In **Era**, select **Databases** from the drop-down menu, and then **Clones** from the left-hand menu.

#. Select the clone for your database *USERXX*\ -LABSQLDB_Clone1 and Click **Refresh**.

   - **Snapshot** - *USERXX*\ -MSSQL-1st-Snapshot (Date Time)

#. Click **Refresh**

#. Select **Operations** from the drop-down menu to monitor the registration. This process should take approximately 2-5 minutes.

   .. figure:: images/13.png

Verify Table is Back
....................

#. RDP/Console into your *USERXX*\ -MSSQL_Clone1 VM

#. Open SQL Server Managment Studio (SSMS), and **Connect** using Windows Authentication.

#. Expand **SampleDB_Clone1 > Tables**.

#. Perform a refresh on **Tables**.

#. Verify **dbo.testlabtable** is there.
