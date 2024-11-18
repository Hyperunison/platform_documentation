# Custom Vocabulary

HyperUnison includes standard OMOP 5.4 vocabularies downloaded from the [OHDSI Athena](https://athena.ohdsi.org/vocabulary/list) website.  
All these vocabularies are available by default and can be explored within the HyperUnison Platform.

When creating a custom CDM, fields may require values only from specific vocabularies.

## Explore Available Vocabularies

- Click on the **profile icon** in the top-right corner.
- Choose the **[Profile]** menu item.
- In the left menu, select **[Vocabularies]**.  

You will see a list of all available vocabularies, including public vocabularies and your custom ones.

## Create a Custom Vocabulary

1. Go to the **Vocabularies** page.
2. Download any existing vocabulary as an example. A good choice is the "Visit Type" vocabulary since it is small.
3. Modify the downloaded CSV file according to your needs. The fields are as follows:
   - **`concept_name`**: The name of the concept, which can be any string.
   - **`concept_code`**: A unique identifier for the concept in the vocabulary.
   - **`domain_id`**: The domain of the concept, such as `Observation`, `Measurement`, etc. Fields in a custom CDM may require values with a specific `domain_id`.
   - **`concept_class_id`**: The class of the concept, which can be any string (e.g., `Precoordinated Pair`, `Variable`, `Value`, etc.). Fields in a custom CDM may require values with a specific `concept_class_id`.
   - **`standard_concept`**: Indicates whether the concept is standard ("S") or non-standard ("N"). By default, auto-suggestions include only standard concepts. Fields in a custom CDM may require values marked as standard concepts.
4. Press the **[IMPORT]** button and upload your modified vocabulary.
