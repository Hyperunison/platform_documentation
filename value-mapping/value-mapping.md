# How Unison Semantic Mapping Works

## Table of Contents

1. [What is Semantic Mapping?](#what-is-semantic-mapping)
2. [Unison Semantic Mapping Process Overview](#unison-semantic-mapping-process-overview)
3. [Equivalence and Mapping Type](#equivalence-and-mapping-type)
   - [Equivalence](#equivalence)
   - [Mapping Types](#mapping-types)
4. [Quick Mapping Interface](#quick-mapping-interface)
5. [Detailed Mapping Interface](#detailed-mapping-interface)
6. [Unison Mapping Suggestions](#unison-mapping-suggestions)
7. [Export Mappings](#export-mappings)
8. [Quick Mapping Workflow](#quick-mapping-workflow)
9. [Detailed Research Workflow](#detailed-research-workflow)
10. [Review Workflow](#review-workflow)
11. [Bulk Concept ID Assignment](#bulk-concept-id-assignment)

---

## What is Semantic Mapping?

Semantic mapping is the process of mapping source data to concept IDs from standard and non-standard vocabularies. These relationships are used for data standardization and enable data federation across multiple datasets.

---

## Unison Semantic Mapping Process Overview

Unison provides a semantic mapping process with three steps:

1. **Quick Mapping**: Table view with options for multiple choices and actions, ideal for values with equal concept names or high-confidence values.  
2. **Detailed Research**: A specialized view with more information.  
3. **Mapping Review**.

This process is managed using four mapping statuses:

- **Todo**  
- **Research**  
- **Review**  
- **Complete**

In all interfaces, Unison provides quick access to:

- The **OHDSI Athena** website.  
- A search field for finding concepts.  
- A detailed interface.

---

## Bulk Concept ID Assignment

Unison now supports bulk concept ID assignment using hotkeys for improved efficiency.

### **New Hotkeys**
- **Ctrl/Cmd + C** – Copy selected concept IDs.
- **Ctrl/Cmd + V** – Paste copied concept IDs into selected rows.
- **Alt/Option + 1** – Mark selected rows as **To Do**.
- **Alt/Option + 2** – Mark selected rows as **Research**.
- **Alt/Option + 3** – Mark selected rows as **Review**.
- **Alt/Option + 4** – Mark selected rows as **Complete**.

### **How It Works**
To copy a concept ID, select a row by clicking the checkbox and press **Ctrl/Cmd + C**. Then, select the target rows where you want to paste and press **Ctrl/Cmd + V**.

Similarly, to change the status of multiple rows, select them using checkboxes and use the corresponding **Alt/Option + [1-4]** hotkey to update their status.

These hotkeys streamline the mapping process, reducing manual input and improving workflow efficiency.

---

## Equivalence and Mapping Type

### **Equivalence**

Equivalence definitions are based on the [HL7 concept map equivalence](https://www.hl7.org/fhir/codesystem-concept-map-equivalence.html):

1. **Equal**: The concepts are exactly the same (i.e., intentionally identical).  
2. **Equivalent**: The concepts mean the same thing (i.e., extensionally identical).  
3. **Wider**: The target contains more information than the source.  
4. **Narrower**: The target contains less information than the source.  
5. **Inexact**: The target overlaps with the source, but both source and target cover additional meanings.

### **Mapping Types**

| Mapping Type        | Description | Example |
|---------------------|-------------|---------|
| **MAPS_TO**        | Indicates a direct relationship between one concept and another. | Diagnosis: Diabetes Mellitus → Concept: "Diabetes Mellitus" in OMOP. |
| **MAPS_TO_VALUE**  | Links specific values to concepts, such as lab test results. | Blood Glucose Level = 5.5 mmol/L → Concept: "Blood Glucose Level" in OMOP. |
| **MAPS_TO_UNIT**   | Associates units of measurement with concepts. | Blood Pressure Unit = mmHg → Concept: "Blood Pressure Measurement" in OMOP. |
| **MAPS_TO_OPERATOR** | Links mathematical/logical operators with concepts. | Blood Glucose Level > 7 mmol/L → Concept: "Blood Glucose Level" in OMOP. |
| **MAPS_TO_TYPE**   | Associates data types (e.g., integer, date) with concepts. | Data Type: Integer → Concept: "Age" in OMOP. |
| **MAPS_TO_NUMBER** | Maps numerical values directly to concepts. | Blood Pressure = 120/80 mmHg → Concept: "Blood Pressure Measurement" in OMOP. |

---

## Quick Mapping Workflow

1. Select a CDM field to view source values requiring mapping.  
2. Identify values with matching names, domains, and vocabularies.  
3. Click a row, then click **Complete**.  
4. For multiple selections, use **Ctrl** and click the desired rows.  
5. For alternative suggestions, click values to explore additional options.  
6. Use the three-dots menu to access detailed information via the Athena portal.  
7. If uncertain, click **Review** to move the value to the detailed research process.

---

## Detailed Research Workflow

1. Click **Detailed Research** in the quick mapping view.  
2. Select a CDM entity or view values by frequency.  
3. Review source value information.  
4. Examine search results and suggestions.  
5. Use previous mappings as references.  
6. If no valid suggestions exist, use the **Open in Athena** button to search for concepts.  
7. Add the correct concept(s) to the **Target Concepts** section.  
8. Define equivalence and mapping types.  
9. Click **Review** or **Complete**, as appropriate.

---

## Review Workflow

1. Click **Review** in the quick mapping view.  
2. Select a CDM entity or view values by frequency.  
3. Examine selected target concepts, source value information, and suggestions.  
4. Reference previously mapped concepts for validation.  
5. If changes are needed:  
   - Write a comment and click **Research** to return to the detailed research stage.  
   - Use the search field to refine concepts or filters.  
   - Update target concepts and mapping details.  
6. Click **Complete** to approve mappings.

