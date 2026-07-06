# Extra Fields

The **Extra Fields** input document is a sub-feed of [Products](products.md) that contains only `uniqueId`, `colorCode`, and `<extraFields>`. Use it to refresh free-form attributes (fabric composition, care instructions, certifications, etc.) without re-sending the full catalog.

{% hint style="success" %}
**Example:** `examples/extra-fields.xml` — extra fields grouped into materials, provenance, and certifications categories, with icons and an `important` flag.
{% endhint %}

{% code title="extra-fields.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Extra Fields sub-feed input document — example.
  Refreshes the <extraFields> block for one product with grouped attributes.
  This document REPLACES the entire <extraFields> block for each included product.

  Validates against https://assets.apptitude.nl/Documentation/ProductExtraField_Feed_XSD.xsd
-->
<products>

  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <extraFields>
      <!-- Group: materials -->
      <extraField>
        <name>fabric</name>
        <description>Fabric composition</description>
        <value>100% organic cotton</value>
        <group>materials</group>
      </extraField>
      <extraField>
        <name>weight</name>
        <description>Weight</description>
        <value>180 gsm</value>
        <group>materials</group>
      </extraField>
      <extraField>
        <name>care</name>
        <description>Care instructions</description>
        <value>Machine wash 30°C, do not tumble dry, iron on low</value>
        <group>materials</group>
      </extraField>

      <!-- Group: provenance -->
      <extraField>
        <name>made-in</name>
        <description>Made in</description>
        <value>Portugal</value>
        <group>provenance</group>
      </extraField>
      <extraField>
        <name>factory</name>
        <description>Factory</description>
        <value>Têxtil Aveiro, certified Sedex SMETA 4-pillar</value>
        <group>provenance</group>
      </extraField>

      <!-- Group: certifications, with icon and important flag -->
      <extraField>
        <name>gots</name>
        <description>GOTS certified</description>
        <value>Certified organic by GOTS</value>
        <group>certifications</group>
        <important>true</important>
        <imageUrl>https://images.example.com/icons/gots.svg</imageUrl>
        <linkUrl>https://global-standard.org/</linkUrl>
      </extraField>
      <extraField>
        <name>oekotex</name>
        <description>OEKO-TEX Standard 100</description>
        <value>OEKO-TEX Standard 100 — Class I (suitable for babies)</value>
        <group>certifications</group>
        <imageUrl>https://images.example.com/icons/oekotex.svg</imageUrl>
      </extraField>
    </extraFields>
  </product>

</products>
```
{% endcode %}

{% hint style="warning" %}
**This document REPLACES the entire `<extraFields>` block** for each included product. To add a single new extra field, send the document with all of the product's existing extra fields plus the new one.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                                  |
| **Root element**        | `<products>`                                                                                  |
| **File name pattern**   | `extraFields*.xml`                                                                            |
| **Sync mode**           | Sub-feed replace — products in document have extra fields replaced; others unchanged           |
| **XSD**                 | [ProductExtraField\_Feed\_XSD.xsd](https://assets.apptitude.nl/Documentation/ProductExtraField_Feed_XSD.xsd) |
| **Identifier**          | `uniqueId` + `colorCode`                                                                      |

***

## Extra field element

| Element        | Type       | Mandatory | Description                                                                                                          |
| -------------- | ---------- | --------- | -------------------------------------------------------------------------------------------------------------------- |
| `name`         | `string`   | **YES**   | Internal name of the extra field.                                                                                    |
| `description`  | `string`   | **YES**   | Title displayed when rendering the field.                                                                            |
| `value`        | `string`   | **YES**   | The value.                                                                                                           |
| `group`        | `string`   | NO        | Group name — extra fields with the same `group` value are rendered together.                                         |
| `important`    | `boolean`  | NO        | `true` to display this field more prominently.                                                                       |
| `imageUrl`     | `string`   | NO        | URL to an icon displayed at the top of the group.                                                                    |
| `linkUrl`      | `string`   | NO        | URL opened when the icon is tapped (rendering not yet implemented in all surfaces).                                  |
| `visible`      | `boolean`  | NO        | Whether the field is visible.                                                                                        |

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<products>
  <product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <extraFields>
      <extraField>
        <name>fabric</name>
        <description>Fabric composition</description>
        <value>100% organic cotton</value>
        <group>materials</group>
      </extraField>
      <extraField>
        <name>care</name>
        <description>Care instructions</description>
        <value>Machine wash 30°C, do not tumble dry</value>
        <group>materials</group>
      </extraField>
      <extraField>
        <name>gots</name>
        <description>Certified organic</description>
        <value>GOTS-certified organic cotton</value>
        <group>certifications</group>
        <important>true</important>
        <imageUrl>https://images.example.com/icons/gots.svg</imageUrl>
        <linkUrl>https://global-standard.org/</linkUrl>
      </extraField>
    </extraFields>
  </product>
</products>
```

***

## Grouping in the UI

The `group` field controls how extra fields cluster together in the app. Fields sharing a `group` value appear under the same heading; the optional `imageUrl` on any field in a group decorates the group header.

```
┌───────────────────────────────────────┐
│ Materials                             │  ← group="materials"
│ ─────────────────────────────────────│
│ Fabric composition: 100% organic cotton│
│ Care instructions: Machine wash 30°C  │
│                                       │
│ [icon] Certifications                 │  ← group="certifications", imageUrl set
│ ─────────────────────────────────────│
│ ⭐ Certified organic                   │  ← important=true → highlighted
│    GOTS-certified organic cotton      │
└───────────────────────────────────────┘
```

***

## Validation tips

{% hint style="info" %}
**Mandatory fields are mandatory.** Even though Extra Fields is a sub-feed, each individual `<extraField>` must still carry `name`, `description`, and `value`. Sending an empty `<extraField/>` is invalid.
{% endhint %}

{% hint style="warning" %}
**To remove all extra fields** from a product, include the product with an empty `<extraFields/>` block. Omitting the product entirely leaves its existing extra fields untouched (sub-feed semantics).
{% endhint %}
