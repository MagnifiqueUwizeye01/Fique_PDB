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

![Final](https://github.com/user-attachments/assets/559f7de9-6e37-4b48-ab46-e978553db281)

### SQL COMMANDS SCREENSHOT



![03](https://github.com/user-attachments/assets/18e40f17-28c0-4323-9362-ae57aee886b0)
![08](https://github.com/user-attachments/assets/2f2e9c54-dc82-4135-a397-9c1368ad0d09)
![07](https://github.com/user-attachments/assets/f4263713-3e96-4bf3-adb1-07456439b863)
![05 2](https://github.com/user-attachments/assets/787b9d1c-ad5e-48cf-a956-3175b2b2befa)
![04](https://github.com/user-attachments/assets/4b97ffa2-e0ac-4113-bdbe-a83054370bc6)
![01](https://github.com/user-attachments/assets/6d6aee00-24e2-4278-81ec-b552ec5fe38d)
![02](https://github.com/user-attachments/assets/1145423b-cb85-4e01-8c3b-121a1c064a0d)


