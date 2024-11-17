# Welcome to Unison platform documentation

## Unison platform 
Unison platform provide functionality for data harmonisation and federation with 
common data models (CDM) like OMOP or Sentinel. For more specific goals Unison provide 
custom CDM builder and builder for vocabularies.  

## Core features
- data convertion from source structures and values to CDM and` standard vocabularies
- export to several SQL databases like PostgreSQL, mySQL, etc. 
- federated queries and data distribution across multiple datasets
- nextflow pipelines execution
- API for integration Unison platform with customer's analysis infrastructure  
- collaborative features for mapping person like data stewards, ETL engineers and others

## Contact us
If any questions via [LinkedIn](https://www.linkedin.com/company/hyperunison/), [Website](https://hyperunison.com/), [Slack community](https://join.slack.com/t/unisoncommunity/shared_invite/zt-2p75t4fhc-Kiksdz2sf19zjYi_JVHzNg)

## Table of contents

### Introduction
- Request account - Ivan

### Harmonisation proccess
- Add datasource
  - Example and training - Ivan
  - [Connect your database](./how-to-connect-db/how-to-connect-db.md)
- Make structure mapping - Ivan
  - [SQL expression examples](https://github.com/Hyperunison/platform_documentation/wiki/Unison-SQL-expressions)
  - [CTE creation and usage](cte-creation-and-usage/index.md)
  - Invert dependencies - Artem
- Make value mapping - Ivan
  - Part 1
  - Part 2
- Test mappings

Specialties
- Create custom CDM - Artem
- Create custom vocabulary - Artem
- Data export- Ivan
  - Export log analysis - Artem

### Federation and data extraction
- Create cohort (Cohort distribution building)
- [Query language = UQL - Artem](UQL/index.md)
- Pipeline execution

### API
- API & SDK
- Create API Key and use Unison API

### Users and access
- Create users (??)




