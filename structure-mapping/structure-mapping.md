# What is the Unison structure mapping

## Introduction

Structure mapping is the process of defining how to transfer data from the source data structure to the target data structure. This involves creating relationships between source and target tables and fields.

There are two types of rules:

- **Mapping rules**: Descriptions on how to transfer and transform the data.  
- **Data extraction rules**: Short SQL scripts that define the data transfer process.

By default, Unison does not transform source data into the target dataset directly. Instead, it uses your scripts to transform the data on the fly when you:

- Export data to the target CDM.  
- Use Unison to write queries and retrieve cohorts of patients based on the target CDM.

This is important to keep in mind when working with Unison.

---

## Interface Overview

### **Source Tables**  
Displays the structure of the source database. You can expand the tree to drill down from the table view to the field view. It’s possible to select a table or a custom set of fields from different tables and view details in the Source Explorer and Mapping Rules sections. Table names can be used in Joins and Data Extraction Rules sections.

### **Source Explorer**  
Provides detailed information about the selected source table or column(s), including:

1. The name of the selected field(s).  
2. Value distribution in the source field.  
3. Domain suggestions: Unison can suggest domains based on algorithms that analyze source values and attempt to find correct concepts, retrieving domains from suggested values.  
4. Mapped CDM entities: Mapped CDM entities for the selected source fields.

### **Mapping Rules**  
Contains instructions on how to map source fields to CDM entities and fields.

### **Data Extraction Rules**  
SQL queries that define how to transfer data from source tables to CDM tables.

### **Joins**  
Intermediate tables that define relationships between source tables and prepare custom tables for mapping.

### **CDM Tables**  
Displays the structure of the target CDM.

---

## Process Overview

1. **Observe data and progress**  
2. **Write mapping rules**  
3. **Build relations between source tables**  
4. **Write extraction rules**  
5. **Publish extraction rules**

---

# Process Details

## How to Write a Mapping Rule

A mapping rule is part of the documentation that describes how each source field should be mapped to a CDM field. A data steward or domain expert provides these instructions for the data engineer.

### Action Map

1. Explore source tables and review the table and field names.  
2. Select a table and examine it in the Source Explorer section.  
3. Analyze source values and domain suggestions.  
4. Choose a target CDM entity.  
5. Click the **Add** button to create a new mapping rule for the selected source field.  
6. Select the target CDM field for the current source field.  
7. Write a description explaining how and why this source field maps to the target entity's field.  
8. Click **Save** (or **Cancel** if more time is needed to finalize the mapping).

---

## How to Create Relations Between Source Tables

When you upload a dataset to Unison, all tables and fields are included. However, Unison cannot automatically determine relationships between tables. Users must establish these relationships in the **Joins** section.

### Relating to the Patient Table

1. Locate the table to relate to the patient table in the **Source Tables** section.  
2. Identify the field in the table that relates to the patient table.  
3. Open the patient table and find the field with patient IDs.  
4. Click **Create Join**.  
5. In the **Alias** field, enter a name for the join (e.g., the table's name).  
6. Set the **Left Table** to the patient table.  
7. Set the **Right Table** to the related table.  
8. Write an expression like:  
   `$table1.name_of_patient_id_field = $table2.name_of_patient_id_field`  
9. Click the **Save** button.

### Relating Other Tables

You can only relate tables to existing ones. If a table has no direct relationship to the patient table, find an intermediate table that relates to both. Establish a relationship for the intermediate table, then connect it to the desired table using the same process as above.

---

## How to Write an Extraction Rule

An extraction rule is an SQL query that links source fields to CDM fields. Unison uses these rules to transform data on the fly during exports or queries in the Unison cohort browser.

Rules are based on CDM entities and include required and optional fields. Required fields are created automatically, while optional fields can be added as needed.

When an extraction rule is created for a CDM entity field, the data becomes available for use immediately if marked as a vocabulary field. After all required fields are published, the data can be used in the cohort API and exports.

Published data appears in the Federation tab and is transformed during query execution. This allows you to store data in the source structure while transforming it into any CDM in parallel.

### Action Map

1. Navigate to the **Source Tables** section.  
2. Select a table with mapping rules (check the count on the right).  
3. Review the **Mapping Rules** section.  
4. Locate the mapped entities in the **CDM Tables** section.  
5. Expand the **Data Extraction Rules** section.  
6. Click the **Add Rule** button.

By default, the person table is available because it is linked in the runner settings. Other tables become available for extraction after relationships are created between the source patient table and the required source table.

#### Steps to Create an Extraction Rule

1. Expand the field list by clicking on an item (e.g., "person").  
2. Select a field for extraction and click the **Edit** button.  
3. Choose the join from which to extract data (this can be the source table or a custom table).  
4. Write an SQL expression for extraction. Refer to the documentation or the demo dataset for guidance.  
5. Click **Save**.  
6. Click the **Play** button to fetch data from the source and verify results.

---

## Publishing

### Core Principle

Publishing is the process that activates your data extraction rules.  
Unpublished data extraction rules are only used for value mapping and do not enable full functionality.  
Data becomes usable after all fields within a mapping rule and the mapping rule itself are published.

**Core Rule**: An entity extraction rule can only be published after all its required fields have been published.

---

### Action Map for Field Publishing


1. Select the CDM entity and expand the **Data Extraction Rules** section.  
2. Locate and expand the entity with data extraction rules.  
3. Choose the field to publish and click the **Edit** button.  
4. Click the **Publish** button.  

---

### Action Map for Entity Publishing


1. Complete the previous steps to publish each field in the data extraction rule.  
2. Click the **Edit** button for the data extraction entity.  
3. Click the **Publish** button.  
