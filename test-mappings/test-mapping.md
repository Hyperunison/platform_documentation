# How to Test Mappings

There are several ways to test mappings:

- **Intermediate Export**: Export a specific table from the CDM to validate mapping accuracy.  
- **Query with API Playground**: Use API and UQL queries to explore results and distributions.  
- **Full Data Export**: Export all mapped data for verification.

---

## Intermediate Export

**Prerequisites**:  
- The runner has permissions for intermediate export.  
- Data has been mapped to a CDM table, and all SQL expressions are correct.

Each mapped CDM table can be exported to verify the correctness of the mapping. Exports are available only in CSV format.

**Steps**:  
1. Open the **Structure Mapping** section.  
2. Select the relevant CDM entity.  
3. Click the **Intermediate Export** button.  
4. Wait for the export process to complete.  
5. Download the CSV file and review its content to verify the mapping.

---

## Query with API Playground

1. Use the **API Playground** to execute queries.  
2. Build a query using **UQL** (Unison Query Language).  
3. Analyze the returned data and its distributions to ensure correctness.

---

## Full Data Export

1. Open the **Structure Mapping** section for the dataset of interest.  
2. Navigate to the **Export Page**.  
3. Select the export type: database (`db`) or CSV.  
4. Follow the provided instructions to complete the export process.  
