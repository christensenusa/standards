# Coding Standards
Spaces instead of Tabs: configure your editor so when you hit tab, they are automatically replaced by FOUR spaces. This applies across all languages.
- Some editors do it automatically
- How to configure it in SSMS: Tools -> Options -> Text Editor -> Transact-SQL -> Tabs -> Insert spaces

## SQL
General information on good syntax can be found here: https://www.sqlstyle.guide/ . Below is recommend syntax so we can have consistent and easy to read queries. 
1.	Always use uppercase for reserved keywords 
    - Example: SELECT Amount=Column1+Amount2 FROM LongTableName AS Table1 WHERE … GROUP BY … HAVING … ORDER BY …
2.	Always use the full name for a table with brackets. For example [MAS_CH1].[dbo].[SO_SalesOrderDetail]
3.	If a query contains joins or where clauses: 
    a.	Each table should have an alias using an acronym from the table name. For example [MAS_CH1].[dbo].[SO_SalesOrderDetail] SOD
    b.	Every column should reference the table alias even if the column name is unique to 1 table. For example SOD.ItemCode
    c.	Each join should be on its own line
    d.	Each AND/OR should be on its own line, indented from the join/where
4.	Any tables and columns we create should use PascalCase with no spaces or underscores. For example DeliveryData
5.	Comments describe the purpose, logic, and usage of any parameters, variables, case statements, and complex joins
6.	Stored procedures and functions must have a comment at the beginning the describes the purpose and any complexities
7.	Conditions in joins should be avoided. In the example below, (b) is preferable.
    a. Example: SELECT * FROM A INNER JOIN B ON A.PK = B.PK AND A.PK > 10
    b. SELECT * FROM A INNER JOIN B ON A.PK = B.PK WHERE A.PK > 10
8.	Column listings in the select statement should be indented with each column on its own line with a comma at the beginning rather than end
9.	If a case statement has multiple conditions, each WHERE clause should be on its own line with an additional indentation
10.	Names should always be unique and not use a reserved key word
11.	Avoid abbreviations
12.	For aliases, use “Name=X” instead of “X AS Name” (although it’s not ANSI compliance, we feel that it is more readability)

## Data warehouse guidelines
1.	The WH1 server is for production database. Any databases, tables, functions, or procedures that are created should be used ongoing. If data is synched from another system then the data is expected to be up to date and match the source system. 
2.	The RDSTEST server is for testing and ad hoc usage purposes. Everyone has freedom to create databases and tables. If you need to test something or work with data for a one time purpose please use RDSTEST. RDSTEST is not backed up so databases should be set to simple recovery so the transaction log does not continually grow. 
    a. If you need to run a query with ad hoc data in RDSTEST that joins with data on WH1 then you can reference WH1 by putting [WH1\DATA] at the beginning of the table schema. For example SELECT * FROM [WH1\DATA].[MAS_CH1].[dbo].SO_SalesOrderDetail. 
    b. Never use data in RDSTEST to update/modify data in WH1. 
3.	Tableau reports published to http://reports should only use data on WH1 and should not reference data on RDSTEST
4.	Generally source databases will be exactly replicated to a database on the WH1 server with a name that aligns with the source system. 
    a. Tables, columns, and data should match the source system and be read only for most users. Custom created tables and manual data modification will generally not be allowed in these databases. 
    b. Depending on the mechanism of synch, indexes should be created when possible
    c. DM2 test company databases are not replicated to WH1. They are accessible as linked servers on WH1 that you can query. For example SELECT * FROM [DM2SQL].[MAS_TST].[dbo].[SO_SalesOrderDetail]
    d. Always use caution when using linked servers to a production system like DM2. Long running queries against a production system can cause the backend tables to be blocked, which may cause issues with normal functionality of the production system. To prevent blocking tables, use the WITH NOLOCK hint in your query (see https://www.mssqltips.com/sqlservertip/2470/understanding-the-sql-server-nolock-hint/)
    
5.	In the future we plan to build a common reporting database for the company that will have key customer data and transactional data. This will replace the InvoiceDetails, TransactionHistory, and LoadToLoad tables that drive many reports today. Please be aware of this change and minimize use of the existing tables. This will be built in a new database that has not yet been created. Once this is created the DM2 Prod database will be deleted. Please do not add any new objects to DM2 Prod. 
6.	Each company division will have its own database that can be used for building reporting functionality specific to that division. A database usually has a main owner. The owner is responsible for data quality, stored procedures and functions code quality, etc. When you need to modify a database, please contact with the database owner to make sure your changes are good. If a database has no owner, please talk to Quentin when you need to make changes. 
    a. The Commercial database is for Commercial specific reporting purposes. This database is owned by Jared Hardinger. New objects added to this database require review and approval by Jared.
    b. The Retail Fuels Prod database is for Retail Fuels specific reporting purposes and is owned by Kirk Bishop. 
    c. IntelliFuel Prod will be used for Common Carrier, in the future it is likely we will switch to a new onboard truck system and will then create a Common Carrier specific reporting database. 
7.	Customer applications will have their own database as needed. 
8.	Creation of new tables, stored procedures, or functions in WH1 require review and approval by either the database owner, Jared, Hung, or Quentin (via code review. The code review is done via DevOps repository at: https://christensenusa.visualstudio.com/ProductionDataWarehouse/_git/ProductionDataWarehouse)

## Python
- PEP8 https://www.python.org/dev/peps/pep-0008/
- class names are *PascalCase*
- variable names and function names are *snake_case*
- constant names are *UPPER_CASE*

## Javascript (analysis tool: ESLint)
- https://google.github.io/styleguide/jsguide.html

## Typescript (analysis tool: TSLint)
- https://adidas.github.io/contributing/typescript-coding-guidelines/

## HTML & CSS
- https://google.github.io/styleguide/htmlcssguide.html

## Visual C#
- https://blogs.msdn.microsoft.com/brada/2005/01/26/internal-coding-guidelines/
- class names and function names are *PascalCase*
- variable names are *camelCase*
- constant name are *UPPER_CASE*

## Git workflow
Gitflow from https://buddy.works/blog/5-types-of-git-workflows
https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow
![alt text](https://buddy.works/blog/images/gitflow.png "Git workflow")

## UnitTest framework
- C#: xUnit
- Python: pytest
- React: Jest

## README
Readme will use markdown (.md) format


