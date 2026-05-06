# Schema Snapshots

This directory stores versioned snapshots of the Colect XML API XSD files. The XSD files don't carry version numbers, so we snapshot them by date to enable diffing between versions.

## Convention

Each snapshot is stored in a date-named directory: `snapshots/YYYY-MM-DD/`

Contents per snapshot:

| File                                | Document covered                                |
| ----------------------------------- | ----------------------------------------------- |
| `Product_Feed_XSD.xsd`              | Products input document                        |
| `Customer_Feed_XSD.xsd`             | Customers input document                       |
| `Customer_Access_XSD.xsd`           | Customer Access input document                 |
| `ProductAccess_Feed_XSD.xsd`        | Product Access input document                  |
| `ProductRelations_Feed_XSD.xsd`     | Product Relations input document _(new in v1.5)_ |
| `ProductPrices_Feed_XSD.xsd`        | Prices sub-feed input document                 |
| `ProductSizes_Feed_XSD.xsd`         | Sizes & Stock sub-feed input document          |
| `ProductExtraField_Feed_XSD.xsd`    | Extra Fields sub-feed input document           |
| `Invoices_Feed_XSD.xsd`             | Invoices input document                        |
| `HistoricalOrders_Feed_XSD.xsd`     | Historical Orders input document               |
| `OrderOutput_Feed_XSD.xsd`          | Orders output document                         |

## How to take a new snapshot

```bash
DATE=$(date +%Y-%m-%d)
SNAP=schema/snapshots/$DATE
BASE=https://assets.apptitude.nl/Documentation
mkdir -p "$SNAP"

for f in \
  Product_Feed_XSD.xsd \
  Customer_Feed_XSD.xsd \
  Customer_Access_XSD.xsd \
  ProductAccess_Feed_XSD.xsd \
  ProductRelations_Feed_XSD.xsd \
  ProductPrices_Feed_XSD.xsd \
  ProductSizes_Feed_XSD.xsd \
  ProductExtraField_Feed_XSD.xsd \
  Invoices_Feed_XSD.xsd \
  HistoricalOrders_Feed_XSD.xsd \
  OrderOutput_Feed_XSD.xsd
do
  curl -fsSL -o "$SNAP/$f" "$BASE/$f"
done
```

{% hint style="info" %}
The `ProductRelations_Feed_XSD.xsd` is new in v1.5. If you take a snapshot before its publication date, that file will 404 — that's expected. Document the gap in your snapshot's commit message.
{% endhint %}

## Diffing between snapshots

```bash
diff schema/snapshots/OLD_DATE/Product_Feed_XSD.xsd schema/snapshots/NEW_DATE/Product_Feed_XSD.xsd
```

To diff a whole snapshot directory:

```bash
diff -r schema/snapshots/OLD_DATE schema/snapshots/NEW_DATE
```
