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

### Overviews 
- Platform principle schema
- Architecture principle schema

### Guides
- Register accounts
- [How to connect database](./how-to-connect-db/how-to-connect-db.md)
- Connect example datasource (data without mappings)
- Connect training datasource (data and the mappings)
- Harmonisation process
  - Structure mapping (from source structure to CDM structure transformation)
  - Value mapping (from source value to vocabulary value mapping)
- Federation process
  - Query language
  - Cohort distribution building


### Tutorials
   * [SQL expression examples](https://github.com/Hyperunison/platform_documentation/wiki/Unison-SQL-expressions)
   * UQL examples
   * Invert dependencies
   * CTE creation and usage
   * Export log analysis

### API
- API & SDK 