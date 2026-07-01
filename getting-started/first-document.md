# Your First Document

This walkthrough takes you from empty SFTP folder to a live product in your Colect collection. By the end you will have pushed a minimal Products document, confirmed it was ingested, and know where to go next.

***

## Before you start

You need two things before you can push anything:

<table><thead><tr><th width="299.92578125">What</th><th>Where to get it</th></tr></thead><tbody><tr><td>SFTP credentials (host, username, password)</td><td>Your Colect contact or the <a href="https://support.apptitude.nl/support/home?utm_source=colect_xml_api_docs&utm_medium=docs&utm_campaign=ask_support">Colect Support Team</a></td></tr><tr><td>An SFTP client</td><td><a href="https://filezilla-project.org/">FileZilla</a>, the <code>sftp</code> CLI, or your ERP's file-transfer tooling</td></tr></tbody></table>

{% hint style="warning" %}
If you do not have SFTP credentials yet, stop here and talk with your Colect contact or the [Colect Support Team](https://support.apptitude.nl/support/home?utm_source=colect_xml_api_docs&utm_medium=docs&utm_campaign=ask_support) for these credentials — there is nothing to configure on the API side until the credentials are issued.
{% endhint %}

***

## What we're building

A Products document is the master record of your catalog in Colect. This walkthrough pushes a single product — enough to confirm the pipeline works end to end before you wire up your ERP's full export.

```
you → products_first.xml → datafiles/ on SFTP → Cloud Connector → Colect collection
```

***

## Step 1 — Write the document

Create a file called `products_first.xml`. Replace the placeholder values with real ones from your catalog and your collection's configured currency.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
  <product>

    <!-- Mandatory: uniqueId + colorCode together identify this product -->
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>

    <!-- Recommended for display in the app -->
    <name>My First Product</name>
    <colorDesc>Black</colorDesc>

    <!-- At least one price — currencyCode must match your collection's currency -->
    <prices>
      <price>
        <currencyCode>EUR</currencyCode>
        <wholesalePrice>25.00</wholesalePrice>
        <retailPrice>49.95</retailPrice>
        <net>false</net>
      </price>
    </prices>

    <!-- At least one size with stock so the product is orderable -->
    <sizes>
      <size>
        <name>ONE SIZE</name>
        <sortCode>1</sortCode>
        <stockLevels>
          <stockLevel>
            <startDate>19800101</startDate>
            <quantity>100</quantity>
          </stockLevel>
        </stockLevels>
      </size>
    </sizes>

  </product>
</products>
```

### Why each section is needed

| Section                      | Why                                                                                                      |
| ---------------------------- | -------------------------------------------------------------------------------------------------------- |
| `<uniqueId>` + `<colorCode>` | Mandatory — the combination uniquely identifies the product and must appear exactly once in the document |
| `<name>`                     | Without a name the product card shows only the `uniqueId`                                                |
| `<prices>`                   | A product with no price cannot be added to a cart                                                        |
| `<sizes>`                    | A product with no sizes has nothing to order                                                             |
| `<stockLevels>`              | A size with no stock level defaults to 0 — the product shows as sold out unless `noos` is set            |

{% hint style="info" %}
**Full-set behavior:** Each Products upload becomes the complete catalog. Sending one product removes any previously imported products that are not in this file. This is expected for an initial push; your production export will include the full catalog.
{% endhint %}

***

## Step 2 — Upload via SFTP

```bash
sftp your-username@sftp.colect.services
sftp> cd datafiles
sftp> put products_first.xml
sftp> bye
```

The Connector picks up the file on the next run once it lands in `datafiles/`.

{% hint style="info" %}
**Filename tip:** The filename must match the pattern configured in your connector settings (e.g. `products*.xml`). Including a timestamp or sequence (e.g. `products_20260610.xml`) is a common convention that helps with bookkeeping and log correlation.
{% endhint %}

***

## Step 3 — Verify

Open your Colect collection in the Sales App or admin UI and check that the product `STYLE-001 / BLK` appears. It should be visible within a minute of upload.

If the product does not appear:

1. Check for a validation error. Common causes: incorrect path in backend setting, filename mismatch, wrong currency code, empty mandatory field, or wrong date format. See [Error Handling](error-handling.md) for the full error reference.
2. Contact your Colect contact or Colect Support with the filename and upload timestamp if you cannot identify the cause.

{% hint style="success" %}
Once the product appears in your collection, your SFTP credentials, file routing, and schema validation are all confirmed working — you're ready to connect your ERP's real export.
{% endhint %}

***

## Next steps

| Task                                                   | Where to go                                                                                                          |
| ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| Add more fields (images, delivery windows, categories) | [Products document](../documents/products.md)                                                                        |
| Push your full product catalog                         | [Products document](../documents/products.md) — the `products*.xml` file pattern, field reference, and sub-feed docs |
| Add customers so the sales team can log in             | [Customers document](../documents/customers.md)                                                                      |
| Control which products each customer can see           | [Product Access](../documents/product-access.md)                                                                     |
| Poll for orders once customers start ordering          | [Orders output document](../documents/orders.md)                                                                     |
| Troubleshoot a rejected file                           | [Error Handling](error-handling.md)                                                                                  |
