#SQL Server 
How to repair a SQL Server
```sql


USE master
GO
sp_configure 'allow updates', 1
GO
RECONFIGURE WITH OVERRIDE
GO

--Reset the suspect status:
EXEC sp_resetstatus [DATABASE_NAME];

--To run the integrity check, the database must be configured as a single user.
ALTER DATABASE [DATABASE_NAME] SET SINGLE_USER;

--In case it is not possible to remove it from suspect, it is possible to switch it to emergency mode.
Alter Database [DATABASE_NAME] Set Emergency

--If this does not work, the entire SQL Server instance may need to be restarted.
--We pass integrity check, to repair without data loss:
DBCC checkdb([DATABASE_NAME],REPAIR_REBUILD);

--To repair with possible loss of data:
DBCC checkdb(MyDB,REPAIR_ALLOW_DATA_LOSS);

--To return the database to multi-user mode:
ALTER DATABASE [DATABASE_NAME] SET MULTI_USER

--Disable the allow_updates option to put it back to the way it was before.
USE master
GO
sp_configure 'allow updates', 0
GO
RECONFIGURE WITH OVERRIDE
GO
```
