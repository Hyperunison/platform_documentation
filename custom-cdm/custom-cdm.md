# Unison custom CDM

## Custom Common Data Model
With the Unison platform you can create your own custom data model and use it within 
standard vocabulries. You may use it for creation fully custom CDMs or for building 
extensions for standard CDMs like OMOP. 

## How to create custom CDM

### define CDM properties
- CDM name - for example OMOP. This name should be stable through versions. 
- CDM version - version you can use major and minor version: 1.1, 1.2, 1.3
- Default table -  you should select root table that contain information of core entity, like  person.

### define tables
- table name - define name of the table. This name will be used within exports and as identifier with queries. 
- table description - 
- user guide - explain this table, what entity this table store, any specific data that helps users to understand it.
- ETL convention - specify requirements od what data may be stored at the field and what not. Describe corner case and instructions for them.

### define fields of the tables
- field name - define name of the field. This name will be used within exports and as identifier with queries. 
- required - select "required" if this field must be filled with the data or whole table can't be used. 
- Type - select type of the data. This type will be used as end type in all export files and all users should transform data from source type to this type.
- FK table - set up relations between tables. Select FK table for building relation. 
- Default mapping type - there are two mapping types at the Unison platform: "plain value" and "concept_id".
  - Concept_id means that values should be mapped to the standard vocabularies. 
  - Plain_value means that values don't need to be mapped to vocabularies and can be transported directly to the end strusture.
- Vocabularies constraints - use it to specify what vocabularies may be used with this CDM. Helpful for fully custom CDM that can't be used with any vocabulary, only with some special.
- Concept classes constraints - use it to specify what vocabularies may be used with this CDM. Helpful for fully custom CDM that can't be used with any vocabulary, only with some special.
- User guide - explain this field, what kind of data this field store.
- ETL convention - specify requirements od what data may be stored at the field and what not. Describe corner case and instructions for them.

