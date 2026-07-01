# Error Handling

When the Cloud Connector processes a file from `datafiles/`, it validates and ingests it. If something is wrong, the import is rejected and the row in the Connector Status screen turns red. If a `monitoringEmailAddresses` is configured on the collection, validation errors also trigger an email notification to that address.

***

## Where to find errors

Open the Colect admin and go to **Connector Status**. Each row represents the most recent import run for a document type. A red row means the last import failed — the message column shows the reason.

The error message format is:

```
<timestamp>: <message>; state={lineNumber=X, columnNumber=Y}
```

**Example:**

```
Wed Jul 01 12:50:48 CEST 2026: Product occurs multiple times in source: uniqueId=DUPE-001, colorCode=BLK
```

The `lineNumber` and `columnNumber` point to the offending element in the uploaded file.

***

## Common errors

### Missing uniqueId

**Error:**
```
Product has no uniqueId (at line 20); state={lineNumber=20, columnNumber=13}
```

**Cause:** A `<product>` element has an empty `<uniqueId></uniqueId>` or the element is absent entirely. Both produce the same message.

**Fix:** Every `<product>` must have a non-empty `<uniqueId>`. Check for accidental blank values in your ERP export.

***

### Duplicate product

**Error:**
```
Product occurs multiple times in source: uniqueId=DUPE-001, colorCode=BLK
```

**Cause:** Two `<product>` elements in the same file share the same `uniqueId` + `colorCode` combination.

**Fix:** Each style-color variant must appear exactly once per file. De-duplicate in your ERP export before uploading.

***

### Unsupported media type

**Error:**
```
Medium format 'image' or type 'IMAGE_PRIMARY' not supported; state={lineNumber=171, columnNumber=35}
```

**Cause:** The `<type>` value inside a `<medium>` element is not recognised. The import format uses lowercase short names — not the internal `IMAGE_*` identifiers.

**Fix:** Use the lowercase values: `primary`, `back`, `swatch`, `model`, etc. See [Media objects](../documents/products.md#media-objects) for the full list.

***

### Invalid field value

**Error:**
```
Illegal value for discount group amount: null; state={lineNumber=43, columnNumber=70}
```

**Cause:** A field the connector expected to find has a null or unrecognised value. This typically means an attribute name is wrong — the connector can't find the field and reads it as null.

**Fix:** Check the field name against the documentation. Attribute names must match exactly — for example `amount` not `percentage` on `<discountGroup>`.

***

### Wrong date format (silent)

{% hint style="warning" %}
**Date errors are silent.** Wrong date formats do not produce a red row — the file imports successfully but dates are stored incorrectly or dropped. Stock levels with bad dates may show no available stock.
{% endhint %}

**Cause:** A date field uses a format other than `yyyyMMdd` (e.g. `2026-01-01` or `01-01-2026`).

**Observed behaviour:**
- `yyyyMMdd` — parsed correctly ✓
- `YYYY-MM-DD` — accepted but stock dates may be stored with an incorrect value
- `DD-MM-YYYY` — accepted but stock dates are dropped entirely

**Fix:** Always use `yyyyMMdd` (e.g. `20260115`). For stock levels that are always available, use `19800101` as the start date.

***

## Verifying connectivity

Upload a minimal products document to confirm your SFTP credentials and pipeline are working end to end:

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
    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <wholesalePrice>1.00</wholesalePrice>
      </price>
    </prices>
    <sizes>
      <size>
        <name>ONE</name>
        <sortCode>1</sortCode>
        <prePackUnitCount>1</prePackUnitCount>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>1</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>
  </product>
</products>
```

A green row in Connector Status confirms the pipeline is working. You can then delete `SMOKE-001` by omitting it from your next full products upload.

***

## Getting help

If you cannot resolve an error:

1. Note the filename and upload timestamp.
2. Copy the full error message from the Connector Status screen.
3. [Contact Colect Support](https://support.apptitude.nl/support/home?utm_source=colect_xml_api_docs&utm_medium=docs&utm_campaign=ask_support) with both — and include the offending lines from the XML if the error references specific line numbers.
