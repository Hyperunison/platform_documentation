
# Unison Common Data Model - Documentation

## Overview
### What is the purpose of the UCDM?
The UCDM is an abstracted language to define cohort of patients in a secure way. This cohort definition may be applied to any biobank or data structure in the future.
UCDM may be built based on any CDM and applied to any biobank. This may be done using a mapping process, where we define the relation between the chosen CDM and the physical data structure in the biobank.

### What is the philosophy behind the UCDM?

Imagine how nice it would be if you could use one SQL query to access data from several biobanks at the same time. Get a lot of patient data in a single format, and analyze them.

But when implementing this, we come across the following problems:
1. The data in biobanks are in different formats, diseases are called differently, and the data, in general, may be poorly structured.
2. Biobanks do not allow you to take data out of their network. And how can they be combined if the data cannot be copied to one centralized location? Additionally, the SQL query language does not allow you to limit the data identifying the patient. Accordingly, biobanks do not allow a direct SQL interface for external researchers.

The first problem, although difficult, has a solution: the ETL process and data standardization (for example, OMOP or Sentinel).
The second problem currently has only a limited solution: you can copy data from one biobank to another friendly biobank (for example, from GEL and UKBB) and analyze them there. But how to combine data between countries when copying is not an option?

If SQL cannot be used, we need another query language that is as flexible as SQL but at the same time restricts the transfer of patient-identifying data.

To achieve this, we first need to define "patient identification data." Let it be any dataset (year of birth + diagnosis + gender + ...) that is available to a small number of patients in this particular biobank. Moreover, the biobank itself can determine this "small number." For Genomics England, this number is 7.

Any SQL queries of the following type will never export the patient's personal data, and we can freely export the result of such a query outside:
```SQL
SELECT COUNT(*), field1, field2, field3, ...
FROM patient
JOIN ...
WHERE ...
HAVING COUNT(*) > N
```
Where `field1`, `field2`, `field3`, ... are any data about the patient: gender, age, diagnosis, blood test result, etc.

The query explanation: We get the number of patients for each combination of `field1`, `field2`, `field3`, ..., provided that this number is greater than N. That is, provided that these data do not identify any patients.

We then create a query language that converts into SQL queries of the described type and standardizes the result.

Within the biobank, you can run any SQL queries to find dependencies between data, but the results of such queries should always remain inside the biobank.

The data analysis process in the federation takes the following form:
1. Define how the fields of the CDM are related to the tables of a specific biobank.
2. Bring the values to a single standard (a unified dictionary of values).
3. Select the patients using a specialized query language.
4. Submit a task to the biobank to "perform an analysis of X for patients selected by the query language."
5. Collect the analysis results from each biobank and analyze them together.

---

## Basic Usage: Differences and Similarities with SQL

In the UCDM, you may query any data from any tables defined in the CDM. I'll use OMOP CDM v5.4 as an example, but it may be another CDM.
Imagine we want to get distributions of patients' race and gender born after 1970. In the OMOP 5.4 data structure, the SQL query would be:
```sql
SELECT gender_concept_id, race_concept_id
FROM patient
WHERE year_of_birth > 1970
GROUP BY 1, 2
```

In the UQL, the query will be similar but a bit different:
```yaml
SELECT:
  - gender
  - race
FROM: [ATLAS1]  # Instead of specifying a table, use the FROM clause to define a list of biobanks.
WHERE:
  - year_of_birth > 1970
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=SELECT%3A%0A++-+gender%0A++-+race%0AFROM%3A+%5BATLAS1%5D%0AWHERE%3A%0A+-+year_of_birth+%3E+1970)

**Differences:**
- YAML syntax instead of SQL.
- No `FROM patient`, as we always query patient distributions. This limits data that can identify a patient.
- No `GROUP BY`, as only distributions are queried. Raw data cannot be queried.
- Hidden clause: `HAVING COUNT(patient.id) > N` is always added, where N is biobank-defined. This ensures no small patient subsets are queried.

---

## Complex WHERE Conditions

You may use any combination of logical operators: OR, AND, NOT. They will be translated into SQL correctly.

Example:
```yaml
SELECT:
  - gender
  - race
FROM: [ATLAS1]
WHERE:
  - (race == 'White' OR gender == 'MALE') AND NOT (year_of_birth > 1970)
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=...)

---

## Using JOINs

The CDM often contains multiple tables. To attach additional tables to the "patient" table (implied by default):
- Define required tables in the JOIN clause.
- Assign aliases to each table for multiple joins.
- JOIN ON expressions are unnecessary as the CDM already defines relationships.

Example query for patient diagnoses distributions:
```yaml
SELECT:
  - c1.name
JOIN:
  - condition_occurrence: c1
FROM: [ATLAS1]
LIMIT: 30
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=...)

---

## Using Multiple JOINs

For example, querying distributions for diagnoses with "Cardiovascular disease":
```yaml
SELECT:
  - c1.name
  - c2.name
JOIN:
  - condition_occurrence: c1
  - condition_occurrence: c2
FROM: [ATLAS1]
WHERE:
  - c1.name='Cardiovascular disease, unspecified'
LIMIT: 30
```
[Try this query yourself](https://app.hyperunison.com/next/cohort/api?uql=...)

---

## Time Series

To query diagnoses within a time frame:
```yaml
SELECT:
  - c1.name
  - c2.name
JOIN:
  - condition_occurrence: c1
  - condition_occurrence: c2
FROM: [ATLAS1]
WHERE:
  - c1.name='Cardiovascular disease, unspecified'
  - c1.start_date < c2.start_date
  - c1.start_date + 90 days > c2.start_date
LIMIT: 30
```
[Try this query](https://app.hyperunison.com/next/cohort/api?uql=...)

---

## Variable Aliases

To rename columns:
```yaml
SELECT:
  - phenotype: gender
FROM: [ATLAS1]
```
[Try this query](https://app.hyperunison.com/next/cohort/api?uql=...)
