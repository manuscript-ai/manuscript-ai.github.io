---
title: Document Recognition
nav_order: 2
---

# Document Recognition

## API /recognize Method Workflow

1. Document Detection: The system scans the input image for rectangular regions that resemble documents and crops them accordingly.

2. Classification: Each cropped region is classified into a document type such as passport, driver's license, etc.

3. Orientation Adjustment: The system assesses and corrects the orientation of each document by rotating or flipping it as necessary.

4. Field Extraction: Specific information fields (e.g., first name, last name, document number, MRZ codes) are identified and extracted from the document.

5. Optical Character Recognition (OCR): An OCR module reads and captures text from the extracted fields.

6. Confidence Scoring: The OCR assigns a confidence score to the text extracted from each field.

7. Human-in-the-Loop (HITL) Processing: If manual OCR mode is enabled, the HITL feature allows for human review of the extracted fields and text pairs.

8. Validation: The extracted text is validated against predefined patterns (masks) and dictionaries to ensure accuracy.

## Confidence Levels

The confidence score indicates the algorithm's certainty regarding the accuracy of character recognition:

- **0.90 – 1.00**: Extremely confident in the recognition.
- **0.70 – 0.89**: Fairly confident; minor errors might exist.
- **0.50 – 0.69**: Possible errors are present in the recognized text.
- **0.01 – 0.49**: Likely errors in the field; low confidence.
- **0**: Recognition failed; the field contains errors.

If the digitized text doesn't pass validation checks against patterns and dictionaries, the system returns an empty result with zero confidence.

## Field Comparison with External Data

This feature enables you to compare the recognized fields with data from your own file, which is useful for cross-verifying information from document images with existing records. To use this function, provide a JSON file through the `verify_fields` parameter.

**Example JSON for Matching Document Details**:

```json
{
  "series_and_number": "1111 222222",
  "surname": "Doe",
  "first_name": "John",
  "other_names": "Edward"
}
```

When creating your JSON file, use the field names specified in the API documentation.

he comparison function adds a valid attribute to each document field:

* true - The text in your JSON file matches the recognized text.

* false - The texts do not match.

* null - The field is not present in your JSON file.

Additionally, a levenshtein attribute is provided, indicating the Levenshtein distance between the recognized text and your data, which measures the difference between the two strings.
`


| Name | Type | Description |
| ----------- | ----------- | ----------- |
| external_check_fake | boolean | true: Simulates checks against external databases without performing actual verification (useful for debugging). false: Disables the simulated check. |
| doc_type | array | 	An array of document types to recognize in the input file. Useful for processing specific document types only. |
| priority | integer | A value greater than 0 (default is 1). Determines the priority of asynchronous tasks in the processing queue. Higher values indicate higher priority. |
| async | boolean | true: Enables asynchronous processing mode, allowing the request to be processed in the background. false: Processes the request synchronously. |
