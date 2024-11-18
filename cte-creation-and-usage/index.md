# Intermediate tables (CTE or Common Table Expressions)
Sometimes during structure mapping you may want to use an SQL query as a table
Hyperunison supports this using CTE. After CTE is created, you may use it as a table:
 - Browse CTE's along with tables in Source Explorer 
 - Browse lits of column in the CTE
 - Browse statistical distribution of values in the CTE

## Creating CTE

To create CTE to go "Intermediate tables (CTE)" section in the structure mapping and click button [ADD TABLE]
In the creating form provide the name of the CTE (it should contain only letters and numbers) and SQL query
After CTE is created you will see it immediately in the Source Explorer's section "Intermediate tables (CTE)" on the right hand side
Scanning of column will be started immediately at may continue for some time, depending on the performance of SQL query provided

## Removing CTE
To remove CTE expand the CTE in the "Intermediate tables (CTE)" section in the bottom of middle screen and click button "DELETE"
