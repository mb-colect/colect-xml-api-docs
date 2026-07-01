# Product Types

Complete reference for all product-related data types.

{% hint style="info" %}
**Field Length Limits:** All string fields have maximum length limits. Key limits: `uniqueId` (64), `colorCode` (64), `name` (128), `description` (512), `summary` (2048). See [Field Length Limits](overview.md#field-length-limits) for the complete reference.
{% endhint %}

***

## Product

The main product entity representing a style-color combination.

{% hint style="info" %}
Products are uniquely identified by `uniqueId` + `colorCode`. Each color variant is a separate product entry.
{% endhint %}

### Mandatory Fields

| Field       | Type   | Description                                                                               |
| ----------- | ------ | ----------------------------------------------------------------------------------------- |
| `uniqueId`  | String | Product style identifier. Combined with `colorCode` this uniquely identifies the product. |
| `colorCode` | String | Color code. Combined with `uniqueId` this uniquely identifies the product.                |

### Basic Information Fields

| Field            | Type   | Description                                                         |
| ---------------- | ------ | ------------------------------------------------------------------- |
| `name`           | String | Product name displayed in app                                       |
| `description`    | String | Product description (second line display)                           |
| `colorDesc`      | String | Color description (e.g., "Black", "Navy Blue")                      |
| `brandId`        | String | Brand identifier for categorizing and filtering                     |
| `brandName`      | String | Brand name for display and filtering                                |
| `material`       | String | Material description for display and filtering                      |
| `gender`         | String | Gender for the product                                              |
| `crossReference` | String | Barcode/identifier for searching and scanning when no EAN available |
| `searchText`     | String | Additional text added to product search queries                     |

### Categorization Fields

| Field               | Type   | Description                        |
| ------------------- | ------ | ---------------------------------- |
| `seasonCode`        | String | Season code (e.g., "SS25", "FW24") |
| `seasonDesc`        | String | Season description                 |
| `categoryCode`      | String | Category code for filtering        |
| `categoryDesc`      | String | Category description               |
| `collectionId`      | String | Functional collection identifier   |
| `collectionDesc`    | String | Collection description             |
| `productGroupCode`  | String | Product group code                 |
| `productGroupDesc`  | String | Product group description          |
| `approvalGroupCode` | String | Approval workflow group code       |
| `approvalGroupDesc` | String | Approval group description         |

### Display and Status Fields

| Field                 | Type           | Description                                                                                                                                         |
| --------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sortCode`            | Integer        | Sort order for products                                                                                                                             |
| `status`              | Product Status | Product status (default: ACTIVE). See [Enumerations](enums.md#product-status)                                                                       |
| `noos`                | Boolean        | Never Out Of Stock flag                                                                                                                             |
| `sale`                | String         | Sale text displayed over product image. **Planned to be deprecated**, please use [label](products.md#nested-collections-xml-element-names) instead. |
| `saleBackgroundColor` | String         | Hexadecimal RGB color for sale badge (e.g., "ff0000" for red)                                                                                       |
| `styleAttributes`     | String         | Display-only attributes for product identification (e.g., season/drop prefix)                                                                       |
| `simpleColor`         | Color         | Basic color category for filtering. See [Enumerations](enums.md#color)                                                                             |

### Pricing and Ordering Fields

| Field                  | Type    | Description                                                           |
| ---------------------- | ------- | --------------------------------------------------------------------- |
| `minimalOrderQuantity` | Integer | Default minimum quantity required for this product (default: 0)       |
| `marginGroupCode`      | String  | Margin group identifier. Margin groups are defined on customer level. |

### Discount Groups

Products can be assigned to one or more discount groups using the `<discountGroupCode>` element. These codes link products to customer-level discount configurations.

```xml
<product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <!-- ... other fields ... -->
    <discountGroupCode>BASIC</discountGroupCode>
    <discountGroupCode>SEASONAL</discountGroupCode>
</product>
```

**How it works:**

1. Assign discount group codes to products (e.g., `BASIC`, `PREMIUM`, `SEASONAL`)
2. Configure discount percentages on customers using `<discountGroup>` with matching codes
3. When a customer orders a product, the system matches product codes to customer discount groups

{% hint style="warning" %}
**Order Matters:** Discount groups are evaluated in the order provided. If a product has multiple codes and a customer has discounts for multiple codes, the first matching discount is applied.
{% endhint %}

{% hint style="info" %}
**Wildcard:** On the customer side, a discount group with code `*` applies to all products regardless of their discount group codes.
{% endhint %}

### Custom Fields

| Field               | Type   | Description                          |
| ------------------- | ------ | ------------------------------------ |
| `fit`               | String | Fit description                      |
| `summary`           | String | Product summary                      |
| `userDefinedField1` | String | Custom text field for categorization |
| `userDefinedField2` | String | Custom text field for categorization |

### Date Fields

| Field               | Type     | Description                                                                             |
| ------------------- | -------- | --------------------------------------------------------------------------------------- |
| `startDate`         | DateTime | First date product is visible. Empty = infinitely in the past.                          |
| `endDate`           | DateTime | Last date product is visible. Empty = infinitely in the future.                         |
| `deliveryStartDate` | DateTime | First date of delivery block                                                            |
| `deliveryEndDate`   | DateTime | Last date of delivery block                                                             |
| `firstReceiptDate`  | DateTime | Date product becomes available. For display only - may differ from `deliveryStartDate`. |

### Nested Collections (XML Element Names)

{% hint style="info" %}
**Naming convention:** The field names in this table differ from the XML element names. Use the XML element name (right column) when writing XML documents.
{% endhint %}

| Field Name                   | XML Element                   | Type                                                         | Description                                                               |
| ---------------------------- | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------- |
| `prices`                     | `<price>`                     | List<[Price](products.md#price)>                           | Product prices                                                            |
| `sizes`                      | `<size>`                      | List<[Size](products.md#size)>                             | Available sizes with stock                                                |
| `media`                      | `<medium>`                    | List<[Medium](products.md#medium)>                         | Product images/videos                                                     |
| `labels`                     | `<label>`                     | List<[Label](products.md#label)>                           | Product labels/badges. Relates to `sale` and `saleBackgroundColor`.       |
| `extraFields`                | `<extraField>`                | List<[Extra Field](products.md#extra-field)>                 | Custom display fields                                                     |
| `deliveryWindows`            | `<deliveryWindow>`            | List<[Delivery Window](products.md#delivery-window)>         | Delivery windows/drops                                                    |
| `discountGroupCodes`         | `<discountGroupCode>`         | List\<String>                                                | Discount group codes. See [Discount Groups](products.md#discount-groups). |
| `translations`               | `<translation>`               | List<[Product Translation](products.md#product-translation)> | Translations                                                              |

### Deprecated Fields

| Field               | Type          | Description                                    |
| ------------------- | ------------- | ---------------------------------------------- |
| `deliverySubBlocks` | List\<String> | **DEPRECATED** - Use `deliveryWindows` instead |

***

## Size

Size information including stock and EAN codes.

### Mandatory Fields

| Field  | Type   | Description                   |
| ------ | ------ | ----------------------------- |
| `name` | String | Size name (S, M, L, 38, etc.) |

### Optional Fields

| Field                   | Type     | Description                                                                                                      |
| ----------------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| `sortCode`              | Integer  | Primary sort order. Sizes are sorted first by `sortCode`, then by `subSizeSortCode`.                             |
| `subSizeName`           | String   | Sub-size name (e.g., length for jeans: 32/34 where 32 is size and 34 is sub-size)                                |
| `subSizeSortCode`       | Integer  | Secondary sort order for sub-sizes                                                                               |
| `eanCode`               | String   | EAN or SKU level barcode                                                                                         |
| `prePackUnitCount`      | Integer  | Total pieces in SKU/box. Value is 1 for single sizes (default if empty).                                         |
| `replenishmentDate`     | DateTime | Date when size will be back in stock                                                                             |
| `orderRoundingQuantity` | Integer  | Ordering is only allowed for multiples of this quantity                                                          |
| `customField`           | String   | Custom information field                                                                                         |
| `accessCode`            | String   | Size access restriction code. If set, size only visible to customers whose `sizeAccessCodes` contain this value. |

### Nested Collections

| Field Name               | XML Element               | Type                                                               | Description                  |
| ------------------------ | ------------------------- | ------------------------------------------------------------------ | ---------------------------- |
| `stockLevels`            | `<stockLevel>`            | List<[Stock Level](products.md#stock-level)>                       | Stock quantities per date    |
| `extraFields`            | `<extraField>`            | List<[Extra Field](products.md#extra-field)>                       | Custom display fields        |
| `prepackContentElements` | `<prepackContentElement>` | List<[Prepack Content Element](products.md#prepack-content-element)> | Prepack contents definition  |
| `customerSizeNamings`    | `<customerSizeNaming>`    | List<[Customer Size Naming](products.md#customer-size-naming)>       | Customer-specific size names |

{% hint style="info" %}
**Size Access:** If `accessCode` is empty or matches one of the customer's `sizeAccessCodes`, the size is visible to that customer.
{% endhint %}

***

## Price

Product pricing with currency and customer group support.

### Mandatory Fields

| Field          | Type   | Description                                                                            |
| -------------- | ------ | -------------------------------------------------------------------------------------- |
| `currencyCode` | String | Currency code (EUR, USD, GBP). Must match exactly what is used in collection settings. |

### Price Targeting Fields

| Field         | Type   | Description                                                              |
| ------------- | ------ | ------------------------------------------------------------------------ |
| `priceGroup`  | String | Price group code for customer tier pricing                               |
| `customerNo`  | String | Specific customer number for individual pricing                          |
| `sizeName`    | String | Size name for size-specific pricing                                      |
| `subSizeName` | String | Sub-size for size-specific pricing. **Requires `sizeName` if set!**      |
| `eanCode`     | String | EAN code for size-specific pricing. **Must belong to the same product!** |

{% hint style="danger" %}
**Mutual Exclusivity:** It is NOT allowed to have both `priceGroup` AND `customerNo` in the same price element.
{% endhint %}

{% hint style="warning" %}
**EAN Code Warning:** If the EAN code refers to a size from a different product, this price will never match!
{% endhint %}

### Price Values

| Field                    | Type    | Required | Description                                                                  |
| ------------------------ | ------- | -------- | ---------------------------------------------------------------------------- |
| `retailPrice`            | Float   | No       | Suggested retail price per unit                                              |
| `wholesalePrice`         | Float   | No       | Wholesale price per unit                                                     |
| `originalWholesalePrice` | Float   | No       | Original wholesale price (shown when discounted)                             |
| `originalRetailPrice`    | Float   | No       | Original retail price (shown when discounted)                                |
| `purchasePrice`          | Float   | No       | Purchase/cost price per unit                                                 |
| `net`                    | Boolean | **Yes**  | Net price flag. When `true`, customer discounts and margins are NOT applied. |

### Validity Period

| Field       | Type     | Description               |
| ----------- | -------- | ------------------------- |
| `startDate` | DateTime | Price validity start date |
| `endDate`   | DateTime | Price validity end date   |

### Price Resolution Logic

The system resolves prices in the following order:

1. Customer-specific price (`customerNo` matches)
2. Price group price (`priceGroup` matches customer's active price group)
3. Size-specific price (`sizeName`/`eanCode` matches)
4. Default price (no `priceGroup`, `customerNo`, or `sizeName` specified)

{% hint style="info" %}
**Default Price:** If `priceGroup`, `customerNo`, and `sizeName` are all omitted, this becomes the default price for the product.
{% endhint %}

***

## Stock Level

Stock quantity with optional date-based availability.

### Fields

| Field              | Type     | Required | Description                                                                                   |
| ------------------ | -------- | -------- | --------------------------------------------------------------------------------------------- |
| `quantity`         | Integer  | **Yes**  | Total pieces in stock for the specified date (or overall if no date)                          |
| `startDate`        | DateTime | No       | Date from which this stock level applies. Omit for current stock.                             |
| `customField`      | String   | No       | Custom information field                                                                      |
| `warningThreshold` | Integer  | No       | Warning threshold for low stock. Values < 0 or > quantity make no sense. Default: no warning. |

{% hint style="info" %}
**Future Deliveries:** If future deliveries are expected, set different stock levels for certain dates. If date-based stock is not supported by your system, omit the `startDate` field.
{% endhint %}

### Example: Future Deliveries

```xml
<!-- Current stock (no date = immediate availability) -->
<stockLevel>
    <quantity>50</quantity>
</stockLevel>

<!-- Expected stock from Feb 15 -->
<stockLevel>
    <startDate>20250215</startDate>
    <quantity>200</quantity>
</stockLevel>

<!-- Expected stock from Mar 1 -->
<stockLevel>
    <startDate>20250301</startDate>
    <quantity>350</quantity>
</stockLevel>
```

***

## Medium

Product images and videos.

### Fields

| Field      | Type        | Required | Description                                                                                  |
| ---------- | ----------- | -------- | -------------------------------------------------------------------------------------------- |
| `type`     | String      | **Yes**  | Media type. See validated values in the table below.                                         |
| `url`      | String      | **Yes**  | High-resolution image/video URL                                                              |
| `thumbUrl` | String      | No       | Thumbnail URL. Auto-generated if omitted, but this slows down image processing considerably. |
| `sortCode` | Integer     | No       | Display order for sortable media types                                                       |

{% hint style="warning" %}
**Primary image required:** Each product must have a medium with `type` set to `primary` to display images in the app. Without it, the product shows without any image.
{% endhint %}

{% hint style="info" %}
**Thumbnail Generation:** While thumbnails are auto-generated if omitted, providing `thumbUrl` significantly improves app reload performance.
{% endhint %}

{% hint style="warning" %}
**sortCode Behavior:** The `sortCode` field has no effect on these media types: `primary`, `swatch`, `stamp_left`, `stamp_right`. These have fixed positions in the UI.
{% endhint %}

### Media Types

See [Medium Type](enums.md#medium-type) for the full list of 23 enum values. Most commonly used:

| Value | Description |
| ----- | ----------- |
| `primary`     | Main product image (**required**) |
| `swatch`      | Color swatch thumbnail (recommended) |
| `back`        | Back view |
| `model`       | Model shot (front) |
| `model_back`  | Model shot (back) |
| `left`        | Left side view |
| `right`       | Right side view |
| `stamp_left`  | Stamp overlay (left) |
| `stamp_right` | Stamp overlay (right) |
| `video`       | Product video |

***

## Delivery Window

Delivery drops or capsule collections.

### Fields

| Field         | Type                                                                       | Required | Description                      |
| ------------- | -------------------------------------------------------------------------- | -------- | -------------------------------- |
| `code`        | String                                                                     | **Yes**  | Window code (returned on orders) |
| `description` | String                                                                     | **Yes**  | Display description              |
| `startDate`   | DateTime                                                                   | No       | Window start date                |
| `endDate`     | DateTime                                                                   | No       | Window end date                  |
| `sortCode`    | Integer                                                                    | No       | Display order                    |
| `translation` | List<[Delivery Window Translation](products.md#delivery-window-translation)> | No       | Translations for description     |

{% hint style="warning" %}
**Important:** A delivery window code must have the same `startDate` across all products. If product A has code "DROP1" starting 2025-02-01, product B cannot have "DROP1" with a different start date.
{% endhint %}

***

## Delivery Window Translation

Translation of delivery window content for multilingual support.

### Fields

| Field      | Type                                                                           | Required | Description                                      |
| ---------- | ------------------------------------------------------------------------------ | -------- | ------------------------------------------------ |
| `language` | String                                                                         | No       | IETF language tag (e.g., "en", "nl", "de", "fr") |
| `fields`   | [Translated Delivery Window Fields](products.md#translated-delivery-window-fields) | No       | Translated field content                         |

***

## Translated Delivery Window Fields

Contains the translated values for a delivery window.

### Fields

| Field         | Type   | Required | Description                            |
| ------------- | ------ | -------- | -------------------------------------- |
| `description` | String | No       | Translated delivery window description |

***

## Label

Product badges/labels for visual highlighting.

### Fields

| Field             | Type   | Required | Description                |
| ----------------- | ------ | -------- | -------------------------- |
| `text`            | String | **Yes**  | Label text                 |
| `backgroundColor` | String | **Yes**  | Hex color (e.g., "ff0000") |

***

## Extra Field

Custom display fields for additional product information.

### Fields

| Field          | Type                                                               | Required | Description                                                            |
| -------------- | ------------------------------------------------------------------ | -------- | ---------------------------------------------------------------------- |
| `name`         | String                                                             | No       | Field identifier (unique within group)                                 |
| `description`  | String                                                             | No       | Display title/label (used as the title when displaying the field)      |
| `value`        | String                                                             | No       | Field value                                                            |
| `group`        | String                                                             | No       | Group name for collapsible organization (products only, not customers) |
| `imageUrl`     | String                                                             | No       | Icon URL displayed next to group header                                |
| `linkUrl`      | String                                                             | No       | Makes the field value a clickable link                                 |
| `important`    | Boolean                                                            | **Yes**  | When `true`, field is prominently displayed                            |
| `visible`      | Boolean                                                            | No       | When `false`, field is hidden (default: true)                          |
| `translations` | List<[Extra Field Translation](products.md#extra-field-translation)> | No       | Translations for description and value (XML: `<translation>`)          |

{% hint style="warning" %}
**Functional Requirement:** While `name` and `description` are technically optional in the schema, they are **functionally required** for the field to display properly. Always provide both for meaningful extra fields.
{% endhint %}

{% hint style="info" %}
**Group Display:** Extra fields with the same `group` value are displayed together in a collapsible section. The `imageUrl` is shown as an icon next to the group header.
{% endhint %}

***

## Product Relation

Relationship between products.

### Fields

| Field                    | Type                                                | Description                   |
| ------------------------ | --------------------------------------------------- | ----------------------------- |
| `sourceProductUniqueId`  | String                                              | Source product style          |
| `sourceProductColorCode` | String                                              | Source color                  |
| `targetProductUniqueId`  | String                                              | Target product style          |
| `targetProductColorCode` | String                                              | Target color                  |
| `type`                   | [ProductRelationType](enums.md#productrelationtype) | `MATCHING_SET` or `SUCCESSOR` |

***

## Translation Types

### Product Translation

| Field      | Type                                                             | Description                    |
| ---------- | ---------------------------------------------------------------- | ------------------------------ |
| `language` | String                                                           | Language code (en, nl, de, fr) |
| `fields`   | [Translated Product Fields](products.md#translated-product-fields) | Translated field values        |

### Translated Product Fields

All translatable product fields:

* `name`, `description`, `brandName`, `colorDesc`
* `categoryDesc`, `collectionDesc`, `productGroupDesc`
* `approvalGroupDesc`, `seasonDesc`, `gender`
* `material`, `fit`, `summary`, `sale`
* `searchText`, `userDefinedField1`, `userDefinedField2`

***

## Prepack Content Element

Defines the contents of a prepack/box containing multiple sizes.

### Fields

| Field                | Type                                                         | Required | Description                      |
| -------------------- | ------------------------------------------------------------ | -------- | -------------------------------- |
| `count`              | Integer                                                      | No       | Quantity of this size in prepack |
| `sizeName`           | String                                                       | No       | Size name included in prepack    |
| `sizeSortCode`       | Integer                                                      | No       | Size sort order                  |
| `subSizeName`        | String                                                       | No       | Sub-size name (e.g., length)     |
| `subSizeSortCode`    | Integer                                                      | No       | Sub-size sort order              |
| `customerSizeNaming` | List<[Customer Size Naming](products.md#customer-size-naming)> | No       | Customer-specific size names     |

### Example

```xml
<!-- Prepack containing 1 S, 2 M, 2 L, 1 XL -->
<size>
    <name>PACK-A</name>
    <prePackUnitCount>6</prePackUnitCount>
    <prepackContentElement>
        <sizeName>S</sizeName>
        <count>1</count>
    </prepackContentElement>
    <prepackContentElement>
        <sizeName>M</sizeName>
        <count>2</count>
    </prepackContentElement>
    <prepackContentElement>
        <sizeName>L</sizeName>
        <count>2</count>
    </prepackContentElement>
    <prepackContentElement>
        <sizeName>XL</sizeName>
        <count>1</count>
    </prepackContentElement>
</size>
```

***

## Customer Size Naming

Customer-specific size name override.

### Fields

| Field         | Type   | Required | Description                               |
| ------------- | ------ | -------- | ----------------------------------------- |
| `code`        | String | **Yes**  | Code matching customer's `sizeNamingCode` |
| `sizeName`    | String | **Yes**  | Customer-specific size name               |
| `subSizeName` | String | No       | Customer-specific sub-size name           |

{% hint style="info" %}
**Usage:** If a customer has a `sizeNamingCode` set, and a size has a `customerSizeNaming` with a matching `code`, the customer sees the custom size name instead of the standard one.
{% endhint %}

### Example

```xml
<size>
    <name>M</name>
    <customerSizeNaming>
        <code>US</code>
        <sizeName>Medium (US 8-10)</sizeName>
    </customerSizeNaming>
    <customerSizeNaming>
        <code>UK</code>
        <sizeName>Medium (UK 12-14)</sizeName>
    </customerSizeNaming>
</size>
```

***

## Product Extra Fields

Used for bulk extra field updates via the `updateExtraFields` operation.

### Fields

| Field        | Type                                         | Required | Description                                               |
| ------------ | -------------------------------------------- | -------- | --------------------------------------------------------- |
| `uniqueId`   | String                                       | **Yes**  | Product style identifier                                  |
| `colorCode`  | String                                       | **Yes**  | Product color code                                        |
| `extraField` | List<[Extra Field](products.md#extra-field)> | No       | Extra fields to set on this product (XML: `<extraField>`) |

{% hint style="info" %}
**Usage:** This type is used specifically with the `updateExtraFields` operation for efficient bulk extra field updates without sending full product data.
{% endhint %}

***

## Extra Field Translation

Translation of extra field content for multilingual support.

### Fields

| Field      | Type                                                                   | Required | Description                                      |
| ---------- | ---------------------------------------------------------------------- | -------- | ------------------------------------------------ |
| `language` | String                                                                 | No       | IETF language tag (e.g., "en", "nl", "de", "fr") |
| `fields`   | [Translated Extra Field Fields](products.md#translated-extra-field-fields) | No       | Translated field content                         |

***

## Translated Extra Field Fields

Contains the translated values for an extra field.

### Fields

| Field         | Type   | Required | Description                    |
| ------------- | ------ | -------- | ------------------------------ |
| `description` | String | No       | Translated display title/label |
| `value`       | String | No       | Translated field value         |

### Example

```xml
<extraField>
    <name>care_instructions</name>
    <description>Care Instructions</description>
    <value>Machine wash cold</value>
    <important>true</important>
    <translation>
        <language>nl</language>
        <fields>
            <description>Wasinstructies</description>
            <value>Machine wassen koud</value>
        </fields>
    </translation>
    <translation>
        <language>de</language>
        <fields>
            <description>Pflegehinweise</description>
            <value>Kalt waschen</value>
        </fields>
    </translation>
</extraField>
```
