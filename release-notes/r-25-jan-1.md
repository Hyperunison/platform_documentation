# Release Notes

## Overview
You can now take on the role of a data custodian, granting controlled access to your data, or act as a consumer, using all data available to you based on your data access status. This release also includes enhancements to the Unison Data Explorer, federated pipeline execution, and more.

---

## Details

### New Features
- **Data Explorer Integration**: Access public data repositories directly within the Unison platform.
- **Data Custodianship**: Become a data custodian and manage access to your data through the platform.
- **Access Rules Management**: Specify granular access permissions for users, from default settings to custom configurations.
- **Researcher Workspaces**:
  - Each user can create individual workspaces to analyze data from repositories.
  - Multiple workspaces can be created from a single data source to address different objectives.
  - Partial mapping can be tailored to align with specific research goals.
- **Spark DB Support**: Directly connect to a Spark database without requiring export to Spark.
- **MySQL Support**: Connect to MySQL databases and seamlessly export data to Spark DB.
- **Custom Federated Pipeline**: Integrate your custom pipelines into the Unison platform for enhanced flexibility.
- **OHDSI QC Tool Integration**: Use the integrated OHDSI QC tool to test the quality of OMOP-structured data before exporting it to PostgreSQL.
- **Pipeline Execution API**: Execute pipelines programmatically via API for streamlined workflows.

---

### Improvements
- **Enhanced Data Repository Connection Status**: Monitor the status of data repository connections.
- **Updated Start Page**: The platform opens directly on the converter page for a more intuitive workflow.
- **Simplified Mapping**: Removed short names from structure and value mapping for clarity.
- **Improved Main Menu**: Key sections are now highlighted for easier navigation.
- **Entity Sorting**: Value mapping lists can now be sorted by:
  - Completion percentage
  - Alphabetical order
  - Recommendation confidence level
- **Clearer Error Messages**: Custom CDM errors are now user-friendly and easier to interpret.
- **Federation Section Enhancements**:
  - The "Data" tab displays only selected workspaces.
  - Renamed columns in the "Mapping Used" tab to improve clarity.

---

### Bug Fixes
- Fixed issues with custom CDM forks and associated bugs.
- Resolved structure mapping inconsistencies between extracted values and source values.
- Corrected value mapping issues in example workspaces.

---

### Breaking Changes
- All APIs and SDK methods are updated and aligned with the latest runner version.

---

### Additional Information
Comprehensive user documentation is now available.