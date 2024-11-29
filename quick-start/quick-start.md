# Quick start guide
> **Warning:** this document assumes that a demo meeting was held

## welcome words  
This document describe number of steps that user can do for quick 
learning Unison platform. There are three level of mapping hardness 
user can reach:  

  - **easy**: person table 
  - **medium**: drug_exposure table
  - **hard**: visit_occurrence

## step 1: add example dataset
Use this [instruction](../example-dataset/example-dataset.md) and add 
"example" and "training" datasets. 

Example dataset contain finished mappings you can explore and use 
as guide for training dataset. 

## step 2: do struсture mapping
[full instructions for structure mapping](../structure-mapping/structure-mapping.md)

First of all - define patient table
![gif](./media/screen-recorder-fri-nov-29-2024-05-52-14-ezgif.com-video-to-gif-converter.gif)

**For patient table**
- create extraction rule
- describe extractions for each entity field

**For drug_exposure table**
- connect source tables `allergy.conditions`, `allergy.medications`, `allergy immunisations` to source patient table `allergies.patients` using SQL joins
- write three extraction rules for drug_exposure entity: 
  - from conditions, using SQL join of `allergy.condition` table
  - from medications, using SQL join of `allergy.medications`
  - from immunisations, using SQL join of `allergy.immunisation`
- for each extraction rule define extraction as in example allergy dataset. 

**For vizit occurrence table**
- [read about CTE](../cte-creation-and-usage/index.md)
- create CTE (copy CTE SQL from example and study SQL script)
- connect CTE to patient table using SQL joins
- write extraction rule for visit_occureence table

## step 3: make value mapping
> **note**: for each entity and entity field use lamp button and generate mapping suggestions.

![gif](./media/screen-recorder-fri-nov-29-2024-06-52-03-ezgif.com-video-to-gif-converter.gif)

- go through the values and check suggestions as complete or as research. 
- after all values are in completed or research statuses open research view. 
- for each value decide to leave suggestion or change it to more accurate value. 

for more details about value mapping use [value mapping guide](../value-mapping/value-mapping.md)


## step 4: open cohort API ant test data

- click on a federation tab
- select example and training data sources
- use examples from the examples tab or [full UQL tutorial](../UQL/index.md)
- write query with mapped data
- execute it and look at the distribution