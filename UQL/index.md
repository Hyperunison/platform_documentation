# Unison Common Data Model - Documentation

## Overview
### What is the purpose of the UCDM?
The UCDM is an abstracted language to define cohort of patients in secure way. This cohort definition may be applied to any biobank or data structure in future
UCDM may be built based on any CDM and applied to any biobank. This may be done using mapping process, where we define relation between choosen CDM and physical data structure in the biobank

### What is the philosophy behind the UCDM?

Imagine how nice it would be if you could use one SQL query to access data from several biobanks at the same time. Get a lot of patient data in a single format, and analyze them

But when implementing this, we come across the following problems:
1. The data in biobanks are in different formats, diseases are called differently and the data in general may be poorly structured
2. Biobanks do not allow you to take data out of their network. And how can they be combined if the data cannot be copied to one centralized location? And the SQL query language does not allow you to limit the data identifying the patient. Accordingly, biobanks do not allow to have a direct SQL interface for external researchers

The first problem, although difficult, has a solution: the ETL process, data standardization (for example, OMOP or sentinel).
And the second problem currently has only a limited solution: you can copy data from one biobank to another friendly biobank (for example, from GEL and UKBB) and analyze them there. But how to combine data between countries? When there can be no question of any copying?

If SQL cannot be used, you want to have another query language that is as flexible as SQL, but at the same time restricts the transfer of patient-identifying data

To do this, first let's define what "patient identification data" is. Let it be any data set (year of birth +diagnosis +gender+...) that is available to a small number of patients in this particular biobank. Moreover, the biobank itself can determine this "small number". For Genomics England, this number is 7

Then any SQL queries of the following type will never export the patient's personal data, and we can freely export the result of such a query to the outside:
```SQL
SELECT COUNT(*), field1, field2, field3, ...
FROM patient
JOIN ...
WHERE ...
HAVING COUNT(*) > N
Where field1, field2, field3, ... - any data about the patient: gender, age, diagnosis, blood test result, etc.
```

Let me explain the query: we get the number of patients for each combination the value of field1, field2, field3, ..., provided that this number is greater than N. That is, provided that these data do not identify any patients

It remains for a small matter to create a query language that can be converted into SQL queries of the described type and bring the result to a single format

And already inside the biobank, you can run absolutely any SQL queries to find dependencies between data, but the results of such queries should always remain inside the biobank.

In total, the data analysis process in the federation takes on the following form:
1. We describe how the fields of the CDM we have selected are related to the tables of a specific biobank
2. We bring the values to a single standard (a single dictionary of values)
3. We select the patients we need with the help of specials. query language
4. We send inside the biobank the task "perform an analysis of X for patients who received the following requests for specials. query language"
5. Get the analysis results from each biobank and analyze them together

## Basic usage: differences and likeness with SQL
In the UCDM you may query any data from any tables defined in the CDM. I'll use OMOP CDM v5.4 as example, but it may be another CDM
Lets image we want to get distributions of patients race and gender born after 1970. in OMOP 5.4 data structure SQL query will be: 
```sql
SELECT gender_concept_id, race_concept_id
FROM patient
WHERE year_of_birth > 1970
GROUP BY 1, 2
```

In the UQL query will be similar, but a bit different

```yaml
SELECT:
  - gender
  - race
FROM: [ATLAS1]                             # Instead of table to select, we use FROM clause to defined list of biobanks
WHERE:
 - year_of_birth > 1970
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=SELECT%3A%0A++-+gender%0A++-+race%0AFROM%3A+%5BATLAS1%5D+++++++++++++++++++++++++++++%23+Instead+of+table+to+select%2C+we+use+FROM+clause+to+defined+list+of+biobanks%0AWHERE%3A%0A+-+year_of_birth+%3E+1970)

Differences are:
 - We use YAML syntax instead of SQL
 - We don't specify `FROM patient`, as we ALWAYS query distributions of patients. This is a subsequence from idea of limitation getting data that by identifier a patient
 - We don't write `GROUP BY`, as we ALWAYS query only distributions. Raw data may not be queried at all 
 - And one hidden thing: `HAVING COUNT(patient.id) > N` as always added to the query, where N is defined by each biobank. So any combination of fields may not be queried, if they limit number of patients more then to N

## Complex WHERE conditions
You may use any combination of logical operators: OR/AND/NOT. They will be translated into SQL correctly
For example:

```yaml
SELECT:                                    # List variables you want to see
  - gender                                 # Variables will be harmonized to CDM 
  - race
FROM: [ATLAS1]                             # Instead of table to select, we use FROM clause to defined list of biobanks
WHERE:
  - (race == 'White' OR gender == 'MALE') AND NOT (year_of_birth > 1970)
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=SELECT%3A++++++++++++++++++++++++++++++++++++%23+List+variables+you+want+to+see%0A++-+gender++++++++++++++++++++++%23+Variables+will+be+harmonized+to+CDM+%0A++-+race%0AFROM%3A+%5BATLAS1%5D+++++++++++++++++++++++++++++%23+Instead+of+table+to+select%2C+we+use+FROM+clause+to+defined+list+of+biobanks%0AWHERE%3A%0A++-+%28race+%3D%3D+%27White%27+OR+gender+%3D%3D+%27MALE%27%29+AND+NOT+%28year_of_birth+%3E+1970%29%0A)

