# MssqlFromCmdLine

This is a set of notes on how to run MSSQL from the command line.

Reference [this site](https://docs.microsoft.com/en-us/sql/ssms/scripting/sqlcmd-start-the-utility?view=sql-server-ver15) for more information

When you install SQLExpress it seems to install sqlcmd Utility by default.
This is the [link](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-ver15) for just sqlcmd Utility to be installed. 

I would assume if you have any instance of MSSQL server installed (for example the sql servers that come with Visual Studio's install), then the sqlcmd Utility should just work. Make sure to install the correct sqlcmd Utility tool for the appropriate MSSQL version you have installed.

#How to connect to specific server: 

Run in the cmd line: 
```
sqlcmd -Slocalhost\sqlexpress
```
If the connection worked, the prompt should change to ```1>```

##Note: When trying to run a sql script in the sqlcmd Utility, all commands are kept in a buffer. Add ```Go``` to the end of the script(s) you are trying to run or just make the next command ```Go```.

Example:

```
1>select name from master.sys.databases
2>Go

master
tempdb
model
msdb
example
```

#Run a sql script from cmd line and output result to file
```
sqlcmd -S (localdb)\mssqllocaldb  -d databaseToDelete -i C:\DatabaseBackUps\query.sql -o C:\DatabaseBackUps\delMe.txt
```

#Run a query that is kept within a field in a table
```
declare @Query nvarchar(100)
SET @Query = (SELECT query FROM queries WHERE id = 1);
EXECUTE sp_executesql @Query
```

#Useful [link}(http://www.sqlservertutorial.net/sql-server-stored-procedures/sql-server-cursor/) on cursors


The following script is used to run queries that exist in a table.
If the script is put into a sql file, then it can be called by cmdline and the results can be saved to a txt file.
for every query, there will be a chunk of the results in the output txt file.

```
--This is where the query kept in a field will be stored per run
DECLARE @Query nvarchar(100)

--this is the cursor where each query is run sequentially
DECLARE cursor_product CURSOR
	FOR SELECT 
			query
		FROM 
			queries;

open cursor_product

--Store the first query 
FETCH NEXT FROM cursor_product INTO 
    @Query 
 
WHILE @@FETCH_STATUS = 0
    BEGIN

		EXECUTE sp_executesql @Query
        FETCH NEXT FROM cursor_product INTO 
            @Query
    END;
	
CLOSE cursor_product;
DEALLOCATE cursor_product;
```



