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
   3. Create file main.py with 3 functions inside in the repository ([repository boilerplate](https://github.com/Hyperunison/federated-pipeline-boilerplate). Fill free to fork it):
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
   2. Installation instructions
4. Now you are free to execute the pipeline in the federated way

## Defining input params
Nextflow community have already made format of file to define input params for nextflow pipelines (This format is describe here: https://json-schema.org/draft/2020-12/schema)
Hyperunison uses the same file format to define pipeline parameters
Input parameters are used for:
- Building web form for running pipeline
- Used in generation input files for pipeline
- Used in API to run pipelines

## Input files generation
As each nextflow pipeline requires input files in the custom format, we need some executable code to generate them base on the input patients cohort.
We provide an abstracted python function to generate any number of input files: `get_input_files()`
Input parameters for `get_input_files()` function are:
- `ucdm: List[Dict[str, str]]` - harmonized result of UQL query plus the participant_id field.
  For example, if you have used UQL query `SELECT: [gender, year_of_birth] ...`,
  input data for `get_input_files()` will be [{participant_id: 12, race: "White", year_of_birth: 1970}, {participant_id: 84, race: "Black", year_of_birth: 1972}, ...].
  Input data will be in the same format, and will not depend on the data custodian data format.
  For example, if in CDM used in the UQL query, genders may be "Male" and "Female", they will be the same in input data, even if in the data custodian's database values are `M` and `F` for example
  As this samplesheet never leaves data custodians side, no defending of personal information politics are applied and participant_id field is free to use
- `parameters: List[Dict[str, str]]`: list of pipeline parameters. Keys are taken from the nextflow_schema.json file and values are provided by the user

Result of function is a Dict[str, str], where keys are filenames and values are content of each file

## Nextflow command generation
Any nextflow pipeline may be run using Unison federation layer, to enable this we provide a function to generate nextflow command to start the pipeline
Input parameters to the function are:
- `input_files: Dict[str, str]` - result of the `get_input_files()` function
- `parameters: Dict[str, str]` - the same parameters as passed to the `get_input_files()` function
- `run_name: str` - it's system parameter, you should pass it to the -name nextflow parameter of the pipeline, like this: `nextflow run ... -name {} ...`. It is used to control pipeline execution by Unison platform
- `weblog_url: str` - it's system parameter, you should pass it to the -with-weblog nextflow parameter of the pipeline, like this: `nextflow run ... -with-weblog {} ...`. It is used to track pipeline execution by Unison platform and show pipeline progress at the Unison web platform

Result of the function is the string with command, like `nextflow run ...`

## Defining safe files for export
This block is most reviewed by data custodian to verify no files with personal information may be taken from their side.
Input parameter to the function is:
- `parameters: Dict[str, str]` - the same parameters as passed to the `get_input_files()` function

Result of the function is a Dict[str, str], where keys are file masks in the pipeline folder and values are path where to upload files to Hyperunison
For example:
```python
return {
    ".nextflow.log": "/basic/.nextflow.log",
    "samplesheet.csv": "/samplesheet.csv",
    "report.html": "/basic/report.html",
    "output/*.html": "/html/",
    "output/*.css": "/html/",
}
```

It means that we will upload
- `.nextflow.log` file to folder `/basic/`
- `samplesheet.csv` to the root
- `report.html` to the `basic`
- All html and css files from the `output` directory to the `html` folder

Ensure all that files do not contain any patient-level data overwise pipeline will not pass moderation process 
