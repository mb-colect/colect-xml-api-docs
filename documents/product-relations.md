# Product Relations

The **Product Relations** input document declares relationships between products — used by the app to surface successor styles and matching sets. New in **XML API v1.5**.

{% hint style="success" %}
**New in v1.5:** Product Relations is the first new input document since v1.4. It supports two relation types: `SUCCESSOR` and `MATCHING_SET`. See [Use cases](#use-cases) below for what each one drives in the user interface.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                                               |
| **Root element**        | `<productRelations>`                                                                                        |
| **File name pattern**   | `productRelations*.xml`                                                                                     |
| **Sync mode**           | Full set — relations not in the document are removed                                                        |
| **XSD**                 | [ProductRelations\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/ProductRelations_Feed_XSD.xsd)             |
| **Identifier**          | Tuple: `groupCode` + `sourceProductUniqueId` + `sourceProductColorCode` + `targetProductUniqueId` + `targetProductColorCode` + `type` |

***

## Use cases

| Relation type     | What it represents                                                              | Where it surfaces in the app                                                            |
| ----------------- | ------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **`SUCCESSOR`**   | The target product replaces the source product (e.g. SS25 successor of FW24).   | "This style replaces …" badge / link from the source product to the target.            |
| **`MATCHING_SET`** | Products that belong to a coordinated set (e.g. blazer + trousers + skirt).     | "Matching items" carousel on the source product, populated by the targets in the group. |

A single document can contain any mix of relation types. Group them by `groupCode` to keep multiple sets distinct.

***

## Fields

The schema declares all fields as optional, but functionally `sourceProductUniqueId`, `targetProductUniqueId`, and `type` are required to make a relation meaningful. The full field list:

| Element                     | Type     | Mandatory      | Description                                                                                                              |
| --------------------------- | -------- | -------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `groupCode`                 | `string` | Recommended    | Code that groups multiple `<productRelation>` entries into a logical set. Use the same `groupCode` for all members of a matching set. |
| `groupDescription`          | `string` | NO             | Human-readable description of the group (e.g. `SS25 Outfit Set 2`).                                                      |
| `sourceProductUniqueId`     | `string` | **YES** (functional) | The `uniqueId` of the source product — the one the user is currently looking at.                                         |
| `sourceProductColorCode`    | `string` | **YES** (functional) | The `colorCode` of the source product. Together with `sourceProductUniqueId`, identifies the exact source variant.       |
| `targetProductUniqueId`     | `string` | **YES** (functional) | The `uniqueId` of the related product (the successor or set member).                                                     |
| `targetProductColorCode`    | `string` | **YES** (functional) | The `colorCode` of the related product. Together with `targetProductUniqueId`, identifies the exact target variant.      |
| `type`                      | enum     | **YES**        | One of `SUCCESSOR` or `MATCHING_SET`.                                                                                    |

{% hint style="warning" %}
**Color codes matter.** A relation between `0102810/STB` and `0521510/STB` is different from `0102810/STB` and `0521510/GLP`. Populate `sourceProductColorCode` and `targetProductColorCode` for every relation, otherwise the relation may surface against the wrong color variant in the app.
{% endhint %}

***

## Basic XML structure

### Successor example

A product replaced by its next-season equivalent:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<productRelations>
  <productRelation>
    <type>SUCCESSOR</type>
    <sourceProductUniqueId>STYLE-FW24-001</sourceProductUniqueId>
    <sourceProductColorCode>BLK</sourceProductColorCode>
    <targetProductUniqueId>STYLE-SS25-001</targetProductUniqueId>
    <targetProductColorCode>BLK</targetProductColorCode>
  </productRelation>
</productRelations>
```

### Matching set example

A blazer paired with trousers and a skirt — all three styles in matching `STB` color, grouped under one set:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<productRelations>
  <productRelation>
    <type>MATCHING_SET</type>
    <groupCode>GROUP-002</groupCode>
    <groupDescription>SS25 Outfit Set 2</groupDescription>
    <sourceProductUniqueId>0102810</sourceProductUniqueId>
    <sourceProductColorCode>STB</sourceProductColorCode>
    <targetProductUniqueId>0521510</targetProductUniqueId>
    <targetProductColorCode>STB</targetProductColorCode>
  </productRelation>

  <productRelation>
    <type>MATCHING_SET</type>
    <groupCode>GROUP-002</groupCode>
    <groupDescription>SS25 Outfit Set 2</groupDescription>
    <sourceProductUniqueId>0102810</sourceProductUniqueId>
    <sourceProductColorCode>STB</sourceProductColorCode>
    <targetProductUniqueId>0521513</targetProductUniqueId>
    <targetProductColorCode>STB</targetProductColorCode>
  </productRelation>

  <productRelation>
    <type>MATCHING_SET</type>
    <groupCode>GROUP-002</groupCode>
    <groupDescription>SS25 Outfit Set 2</groupDescription>
    <sourceProductUniqueId>0102810</sourceProductUniqueId>
    <sourceProductColorCode>STB</sourceProductColorCode>
    <targetProductUniqueId>0621630</targetProductUniqueId>
    <targetProductColorCode>STB</targetProductColorCode>
  </productRelation>
</productRelations>
```

In this example, looking at product `0102810/STB` in the app surfaces three matching items: `0521510/STB`, `0521513/STB`, and `0621630/STB`.

***

## Bidirectional relations

The document is **directional**: a `<productRelation>` from A → B does not automatically imply B → A. To make a relation visible from both sides, send it twice with source and target swapped.

```xml
<!-- Visible from A: shows B as a related item -->
<productRelation>
  <type>MATCHING_SET</type>
  <groupCode>GROUP-002</groupCode>
  <sourceProductUniqueId>0102810</sourceProductUniqueId>
  <sourceProductColorCode>STB</sourceProductColorCode>
  <targetProductUniqueId>0521510</targetProductUniqueId>
  <targetProductColorCode>STB</targetProductColorCode>
</productRelation>

<!-- Visible from B: shows A as a related item -->
<productRelation>
  <type>MATCHING_SET</type>
  <groupCode>GROUP-002</groupCode>
  <sourceProductUniqueId>0521510</sourceProductUniqueId>
  <sourceProductColorCode>STB</sourceProductColorCode>
  <targetProductUniqueId>0102810</targetProductUniqueId>
  <targetProductColorCode>STB</targetProductColorCode>
</productRelation>
```

For a matching set of N items, build the cross-product if every member should reveal every other member — N × (N − 1) entries.

***

## Validation tips

{% hint style="info" %}
**Both source and target products must exist** in the most recent Products document. References to unknown products are silently dropped at ingestion.
{% endhint %}

{% hint style="warning" %}
**Removing a relation:** Send a Product Relations document that omits it. The full-set sync model removes any relation not present in the latest document.
{% endhint %}

{% hint style="info" %}
**`<xs:all>` ordering:** The XSD allows the inner elements of `<productRelation>` to appear in any order. Pick a stable order in your ERP exports for predictable diffs between snapshots.
{% endhint %}
