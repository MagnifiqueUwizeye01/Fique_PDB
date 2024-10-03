### ORACLE DATABASE MANAGMENT SQL COMMANDS FOR PLUGGABL DATABASE OPERATIONS

### This document provides a detailed step-by-step demonstration of using SQL commands in Oracle 21c Express Edition to manage pluggable databases (PDBs). It includes operations like creating, opening, modifying, and deleting pluggable databases, as well as managing users and privileges within them. The commands are executed in an Oracle Database environment using SQL*Plus with SYSDBA privileges, highlighting common tasks that database administrators might perform, such as:

Checking the instance and PDB status,
Creating new pluggable databases,
Opening and closing databases,
Managing user accounts and privileges,
Saving database states and handling database files and
Unplugging and dropping pluggable databases.

```SQL
Microsoft Windows [Version 10.0.19045.4894]
(c) Microsoft Corporation. All rights reserved.

C:\Users\User>sqlplus sys as SYSDBA

### Connect to the Oracle database as SYSDBA (superuser privileges)
SQL*Plus: Release 21.0.0.0.0 - Production on Thu Oct 3 13:03:36 2024
Version 21.3.0.0.0

Enter password:

Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> show user;
-- Shows the current user you are logged in as
USER is "SYS"

SQL> SELECT instance_name FROM v$instance;
-- Retrieves the current instance name
INSTANCE_NAME
----------------
xe

SQL> show ppdbs;
-- Invalid SHOW option (typo error or wrong command)
SP2-0158: unknown SHOW option "ppdbs"

SQL> show pdbs;
-- Shows pluggable databases (PDBs) and their status (open mode and restriction status)

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 XEPDB1                         READ WRITE NO

SQL> SELECT CON_ID, TABLESPACE_NAME, FILE_NAME
  2  FROM CDB_DATA_FILES
  3  WHERE CON_ID = 3;
-- Retrieves tablespace names and file paths for a specific container (PDB with CON_ID = 3)

    CON_ID TABLESPACE_NAME
---------- ------------------------------
FILE_NAME
--------------------------------------------------------------------------------
         3 SYSTEM
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\SYSTEM01.DBF

         3 SYSAUX
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\SYSAUX01.DBF

         3 UNDOTBS1
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\UNDOTBS01.DBF

         3 USERS
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\USERS01.DBF

SQL> SHOW PDBS;
-- Repeats the command to show pluggable databases (PDBs)

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 XEPDB1                         READ WRITE NO

SQL> create pluggable database plsql_class2024db
  2  admin user pdbadmin identified by admin
  3  file_name_convert=('C:\APP\USER\PRODUCT\21C\ORADATA\XE\pdbseed','C:\APP\USER\PRODUCT\21C\ORADATA\XE\plsql_class2024');
-- Creates a new pluggable database (PDB) named 'plsql_class2024db' with a new admin user and specific file name conversion

Pluggable database created.

SQL> alter session set container =plsql_class2024;
-- Error caused because the PDB name is incorrect (typo). Correct name is `plsql_class2024db`
ERROR:
ORA-65011: Pluggable database PLSQL_CLASS2024 does not exist.

SQL> alter session set container =plsql_class2024db;
-- Corrects the command by setting the session to the correct container (PDB)

Session altered.

SQL> alter pluggable database plsql_class2024db open;
-- Opens the newly created pluggable database

Pluggable database altered.

SQL> alter pluggable database plsql_class2024db save state;
-- Saves the state of the pluggable database to keep it open after the container is restarted

Pluggable database altered.

SQL> create user ma_plsqlauca identified by kabutare;
-- Creates a new user 'ma_plsqlauca' with the password 'kabutare'

User created.

SQL> grant all privileges to ma_plsqlauca;
-- Grants all system privileges to the newly created user

Grant succeeded.

SQL> show pdbs;
-- Shows the PDBs, including the newly created one 'plsql_class2024db'

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         4 PLSQL_CLASS2024DB              READ WRITE NO

SQL> show con_name;
-- Shows the name of the currently connected PDB (plsql_class2024db)

CON_NAME
------------------------------
PLSQL_CLASS2024DB

SQL> alter session set container =cdb$root;
-- Switches the session back to the root container (CDB$ROOT)

Session altered.

SQL> alter session set container =ma_to_delete_pdb;
-- Switches the session to a different PDB (ma_to_delete_pdb)

Session altered.

SQL> show pdbs;
-- Shows details of all PDBs, including the one to delete

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         6 MA_TO_DELETE_PDB               MOUNTED

SQL> alter session set container=ma_to_delete_pdb;
-- Confirms switching the session to 'ma_to_delete_pdb'

Session altered.

SQL> alter pluggable database ma_to_delete_pdb open;
-- Opens the pluggable database 'ma_to_delete_pdb'

Pluggable database altered.

SQL> alter pluggable database ma_to_delete_pdb save state;
-- Saves the state of 'ma_to_delete_pdb' to remain open after restart

Pluggable database altered.

SQL> show pdbs;
-- Displays the status of PDBs, including 'ma_to_delete_pdb'

 CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         6 MA_TO_DELETE_PDB               READ WRITE NO

SQL> alter session set container=cdb$root;
-- Switches back to the root container

Session altered.

SQL> alter pluggable database ma_to_delete_pdb close immediate;
-- Closes the pluggable database 'ma_to_delete_pdb' immediately

Pluggable database altered.

SQL> alter pluggable database ma_to_delete_pdb unplug into 'C:\app\User\product\21c\admin\XE\dpdump\ma_to_delete_pdb.xml';
-- Unplugs the pluggable database 'ma_to_delete_pdb' and exports its metadata to the specified XML file

Pluggable database altered.

SQL> drop pluggable database ma_to_delete_pdb;
-- Drops the pluggable database 'ma_to_delete_pdb' from the container

Pluggable database dropped.

```

![Final](https://github.com/user-attachments/assets/559f7de9-6e37-4b48-ab46-e978553db281)

### SQL COMMANDS SCREENSHOT



![03](https://github.com/user-attachments/assets/18e40f17-28c0-4323-9362-ae57aee886b0)
![08](https://github.com/user-attachments/assets/2f2e9c54-dc82-4135-a397-9c1368ad0d09)
![07](https://github.com/user-attachments/assets/f4263713-3e96-4bf3-adb1-07456439b863)
![05 2](https://github.com/user-attachments/assets/787b9d1c-ad5e-48cf-a956-3175b2b2befa)
![04](https://github.com/user-attachments/assets/4b97ffa2-e0ac-4113-bdbe-a83054370bc6)
![01](https://github.com/user-attachments/assets/6d6aee00-24e2-4278-81ec-b552ec5fe38d)
![02](https://github.com/user-attachments/assets/1145423b-cb85-4e01-8c3b-121a1c064a0d)


