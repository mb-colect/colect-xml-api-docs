# Matching Sets

The catalog and stock are in — last piece is telling the app that the Coral (and Black) bikini top and bottom belong together, so a shopper looking at one sees the other as a matching set instead of two unrelated products.

***

## Step — Write the document

Create `atoll_product_relations.xml`. Relations are bidirectional by design — each pair needs one `<productRelation>` in each direction so the set surfaces regardless of which piece the shopper opens first.

{% code title="atoll_product_relations.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Atoll — walkthrough product relations document.
  Matching sets: bikini top + bottom, same colorway.
    ATL-BKT/CRL ↔ ATL-BKB/CRL
    ATL-BKT/BLK ↔ ATL-BKB/BLK
  Bidirectional — each pair links both ways so the set surfaces
  whichever piece the shopper looks at first.
-->
<productRelations>

  <productRelation>
    <type>MATCHING_SET</type>
    <groupCode>ATOLL-SET-CRL</groupCode>
    <groupDescription>Atoll Coral Set</groupDescription>
    <sourceProductUniqueId>ATL-BKT</sourceProductUniqueId>
    <sourceProductColorCode>CRL</sourceProductColorCode>
    <targetProductUniqueId>ATL-BKB</targetProductUniqueId>
    <targetProductColorCode>CRL</targetProductColorCode>
  </productRelation>

  <productRelation>
    <type>MATCHING_SET</type>
    <groupCode>ATOLL-SET-CRL</groupCode>
    <groupDescription>Atoll Coral Set</groupDescription>
    <sourceProductUniqueId>ATL-BKB</sourceProductUniqueId>
    <sourceProductColorCode>CRL</sourceProductColorCode>
    <targetProductUniqueId>ATL-BKT</targetProductUniqueId>
    <targetProductColorCode>CRL</targetProductColorCode>
  </productRelation>

  <productRelation>
    <type>MATCHING_SET</type>
    <groupCode>ATOLL-SET-BLK</groupCode>
    <groupDescription>Atoll Black Set</groupDescription>
    <sourceProductUniqueId>ATL-BKT</sourceProductUniqueId>
    <sourceProductColorCode>BLK</sourceProductColorCode>
    <targetProductUniqueId>ATL-BKB</targetProductUniqueId>
    <targetProductColorCode>BLK</targetProductColorCode>
  </productRelation>

  <productRelation>
    <type>MATCHING_SET</type>
    <groupCode>ATOLL-SET-BLK</groupCode>
    <groupDescription>Atoll Black Set</groupDescription>
    <sourceProductUniqueId>ATL-BKB</sourceProductUniqueId>
    <sourceProductColorCode>BLK</sourceProductColorCode>
    <targetProductUniqueId>ATL-BKT</targetProductUniqueId>
    <targetProductColorCode>BLK</targetProductColorCode>
  </productRelation>

</productRelations>
```
{% endcode %}

`groupCode` ties the two directional entries together as one set; `type` is `MATCHING_SET` (the other value is `SUCCESSOR`, for style replacements — see [Product Relations](../documents/product-relations.md)).

***

## Step — Push it

```bash
sftp your-username@sftp.colect.services
sftp> cd datafiles
sftp> put atoll_product_relations.xml
sftp> bye
```

***

## Step — Verify

Open the Coral Bikini Top or Bikini Bottom and look for the matching set surfaced on the product page — the other piece should appear as a suggested match. Repeat for the Black pair.

<!-- screenshot: matching set surfaced on a product page -->

***

Once the matching set shows up, move on to [Customers & Access](customers.md).
