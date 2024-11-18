# Custom Vocabulary
Hyperunison contains standard OMOP 5.4 vocabularies downloaded from [OHDSI Athena](https://athena.ohdsi.org/vocabulary/list) website
All those vocabularies are available by default and may be explored in the Hyperunison Platform

During custom CDM creation, field may require to be only values from some specific vocabularies

## Explore available vocabularies
- Click on profile icon in the top right corner
- Choose [Profile] menu item
- On the left menu choose [Vocabularies]
You will see all available vocabularies: public and your own

## Create custom vocabulary
- Go to vocabularies page
- Download any existing vocabulary as example. Good choice will be "Visit Type" vocabulary, as it is small
- Modify downloaded CSV file to your needs. Fields description:
- - `concept_name` - name of concept, may be any string
- - `concept_code` - unique identifier of the concept in the vocabulary
- - `domain_id` - domain of concept, for example `Observation`, `Measurement`, .... Fields in a custom CDM may require only values with specific domain_id
- - `concept_class_id` - domain of concept, may be any string. For example `Precoordinated pair`, `Variable`, `Value`, .... Fields in a custom CDM may require only values with concept_class_id
- - `standard_concept` - string "S" (standard) or "N" (non-stanrard). By default auto suggestions are generated with only standard concepts.  Fields in a custom CDM may require only values with standard concepts
- Press [IMPORT] button and upload the vocabulary
