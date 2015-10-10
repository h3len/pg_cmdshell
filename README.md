pg_cmdshell
===========

PostgreSQL Windows extension for executing shell commands and return the results as rows.

It's behaviour is similar to xp_cmdshell on Microsoft SQL Server.

i.e. ```select pg_cmdshell('tasklist.exe')``` will return the list of running tasks.

Build
-----
It can be build with msbuild with the following cmd :
- ```msbuild /p:Configuration=Release /p:Platform="Win32"``` for 32bits
- ```msbuild /p:Configuration=Release /p:Platform="x64"``` for 64bits

Before building it, you might want to change the output path as they are
- ```C:\Program Files (x86)\PostgreSQL\9.5\lib\``` for ```Win32``` configuration 
- ```C:\Program Files\PostgreSQL\9.4\lib\``` for ```x64``` configuration
*Depending on your output locations, you might need to be in administrator mode to build.*

Installation
------------
The pg_cmdshell.dll must be placed in the ```lib\``` folder of your postgresql installation.

Once it's done, you can create a function in any database that needs it with :
```
CREATE OR REPLACE FUNCTION pg_cmdshell(text)   
RETURNS SETOF text AS
'pg_cmdshell', 'pg_cmdshell' LANGUAGE c ;
REVOKE ALL ON FUNCTION public.pg_cmdshell(text) FROM public;
```

#Warning
**Restrict usage of this function to very specific scenarios and only some specific users!**
**The commands used with this function are executed under the account running postgresql.**
**Besides the security limitation of that account, there are no filters in place to prevent you from doing anything with it.**

**A malicious user could do damages to your server if it can gain control of it.**


----
Tested with PostgreSQL 9.4 and 9.5b