## Using JOINs
Since the CDM basically contains more than 1 table, we must have a way to attach additional tables to the "patient" table, which is implied by default
To do this we should define required tables in the JOIN clause
As the same table may be joined several times, we should define an alias for each table
Important: we don't need to define JOIN ON expressions, as this knowledge already present in the CDM 

Let's imagine we want to query distributions on patients by deagnoses. UQL query for this will looks like:
```yaml
SELECT:
  - c1.name
JOIN:
  - condition_occurrence: c1              # condition_occurrence is table name defined in the CDM, where c1 is alias for this table in the query
FROM: [ATLAS1]
LIMIT: 30                                 # We also may define the limit of rows for better performance
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=SELECT%3A%0A++-+c1.name%0AJOIN%3A%0A++-+condition_occurrence%3A+c1%0AFROM%3A+%5BATLAS1%5D%0A%0A)

## Using multiple JOINs
And not lets imagine we want to query distribution on patient diagnoses with another diagnose "Cardiovascular disease"
TO do this we should JOIN diagnoses table twice:
1. To define first diagnose: Cardiovascular disease
2. To get variable with the second diagnose

```yaml
SELECT:
  - c1.name                               # It will be always "Cardiovascular", as we defined condition in WHERE expression
  - c2.name                               # It's another diagnoses of patients with Cardiovascular
JOIN:
  - condition_occurrence: c1              # c1 - it's first diagnoses of patient
  - condition_occurrence: c2              # c2 - it the second diagnose
FROM: [ATLAS1]
WHERE:
  - c1.name='Cardiovascular disease, unspecified'   # We are interested only in patients with Cardiovascular
LIMIT: 30   
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=SELECT%3A%0A++-+c1.name+++++++++++++++++++++++++++++++%23+It+will+be+always+%22Cardiovascular%22%2C+as+we+defined+condition+in+WHERE+expression%0A++-+c2.name+++++++++++++++++++++++++++++++%23+It%27s+another+diagnoses+of+patients+with+Cardiovascular%0AJOIN%3A%0A++-+condition_occurrence%3A+c1++++++++++++++%23+c1+-+it%27s+first+diagnoses+of+patient%0A++-+condition_occurrence%3A+c2++++++++++++++%23+c2+-+it+the+second+diagnose%0AFROM%3A+%5BATLAS1%5D%0AWHERE%3A%0A++-+c1.name%3D%27Cardiovascular+disease%2C+unspecified%27+++%23+We+are+interested+only+in+patients+with+Cardiovascular%0ALIMIT%3A+30+++)

## Time series
If we need not only "another diagnoses of patients with Cardiovascular", but diagnoses diagnosed in some period of time after Cardiovascular, we should work with time series
To do this we have options:
- Compare dates using more/less/equal sign (`<`, `>`, `=`)
- Add days/hours/minutes to date using `+` sign, for example: `c1.date + 90 days`

SQL query to get distribution of second diagnose, observed in 90 days after "Cardiovascular" will looks like this:

```yaml
SELECT:
  - c1.name                               # It will be always "Cardiovascular", as we defined condition in WHERE expression
  - c2.name                               # It's another diagnoses of patients with Cardiovascular
JOIN:
  - condition_occurrence: c1              # c1 - it's first diagnoses of patient
  - condition_occurrence: c2              # c2 - it the second diagnose
FROM: [ATLAS1]
WHERE:
  - c1.name='Cardiovascular disease, unspecified'   # We are interested only in patients with Cardiovascular
  - c1.start_date < c2.start_date
  - c1.start_date + 90 days > c2.start_date
LIMIT: 30
```
[Try this query](https://app.hyperunison.com/next/cohort/api?uql=SELECT%3A%0A++-+c1.name+++++++++++++++++++++++++++++++%23+It+will+be+always+%22Cardiovascular%22%2C+as+we+defined+condition+in+WHERE+expression%0A++-+c2.name+++++++++++++++++++++++++++++++%23+It%27s+another+diagnoses+of+patients+with+Cardiovascular%0AJOIN%3A%0A++-+condition_occurrence%3A+c1++++++++++++++%23+c1+-+it%27s+first+diagnoses+of+patient%0A++-+condition_occurrence%3A+c2++++++++++++++%23+c2+-+it+the+second+diagnose%0AFROM%3A+%5BATLAS1%5D%0AWHERE%3A%0A++-+c1.name%3D%27Cardiovascular+disease%2C+unspecified%27+++%23+We+are+interested+only+in+patients+with+Cardiovascular%0A++-+c1.start_date+%3C+c2.start_date%0A++-+c1.start_date+%2B+90+days+%3E+c2.start_date%0ALIMIT%3A+30)

## Variable aliases
If you want to define another name of column, you may define it as follows:
```yaml
SELECT:                                    # List variables you want to see
  - phenotype: gender                      # Variable `gender` will be shown in the column with name "phenotype" 
FROM: [ATLAS1]                             # Instead of table to select, we use FROM clause to defined list of biobanks
```
[Try this query](https://app.hyperunison.com/next/cohort/api?uql=SELECT%3A++++++++++++++++++++++++++++++++++++%23+List+variables+you+want+to+see%0A++-+phenotype%3A+gender++++++++++++++++++++++%23+Variable+%60gender%60+will+be+shown+in+the+column+with+name+%22phenotype%22+%0AFROM%3A+%5BATLAS1%5D+++++++++++++++++++++++++++++%23+Instead+of+table+to+select%2C+we+use+FROM+clause+to+defined+list+of+biobanks%0A)
