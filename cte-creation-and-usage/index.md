# Intermediate Tables (CTE or Common Table Expressions)

Sometimes, during structure mapping, you may want to use an SQL query as a table.  
HyperUnison supports this by using CTEs. Once a CTE is created, you can use it as a table:  
- Browse CTEs alongside tables in the **Source Explorer**.  
- View the list of columns in the CTE.  
- Analyze the statistical distribution of values in the CTE.  

## Creating a CTE

To create a CTE, navigate to the **Intermediate Tables (CTE)** section in the structure mapping view and click the **[ADD TABLE]** button.  
In the creation form:  
- Provide a name for the CTE (it should contain only letters and numbers).  
- Enter the SQL query.  

Once the CTE is created, it will immediately appear in the **Intermediate Tables (CTE)** section of the **Source Explorer** on the right-hand side.  
The column scanning process will start automatically and may take some time, depending on the performance of the provided SQL query.

## Removing a CTE

To remove a CTE:  
1. Expand the CTE in the **Intermediate Tables (CTE)** section at the bottom of the middle screen.  
2. Click the **DELETE** button.  
