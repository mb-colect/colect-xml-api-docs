# Error Handling

When the Cloud Connector processes a file from `datafiles/`, it validates the XML against the schema before ingesting it. If validation fails, the data is not imported. How that failure is reported back to you depends on your integration setup — confirm the error notification mechanism with your Colect Support contact during onboarding.

{% hint style="info" %}
Regardless of how errors are surfaced, the XSD validation messages follow a standard format. The sections below document the most common errors and how to fix them.
{% endhint %}

***

## Validation error format

XSD validation errors follow this pattern:

```
Validation error at line <L>, column <C>: <XSD error code>: <human-readable detail>
```

**Example messages:**

```
Validation error at line 8, column 14: cvc-minLength-valid: Value '' with length = '0' is not facet-valid with respect to minLength '1' for type 'nonEmptyString'.
Validation error at line 42, column 8: cvc-complex-type.2.4.a: Invalid content was found starting with element 'colorCode'. One of '{uniqueId}' is expected.
Validation error at line 104, column 22: cvc-datatype-valid.1.2.1: '2025-13-45' is not a valid value for 'dateTime'.
```

The line and column numbers point to the offending element in the rejected XML.

***

## Common validation errors

### Missing mandatory field

**Error:**
```
cvc-minLength-valid: Value '' with length = '0' is not facet-valid with respect to minLength '1' for type 'nonEmptyString'.
```

**Cause:** A required field is present in the XML but contains an empty string. Most often `uniqueId`, `colorCode`, or `currencyCode`.

**Fix:** All mandatory fields must carry a non-empty value. See the field reference for each document type — mandatory fields are marked **Required**.

***

### Wrong date format

**Error:**
```
cvc-datatype-valid.1.2.1: '06-05-2025' is not a valid value for 'dateTime'.
```

**Cause:** Dates must be ISO 8601 (`YYYY-MM-DDTHH:MM:SS`). Day/month-first formats, slash-separated dates, and date-only strings are all rejected.

**Fix:** Use the format `2025-05-06T00:00:00`. The time component is required — use `T00:00:00` when you only have a calendar date.

***

### Duplicate uniqueId + colorCode

**Error:**
```
cvc-identity-constraint.4.2.2: Duplicate key-sequence ['STYLE-001', 'BLK'] in identity constraint ...
```

**Cause:** Two `<product>` elements in the same file share the same `uniqueId` + `colorCode` combination.

**Fix:** Each style-color variant must appear exactly once per file. De-duplicate in your ERP export before uploading.

***

### Wrong element order

**Error:**
```
cvc-complex-type.2.4.a: Invalid content was found starting with element 'colorCode'. One of '{uniqueId}' is expected.
```

**Cause:** Elements appear out of sequence. The XSD enforces element order within each type.

**Fix:** Check the element order against the field documentation for that document type. For products, `uniqueId` must come before `colorCode`.

***

## Verifying connectivity

Upload a minimal products document to confirm your credentials and pipeline are working:

```bash
sftp colect-user@sftp.colect.services
sftp> cd datafiles
sftp> put products_smoketest.xml
sftp> bye
```

**Minimal valid products file for smoke testing:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
  <product>
    <uniqueId>SMOKE-001</uniqueId>
    <colorCode>TST</colorCode>
  </product>
</products>
```

If the product appears in your Colect collection shortly after upload, the pipeline is working end to end.

***

## Getting help

If you cannot resolve a validation error:

1. Note the filename and upload timestamp.
2. Copy the full validation error message.
3. Contact Colect Support with both — and include the offending lines from the source XML if the error references specific line numbers.
