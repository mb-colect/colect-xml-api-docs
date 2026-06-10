# Error Handling

When the Cloud Connector processes a file from your SFTP inbox, it either ingests the data successfully or rejects it. Rejected files are never silently dropped — every failure produces a `.err` sidecar that tells you exactly what went wrong.

***

## The inbox/error/ folder

After picking up a file from `inbox/`, the Connector does one of two things:

| Outcome | Where the file goes |
| ------- | ------------------- |
| Validation passes, data ingested | `inbox/archive/` |
| Validation fails | `inbox/error/` |

For every file that lands in `inbox/error/`, the Connector also writes a sidecar with the same filename and an added `.err` extension:

```
inbox/error/
├── products_2026-05-06T0900.xml
└── products_2026-05-06T0900.xml.err
```

{% hint style="info" %}
Your SFTP user has read access to `inbox/error/`. Poll this folder on the same schedule as your upload cycle to catch failures automatically.
{% endhint %}

***

## The .err sidecar

The `.err` file is plain text — one XSD validation message per line. Open it in any text editor or read it programmatically.

**Example:**

```
Validation error at line 8, column 14: cvc-minLength-valid: Value '' with length = '0' is not facet-valid with respect to minLength '1' for type 'nonEmptyString'.
Validation error at line 42, column 8: cvc-complex-type.2.4.a: Invalid content was found starting with element 'colorCode'. One of '{uniqueId}' is expected.
Validation error at line 104, column 22: cvc-datatype-valid.1.2.1: '2025-13-45' is not a valid value for 'dateTime'.
```

Each line follows the pattern:

```
Validation error at line <L>, column <C>: <XSD error code>: <human-readable detail>
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

The fastest way to confirm your credentials and pipeline are working end to end is to upload a minimal products document and observe where it lands.

**Upload:**

```bash
sftp colect-user@sftp.colect.services
sftp> cd inbox
sftp> put products_smoketest.xml
sftp> bye
```

**Check after a few seconds:**

```bash
sftp colect-user@sftp.colect.services
sftp> ls inbox/archive/   # present here = success
sftp> ls inbox/error/     # present here = failure, read the .err sidecar
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

{% hint style="success" %}
If the file lands in `inbox/archive/` and the product appears in your Colect collection, your pipeline is working end to end.
{% endhint %}

***

## Getting help

If you cannot resolve an error from the `.err` message alone:

1. Note the rejected filename and upload timestamp.
2. Copy the full content of the `.err` sidecar.
3. Contact Colect Support with both — and include the offending lines from the source XML if the error message references specific line numbers.
