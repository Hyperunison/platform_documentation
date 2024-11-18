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

### Harmonisation proccess
- Add datasource
  - [Connect your database](./how-to-connect-db/how-to-connect-db.md)
- [Make structure mapping](./structure-mapping/structure-mapping.md)
  - [SQL expression examples](./sql-expressions-examples/sql-expressions.md)
  - [CTE creation and usage](cte-creation-and-usage/index.md)
- [Make value mapping](./value-mapping/value-mapping.md)
- Test mappings

Specialties
- [Create custom CDM](./custom-cdm/custom-cdm.md)
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
- Create users 




