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






