# Unison Custom CDM

## Custom Common Data Model

With the Unison platform, you can create your own custom data model and use it within standard vocabularies.  
This can be used to create fully custom CDMs or to build extensions for standard CDMs like OMOP.

## How to Create a Custom CDM

### Define CDM Properties

- **CDM Name**: For example, OMOP. This name should remain stable across versions.
- **CDM Version**: Specify the version using major and minor versions, e.g., 1.1, 1.2, 1.3.
- **Default Table**: Select the root table that contains information about the core entity, such as a person.

### Define Tables

- **Table Name**: Define the name of the table. This name will be used in exports and as an identifier in queries.
- **Table Description**: Provide a brief description of the table.
- **User Guide**: Explain the table’s purpose, the entity it stores, and any specific data to help users understand its role.
- **ETL Convention**: Specify requirements for the data stored in the table. Include corner cases and provide instructions for handling them.

### Define Fields of the Tables

- **Field Name**: Define the name of the field. This name will be used in exports and as an identifier in queries.
- **Required**: Mark the field as "required" if it must be filled with data; otherwise, the entire table cannot be used.
- **Type**: Select the data type. This type will be the final
