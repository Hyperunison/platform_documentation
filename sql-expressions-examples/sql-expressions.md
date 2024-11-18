# How to write Unison-SQL scripts

## Examples for SQL joins

To write SQL join between two tables you need to select tables that you want to join. Access to left and right tables is provided through variables $table1 и $table2. Use it to get access to the table and select concrete fields of these tables. 

Simple example of JOIN

```
$table1.id = $table2.patient
```

Advanced example of JOIN

```
$table1.id = $table2.patient AND $table2.type='Patient'
```

By default Unison platform uses only INNER JOIN.

## 

## Examples for Data Extraction Rules (DER)

You should select SQL Join as a linked table. Then you can use $table variable to get access to the fields of chosen joined tables. 

Simple example of Data Extraction Rule

```
$table.id
```

Advanced example of Data Extraction Rule

```
(
    SELECT CASE
    WHEN $table.gender = ‘M’ THEN ‘Male’
    WHEN $table.gender = ‘F’ THEN ‘Female’
    else $table.gender
    END
)
```

*PostgreSQL only*

Constant extraction example  
If you want to use string constant, write it in single quotes, like ‘ETH’. For number constant or null, write number without quotes or null

```
'ETH'
```

Table data 

Data type transformation example

```
$table.birthdate::timestamp
```

*PostgreSQL only*

## 

## Examples for CTE

By writing CTE you can use full functionality of SQL to select data and create intermediate tables for any transformation goals. Be careful if you use this CTE with JOINS and then with Data Extraction Rules: Unison does not store this table at the database. It is just a virtual table that will be created every time you try to query data.  

Simple example of CTE

```
select
   row_number() over() as id_int,
   tt.patient,
   min(tt.started_at::date) as observation_period_start_date,
   max(tt.started_at::date) as observation_period_end_date
from allergies.encounters tt
group by patient
```

## 

## FAQ and tricks:

### Why can't I use a plain table name in JOINs or data extraction rules?

For example, let's review the situation with conditions. User may write UQL (Unison query language) query to select participants with 2 diagnoses: first and second

Unison platform will generate 2 JOINs of tables with diagnoses in this case. And Unison will generate 2 uniq aliases in each join and replace $table with alias in each case. 

The same situation with $table1 and $table2 and other entities. User may query patients with 2 different measurements, so they should have unique aliases

### ---

### How to use complex expressions?

Users may write any complex UQL expression using any column of the table ($table) in SQL syntax used in Biobank’s database engine. 

In examples below examples for PostgreSQL are shown:

* Concatenation: $table.name || $table2.description  
* Type casting: $table1.date::date  
* Year/Month/day extraction: (EXTRACT (‘month’ FROM $table.date\_field::date))::int

---

### How to convert string identifiers to int?

You can use the default translator from string to integer at the Unison platform. Translator is available at the Data Extraction Rule definitions section. This transformation works with unique strings and converts them to unique numeric values.

Manually this task may be solved by using sorting all string IDs and enumerating them. To do this we should create a lookup table to translate each string ID to int ID. After this we should join the lookup table and translate string to int.

**Example**: let's assume we have table **persons** with a *string* ID field named **UUID** varchar(64). To create lookup table we should write SQL (example with PostgreSQL) 

```
SELECT row_number() over (order by uuid) as id_int, uuid from persons
```

This query will return a 2-columns table: uuid and id\_int. So it’s actually lookup table

To join in we should create JOIN with these settings:

- $table1 \= `persons`  
- $table2 \= `(SELECT row_number() over (order by uuid) as id_int, uuid from persons)`  
- ON expression: `$table1.uuid = $table2.uuid`

In data extraction rule we should choose created join in “table joined” field and use SQL expression `$table.id_int`

---

### How to generate what?

❗This example based on tutorial Allergy dataset. You can add to your account and look at all the code directly at the Unison Platform. 

location\_id field from 2 tables: allergies.patients and allergies.organizations with linked table location.id

This example looks like the previous one with one exception: we should enumerate the results of 2 source tables: organisation and patients.

To make end-to-end enumeration, we should UNION 2 sources tables and generate enumeration for the result. After this we will use result with enumeration as lookup table to translate patient.id or organization.id to location.id

As **patient.id** and **organization.id** may intersect, let's add additional field to distinguish them:

```
(
   select row_number() over (order by id, type) as id_int, type, id from (
       select id, ‘persons’ as type from allergies.patients
       union select id, ‘org’ from allergies.organizations
   ) as tmp
)
```

Result of this query: 3-columns table: 

- end-to-end enumeration  
- type (persons or organisations)  
- ID (persons or organisations)

After we should use this query in 3 joins:

* To translate patient.id to location\_id  
* To translate organization.id to location\_id  
* To generate location.id by data from patients  
* To generate location.id by data from organisations

### How to unpivot list of columns, for example: diag1, diag2, diag3, …, diag20

Sometimes in databases diagnoses, analyses or other entities may be presented not as rows, but as columns. But as OMOP requires storing them as rows, we should convert the table. In the example above we should convert one row with 20 diagnoses to 20 rows. To do this we should create another temporary table with 20 rows and join it. So we will multiply each row in the source table to 20 rows. After this we should create another column with content from first, second, third and so on column in first, second, third and so on rows accordingly. Lets do it\!  
Let's assume a table with 20 diagnose columns is called hes\_apc. 

1. Create a join. In PostgreSQL we may use the function generate\_series(1, 20\) to generate a table with 1 column and 20 rows. So:  
   Name \= diagnoses\_in\_rows  
   $table1 \= `hes_apc`  
   $table2 \= `(generate_series(1, 20) as diag_num)`  
   On \= `true`  
2. Create extraction rule for condition\_occurrence.condition\_concept\_id:  
   Table joined=diagnoses\_in\_rows  
   SQL expression \= `(ARRAY[$table1.diag1, $table1.diag2, $table1.diag3, …, $table1.diag20])[$table2.diag_num]`

In this case we create an array with all 20 diagnoses, and then take N’s element from the array. So:

* Each source rows will be multiplied by 20 times, as we joined in with a table with 20 rows without any condition  
* We created an expression with the array of 20 elements and took N’s element from the array in N’s row. So the first row will contain diag1, second \- diag2, …

