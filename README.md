```SQL
Microsoft Windows [Version 10.0.19045.4894]
(c) Microsoft Corporation. All rights reserved.

C:\Users\User>sqlplus sys as SYSDBA

SQL*Plus: Release 21.0.0.0.0 - Production on Thu Oct 3 13:03:36 2024
Version 21.3.0.0.0

Copyright (c) 1982, 2021, Oracle.  All rights reserved.

Enter password:

Connected to:
Oracle Database 21c Express Edition Release 21.0.0.0.0 - Production
Version 21.3.0.0.0

SQL> show user;
USER is "SYS"
SQL> SELECT instance_name FROM v$instance;

INSTANCE_NAME
----------------
xe

SQL> show ppdbs;
SP2-0158: unknown SHOW option "ppdbs"
SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 XEPDB1                         READ WRITE NO
SQL> SELECT CON_ID, TABLESPACE_NAME, FILE_NAME
  2  FROM CDB_DATA_FILES
  3  WHERE CON_ID = 3;

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


    CON_ID TABLESPACE_NAME
---------- ------------------------------
FILE_NAME
--------------------------------------------------------------------------------
         3 USERS
C:\APP\USER\PRODUCT\21C\ORADATA\XE\XEPDB1\USERS01.DBF


SQL> SHOW PDBS;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 XEPDB1                         READ WRITE NO

SQL> create pluggable database plsql_class2024db
  2  admin user pdbadmin identified by admin
  3  file_name_convert=('C:\APP\USER\PRODUCT\21C\ORADATA\XE\pdbseed','C:\APP\USER\PRODUCT\21C\ORADATA\XE\plsql_class2024');

Pluggable database created.

SQL> alter session set container =plsql_class2024;
ERROR:
ORA-65011: Pluggable database PLSQL_CLASS2024 does not exist.


SQL> alter session set container =plsql_class2024db;

Session altered.

SQL> alter pluggable database plsql_class2024db open;

Pluggable database altered.

SQL> alter pluggable database plsql_class2024db save state;

Pluggable database altered.

SQL> create user ma_plsqlauca identified by kabutare;

User created.

SQL> grant all privileges to ma_plsqlauca;

Grant succeeded.

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         4 PLSQL_CLASS2024DB              READ WRITE NO
SQL> show con_name;

CON_NAME
------------------------------
PLSQL_CLASS2024DB
SQL> alter session set container =cdb$root;

Session altered.

Pluggable database created.

SQL> alter session set container =ma_to_delete_pdb;

Session altered.

SQL> show pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         6 MA_TO_DELETE_PDB               MOUNTED
SQL> alter session set container=ma_to_delete_pdb;

Session altered.

SQL> alter pluggable database ma_to_delete_pdb open;

Pluggable database altered.

SQL> alter pluggable database ma_to_delete_pdb save state;

Pluggable database altered.

SQL> show pdbs;

 CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         6 MA_TO_DELETE_PDB               READ WRITE NO
SQL> alter session set container=cdb$root;

Session altered.

SQL> alter pluggable database ma_to_delete_pdb close immediate;

Pluggable database altered.

SQL> alter pluggable database ma_to_delete_pdb unplug into 'C:\app\User\product\21c\admin\XE\dpdump\ma_to_delete_pdb.xml';

Pluggable database altered.

SQL> drop pluggable database ma_to_delete_pdb;

Pluggable database dropped.

```
