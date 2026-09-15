# Restore SQL Server 2017 `.bak` Backup to SQL Server 2022

## Environment

- Backup source: SQL Server 2017
- Destination database engine: SQL Server 2022 (`16.x`)
- Management tool: SQL Server Management Studio (SSMS) 2018
- Example source backup: `D:\Backup\DB-Name.bak`
- Example destination database: `DB-Name-New`

> SSMS 2018 can be used to connect to SQL Server 2022. The important part is connecting to the SQL Server 2022 instance, confirmed by version `16.x`.

---

## 1. Confirm the SQL Server version

Run this query in SSMS:

```sql
SELECT
    @@SERVERNAME AS ServerName,
    SERVERPROPERTY('ProductVersion') AS ProductVersion,
    SERVERPROPERTY('Edition') AS Edition;
```

Expected result for SQL Server 2022:

```text
16.x
```

Version reference:

| SQL Server | Major version |
|---|---:|
| SQL Server 2017 | 14.x |
| SQL Server 2019 | 15.x |
| SQL Server 2022 | 16.x |

---

## 2. Check the backup logical file names

Before restoring, run:

```sql
RESTORE FILELISTONLY
FROM DISK = 'D:\Backup\DB-Name.bak';
```

Replace the backup path with the actual `.bak` file path.

Example logical file names:

| LogicalName | Type |
|---|---|
| `DB-Name` | D |
| `DB-Name_log` | L |

Always use the actual logical names returned by `RESTORE FILELISTONLY`.

---

## 3. Recommended restore method

The earlier restore failed with:

```text
Operating system returned the error '5(Access is denied.)'
```

This happened because SQL Server could not create the `.mdf` and `.ldf` files in the target folder.

Create a writable folder, for example:

```text
C:\SQLData
```

Make sure the SQL Server 2022 Database Engine service account has permission to write to this folder.

Then run:

```sql
USE master;
GO

RESTORE DATABASE [DB-Name-New]
FROM DISK = 'D:\Backup\DB-Name.bak'
WITH
    MOVE 'DB-Name'
    TO 'C:\SQLData\DB-Name-New.mdf',

    MOVE 'DB-Name_log'
    TO 'C:\SQLData\DB-Name-New_log.ldf',

    RECOVERY,
    STATS = 10;
GO
```

### Change these values

Backup path:

```sql
'D:\Backup\DB-Name.bak'
```

Destination database:

```sql
[DB-Name-New]
```

Logical file names:

```sql
'DB-Name'
'DB-Name_log'
```

The logical names must match the result of `RESTORE FILELISTONLY`.

---

## 4. Restore and overwrite an existing database

Use this only when you intentionally want to replace the existing destination database.

> WARNING: `WITH REPLACE` overwrites the existing database. Any data in the existing database that is not included in the backup may be lost.

```sql
USE master;
GO

ALTER DATABASE [DB-Name-New]
SET SINGLE_USER
WITH ROLLBACK IMMEDIATE;
GO

RESTORE DATABASE [DB-Name-New]
FROM DISK = 'D:\Backup\DB-Name.bak'
WITH
    MOVE 'DB-Name'
    TO 'C:\SQLData\DB-Name-New.mdf',

    MOVE 'DB-Name_log'
    TO 'C:\SQLData\DB-Name-New_log.ldf',

    REPLACE,
    RECOVERY,
    STATS = 10;
GO

ALTER DATABASE [DB-Name-New]
SET MULTI_USER;
GO
```

---

## 5. Verify the restored database

Run:

```sql
SELECT
    name,
    state_desc,
    compatibility_level
FROM sys.databases
WHERE name = 'DB-Name-New';
```

Expected result:

```text
name         state_desc
-----------  ----------
DB-Name-New   ONLINE
```

---

## 6. Check the restored database files

Run:

```sql
USE [DB-Name-New];
GO

SELECT
    name AS LogicalName,
    physical_name,
    type_desc
FROM sys.database_files;
```

This confirms where the `.mdf` and `.ldf` files were restored.

---

## 7. Optional: Set SQL Server 2022 compatibility level

A SQL Server 2017 database may retain its older compatibility level after restoration.

Check it:

```sql
SELECT
    name,
    compatibility_level
FROM sys.databases
WHERE name = 'DB-Name-New';
```

SQL Server 2022 compatibility level is `160`.

After testing the application, you may set it to 160:

```sql
ALTER DATABASE [DB-Name-New]
SET COMPATIBILITY_LEVEL = 160;
```

Do not change the compatibility level blindly in production. Test the application first.

---

## 8. Find the SQL Server service account

If you receive Access Denied again, run:

```sql
SELECT
    servicename,
    service_account
FROM sys.dm_server_services;
```

Give the SQL Server Database Engine service account permission to the target folder, for example:

```text
C:\SQLData
```

Recommended permissions:

- Read & execute
- Write
- Modify

---

## Quick checklist

- [ ] Connect to SQL Server 2022, not SQL Server 2017
- [ ] Confirm `ProductVersion` starts with `16.`
- [ ] Confirm the `.bak` file path
- [ ] Run `RESTORE FILELISTONLY`
- [ ] Use the correct logical file names
- [ ] Create a writable restore folder
- [ ] Use `WITH MOVE`
- [ ] Use `WITH REPLACE` only when intentionally overwriting
- [ ] Verify the database state is `ONLINE`
- [ ] Test the application before changing compatibility level

---

## Common errors

### Error: Operating system error 5 — Access is denied

Cause: SQL Server does not have permission to create files in the target folder.

Fix:

1. Create `C:\SQLData`.
2. Give the SQL Server service account Modify permission.
3. Restore using `WITH MOVE`.

### Error: Database already exists

Cause: The destination database already exists.

Fix:

- Restore to a new database name, or
- Use `WITH REPLACE` after confirming that overwriting is safe.

### Error: File cannot be restored

Cause: The logical file names or target paths are incorrect.

Fix:

```sql
RESTORE FILELISTONLY
FROM DISK = 'D:\Backup\DB-Name.bak';
```

Then update the `MOVE` clauses with the exact logical names.

### Error: Wrong SQL Server version

Cause: SSMS is connected to SQL Server 2017 instead of SQL Server 2022.

Fix:

```sql
SELECT SERVERPROPERTY('ProductVersion');
```

SQL Server 2022 should return a version beginning with `16.`.
