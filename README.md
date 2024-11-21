# Welcome to Unison Platform Documentation

## Unison Platform

The Unison platform provides functionality for data harmonization and federation with common data models (CDM) such as OMOP or Sentinel. For specific goals, Unison also offers a custom CDM builder and vocabulary builder.

---

## Core Features

- Data conversion from source structures and values to CDM and standard vocabularies.  
- Export to multiple SQL databases like PostgreSQL, MySQL, etc.  
- Federated queries and data distribution across multiple datasets.  
- **Nextflow** pipeline execution.  
- API for integrating the Unison platform with the customer's analysis infrastructure.  
- Collaborative features for mapping personnel such as data stewards, ETL engineers, and others.

---

## Contact Us

If you have any questions, feel free to reach out via:  
- [LinkedIn](https://www.linkedin.com/company/hyperunison/)  
- [Website](https://hyperunison.com/)  
- [Slack Community](https://join.slack.com/t/unisoncommunity/shared_invite/zt-2p75t4fhc-Kiksdz2sf19zjYi_JVHzNg)

---

## Table of Contents

### Introduction
- [Platform overview](./platform-overview/overview.md)
- [How to add example dataset](./example-dataset/example-dataset.md)

### Harmonization Process

- [Connect Your Database](./how-to-connect-db/how-to-connect-db.md)  
- [Make Structure Mapping](./structure-mapping/structure-mapping.md)  
  - [SQL Expression Examples](./sql-expressions-examples/sql-expressions.md)  
  - [CTE Creation and Usage](cte-creation-and-usage/index.md)  
- [Make Value Mapping](./value-mapping/value-mapping.md)  
- [Test Mappings](./test-mappings/test-mapping.md)  

### Federation and Data Extraction

- [Build Cohort Distribution](./api-playground/cohort-qerying.md)  
  - [Query Language - UQL](UQL/index.md)  
- [Pipeline Execution](./api-playground/pipeline-execution.md)  

### Specialties

- [Create Custom CDM](./custom-cdm/custom-cdm.md)  
- [Custom Vocabulary](./custom-vocabulary/index.md)

### API & SDK

- [SDK](https://github.com/Hyperunison/unison-sdk-python/tree/master)  
- [Create API Key](./api-descriptions/create-api-key.md)  
