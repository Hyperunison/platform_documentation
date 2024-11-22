# Federated pipelines
## Philosophy
Unison pipelines - are nextflow pipelines with federated ability.
Unison makes not real federation, but federation of results.
So we execute pipelines on each node, export unpersonalized pipeline
results to Unison save haven and then combine results into one final
result. 

Federation means this additional properties: 
 - Defining input params for the pipeline, as they may differ from origin nextflow pipeline (or may not)
 - Automatic generation of input files to pipelines: samplesheets, config files, etc, from harmonized result of Cohort API
 - Automatic generation of nextflow command to be executed, based on input params
 - Defining safe files for export and verification rules, as federation should guarantee no patient level data will leave federation node

So we should add this 4 abilities to existing nextflow pipelines to make them federated.
To do this we will create additional github repository for each nextflow pipeline with definition of this 4 abilities

## Basic user journey
1. Create repository with the pipeline wrapper code on public github.com repository:
   1. Create a public pupeline in the github for federated pipeline wrapper
   2. Create nextflow_schema.json file in repository with input parameters definition
   3. Create file main.py with 3 functions inside in the repository ([repository boilerplate](https://github.com/Hyperunison/pipeline-wrapper-boilerplate). Fill free to fork it):
       - `get_input_files`: to build input files for pipeline, based on input params and harmonized patients cohort
       - `get_nextflow_cmd`: to generate nextflow command to be executed (like "nextflow run ...")
       - `get_output_file_masks`: to define files without patient-level data to be exposed from the federation node. Warning: this files should not contain any patient level data
   4. Create a git tag with first version of pipeline wrapper
   5. Create a release in the github with the tag
2. Register pipeline repository at the Hyperunison platform
   1. Click on user icon on the top right screen corner
   2. Choose "My Pipelines" on the left menu
   3. Click "Register custom pipeline" and fill the form with the link to the github repository and pipeline unique name
   4. Pipeline versions will be created automatically from the releases in the github repository
3. Install the pipeline code on the data custodian side:
   1. Ask data custodian to review and install required version of the wrapper and restart Runner
4. Now you are free to execute the pipeline in the federated way

## Defining input params
Nextflow community have already made format of file to define input params for nextflow pipelines (This format is describe here: https://json-schema.org/draft/2020-12/schema)
Hyperunison uses the same file format to define pipeline parameters
Input parameters are used for:
- Building web form for running pipeline
- Used in generation input files for pipeline


## Input files generation
As each nextflow pipeline requires input files in the custom format, we need some executable code to generate them base on the input patients cohort.


## Nextflow command generation


## Defining safe files for export

