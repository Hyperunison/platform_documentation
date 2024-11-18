# How to test mappings
There are several ways to test mappings:
- Intermediate export - export of concrete table from CDM. 
- Query with API playground
- Full data export - export of all mapped data.

## Intermediate export
Available only if: 
- User runner has permissions for intermediate export
- Data was mapped to the CDM table and all SQL expressions are correct

Each mapped CDM table may be exported to check mapping correctness. Export works only to CSV format. All you need is:
- Open structure mapping
- Select interested CDM entity
- Click on "intermediate export" button and 
- Wait for export 
- Download CSV file and explore it 

## Query with API playground
- Use API playground and UQL
- Build query and explore results data and distributions

## Full data export
- Open structure mapping of the dataset of interest
- Go to the export page
- Select type of export (db or csv)
- Follow the instructions
