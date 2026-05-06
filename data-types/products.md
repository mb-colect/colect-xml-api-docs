# Product Types

Complete reference for all product-related data types.

{% hint style="info" %}
**Field Length Limits:** All string fields have maximum length limits. Key limits: `uniqueId` (64), `colorCode` (64), `name` (128), `description` (512), `summary` (2048). See [Field Length Limits](overview.md#field-length-limits) for the complete reference.
{% endhint %}

***

## XProduct

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
| `status`              | XProductStatus | Product status (default: ACTIVE). See [Enumerations](enums.md#xproductstatus)                                                                       |
| `noos`                | Boolean        | Never Out Of Stock flag                                                                                                                             |
| `sale`                | String         | Sale text displayed over product image. **Planned to be deprecated**, please use [label](products.md#nested-collections-xml-element-names) instead. |
| `saleBackgroundColor` | String         | Hexadecimal RGB color for sale badge (e.g., "ff0000" for red)                                                                                       |
| `styleAttributes`     | String         | Display-only attributes for product identification (e.g., season/drop prefix)                                                                       |
| `simpleColor`         | XColor         | Basic color category for filtering. See [Enumerations](enums.md#xcolor)                                                                             |

### Pricing and Ordering Fields

| Field                  | Type    | Description                                                           |
| ---------------------- | ------- | --------------------------------------------------------------------- |
| `minimalOrderQuantity` | Integer | Default minimum quantity required for this product (default: 0)       |
| `marginGroupCode`      | String  | Margin group identifier. Margin groups are defined on customer level. |

### Discount Groups

Products can be assigned to one or more discount groups using the `<discountGroupCode>` element. These codes link products to customer-level discount configurations.

```xml
<api:product>
    <api:uniqueId>STYLE-001</api:uniqueId>
    <api:colorCode>BLK</api:colorCode>
    <!-- ... other fields ... -->
    <api:discountGroupCode>BASIC</api:discountGroupCode>
    <api:discountGroupCode>SEASONAL</api:discountGroupCode>
</api:product>
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
**WS: Naming Convention:** The field names in this documentation differ from XML element names. Use the XML Element name in your SOAP requests.
{% endhint %}

| Field Name                   | XML Element                   | Type                                                         | Description                                                               |
| ---------------------------- | ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------- |
| `prices`                     | `<price>`                     | List<[XPrice](products.md#xprice)>                           | Product prices                                                            |
| `sizes`                      | `<size>`                      | List<[XSize](products.md#xsize)>                             | Available sizes with stock                                                |
| `media`                      | `<medium>`                    | List<[XMedium](products.md#xmedium)>                         | Product images/videos                                                     |
| `labels`                     | `<label>`                     | List<[XLabel](products.md#xlabel)>                           | Product labels/badges. Relates to `sale` and `saleBackgroundColor`.       |
| `extraFields`                | `<extraField>`                | List<[XExtraField](products.md#xextrafield)>                 | Custom display fields                                                     |
| `deliveryWindows`            | `<deliveryWindow>`            | List<[XDeliveryWindow](products.md#xdeliverywindow)>         | Delivery windows/drops                                                    |
| `minimumQuantities`          | `<minimumQuantity>`           | List<[XMinimumQuantity](products.md#xminimumquantity)>       | Minimum order quantities per customer key                                 |
| `discountGroupCodes`         | `<discountGroupCode>`         | List\<String>                                                | Discount group codes. See [Discount Groups](products.md#discount-groups). |
| `sizeIndependentStockLevels` | `<sizeIndependentStockLevel>` | List<[XStockLevel](products.md#xstocklevel)>                 | Overall product stock levels                                              |
| `translations`               | `<translation>`               | List<[XProductTranslation](products.md#xproducttranslation)> | Translations                                                              |

{% hint style="warning" %}
**Size-Independent Stock:** If a size is out of stock, ordering is still possible until the size-independent stock level reaches 0.
{% endhint %}

### Deprecated Fields

| Field               | Type          | Description                                    |
| ------------------- | ------------- | ---------------------------------------------- |
| `deliverySubBlocks` | List\<String> | **DEPRECATED** - Use `deliveryWindows` instead |

***

## XSize

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
| `stockLevels`            | `<stockLevel>`            | List<[XStockLevel](products.md#xstocklevel)>                       | Stock quantities per date    |
| `extraFields`            | `<extraField>`            | List<[XExtraField](products.md#xextrafield)>                       | Custom display fields        |
| `prepackContentElements` | `<prepackContentElement>` | List<[XPrepackContentElement](products.md#xprepackcontentelement)> | Prepack contents definition  |
| `customerSizeNamings`    | `<customerSizeNaming>`    | List<[XCustomerSizeNaming](products.md#xcustomersizeNaming)>       | Customer-specific size names |

{% hint style="info" %}
**Size Access:** If `accessCode` is empty or matches one of the customer's `sizeAccessCodes`, the size is visible to that customer.
{% endhint %}

***

## XPrice

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

## XStockLevel

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
<api:stockLevel>
    <api:quantity>50</api:quantity>
</api:stockLevel>

<!-- Expected stock from Feb 15 -->
<api:stockLevel>
    <api:startDate>2025-02-15T00:00:00</api:startDate>
    <api:quantity>200</api:quantity>
</api:stockLevel>

<!-- Expected stock from Mar 1 -->
<api:stockLevel>
    <api:startDate>2025-03-01T00:00:00</api:startDate>
    <api:quantity>350</api:quantity>
</api:stockLevel>
```

***

## XMedium

Product images and videos.

### Fields

| Field      | Type        | Required | Description                                                                                  |
| ---------- | ----------- | -------- | -------------------------------------------------------------------------------------------- |
| `type`     | XMediumType | **Yes**  | Media type. See [Enumerations](enums.md#xmediumtype)                                         |
| `url`      | String      | **Yes**  | High-resolution image/video URL                                                              |
| `thumbUrl` | String      | No       | Thumbnail URL. Auto-generated if omitted, but this slows down image processing considerably. |
| `sortCode` | Integer     | No       | Display order for sortable media types                                                       |

{% hint style="warning" %}
**IMAGE\_PRIMARY Required:** Each product must have a medium of type `IMAGE_PRIMARY` to display images in the app. Without it, the product will show without any image.
{% endhint %}

{% hint style="info" %}
**Thumbnail Generation:** While thumbnails are auto-generated if omitted, providing `thumbUrl` significantly improves app reload performance.
{% endhint %}

{% hint style="warning" %}
**sortCode Behavior:** The `sortCode` field has no effect on these media types: `IMAGE_PRIMARY`, `IMAGE_SWATCH`, `IMAGE_STAMP_LEFT`, `IMAGE_STAMP_RIGHT`. These have fixed positions in the UI.
{% endhint %}

### Media Types

| Type                   | Description            | Required    |
| ---------------------- | ---------------------- | ----------- |
| `IMAGE_PRIMARY`        | Main product image     | **Yes**     |
| `IMAGE_SWATCH`         | Color swatch thumbnail | Recommended |
| `IMAGE_BACK`           | Back view              | Optional    |
| `IMAGE_MODEL`          | Model wearing product  | Optional    |
| `IMAGE_MODEL_BACK`     | Model back view        | Optional    |
| `IMAGE_LEFT`           | Left side view         | Optional    |
| `IMAGE_RIGHT`          | Right side view        | Optional    |
| `IMAGE_TOP`            | Top view               | Optional    |
| `IMAGE_BOTTOM`         | Bottom view            | Optional    |
| `IMAGE_PACK`           | Packaging view         | Optional    |
| `IMAGE_FIT`            | Fit guide image        | Optional    |
| `IMAGE_STAMP_LEFT`     | Left corner overlay    | Optional    |
| `IMAGE_STAMP_RIGHT`    | Right corner overlay   | Optional    |
| `IMAGE_ADDITIONAL_1`   | Additional image 1     | Optional    |
| `IMAGE_ADDITIONAL_2`   | Additional image 2     | Optional    |
| `IMAGE_ADDITIONAL_3`   | Additional image 3     | Optional    |
| `IMAGE_ADDITIONAL_4`   | Additional image 4     | Optional    |
| `IMAGE_ADDITIONAL_5`   | Additional image 5     | Optional    |
| `VIDEO`                | Product video          | Optional    |
| `VIDEO_MODEL`          | Model video            | Optional    |
| `VIDEO_MODEL_LEAD_IN`  | Model video intro      | Optional    |
| `VIDEO_MODEL_LEAD_OUT` | Model video outro      | Optional    |
| `HTML`                 | HTML content           | Optional    |

***

## XDeliveryWindow

Delivery drops or capsule collections.

### Fields

| Field         | Type                                                                       | Required | Description                      |
| ------------- | -------------------------------------------------------------------------- | -------- | -------------------------------- |
| `code`        | String                                                                     | **Yes**  | Window code (returned on orders) |
| `description` | String                                                                     | **Yes**  | Display description              |
| `startDate`   | DateTime                                                                   | No       | Window start date                |
| `endDate`     | DateTime                                                                   | No       | Window end date                  |
| `sortCode`    | Integer                                                                    | No       | Display order                    |
| `translation` | List<[XDeliveryWindowTranslation](products.md#xdeliverywindowtranslation)> | No       | Translations for description     |

{% hint style="warning" %}
**Important:** A delivery window code must have the same `startDate` across all products. If product A has code "DROP1" starting 2025-02-01, product B cannot have "DROP1" with a different start date.
{% endhint %}

***

## XDeliveryWindowTranslation

Translation of delivery window content for multilingual support.

### Fields

| Field      | Type                                                                           | Required | Description                                      |
| ---------- | ------------------------------------------------------------------------------ | -------- | ------------------------------------------------ |
| `language` | String                                                                         | No       | IETF language tag (e.g., "en", "nl", "de", "fr") |
| `fields`   | [XTranslatedDeliveryWindowFields](products.md#xtranslateddeliverywindowfields) | No       | Translated field content                         |

***

## XTranslatedDeliveryWindowFields

Contains the translated values for a delivery window.

### Fields

| Field         | Type   | Required | Description                            |
| ------------- | ------ | -------- | -------------------------------------- |
| `description` | String | No       | Translated delivery window description |

***

## XLabel

Product badges/labels for visual highlighting.

### Fields

| Field             | Type   | Required | Description                |
| ----------------- | ------ | -------- | -------------------------- |
| `text`            | String | **Yes**  | Label text                 |
| `backgroundColor` | String | **Yes**  | Hex color (e.g., "ff0000") |

***

## XExtraField

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
| `translations` | List<[XExtraFieldTranslation](products.md#xextrafieldtranslation)> | No       | Translations for description and value (XML: `<translation>`)          |

{% hint style="warning" %}
**Functional Requirement:** While `name` and `description` are technically optional in the schema, they are **functionally required** for the field to display properly. Always provide both for meaningful extra fields.
{% endhint %}

{% hint style="info" %}
**Group Display:** Extra fields with the same `group` value are displayed together in a collapsible section. The `imageUrl` is shown as an icon next to the group header.
{% endhint %}

***

## XMinimumQuantity

Minimum order quantities per customer key.

### Fields

| Field      | Type    | Required | Description                                    |
| ---------- | ------- | -------- | ---------------------------------------------- |
| `key`      | String  | **Yes**  | Key matching customer's `minimumQuantitiesKey` |
| `quantity` | Integer | **Yes**  | Minimum quantity                               |

***

## XProductRelation

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

### XProductTranslation

| Field      | Type                                                             | Description                    |
| ---------- | ---------------------------------------------------------------- | ------------------------------ |
| `language` | String                                                           | Language code (en, nl, de, fr) |
| `fields`   | [XTranslatedProductFields](products.md#xtranslatedproductfields) | Translated field values        |

### XTranslatedProductFields

All translatable product fields:

* `name`, `description`, `brandName`, `colorDesc`
* `categoryDesc`, `collectionDesc`, `productGroupDesc`
* `approvalGroupDesc`, `seasonDesc`, `gender`
* `material`, `fit`, `summary`, `sale`
* `searchText`, `userDefinedField1`, `userDefinedField2`

***

## XPrepackContentElement

Defines the contents of a prepack/box containing multiple sizes.

### Fields

| Field                | Type                                                         | Required | Description                      |
| -------------------- | ------------------------------------------------------------ | -------- | -------------------------------- |
| `count`              | Integer                                                      | No       | Quantity of this size in prepack |
| `sizeName`           | String                                                       | No       | Size name included in prepack    |
| `sizeSortCode`       | Integer                                                      | No       | Size sort order                  |
| `subSizeName`        | String                                                       | No       | Sub-size name (e.g., length)     |
| `subSizeSortCode`    | Integer                                                      | No       | Sub-size sort order              |
| `customerSizeNaming` | List<[XCustomerSizeNaming](products.md#xcustomersizeNaming)> | No       | Customer-specific size names     |

### Example

```xml
<!-- Prepack containing 1 S, 2 M, 2 L, 1 XL -->
<api:size>
    <api:name>PACK-A</api:name>
    <api:prePackUnitCount>6</api:prePackUnitCount>
    <api:prepackContentElement>
        <api:sizeName>S</api:sizeName>
        <api:count>1</api:count>
    </api:prepackContentElement>
    <api:prepackContentElement>
        <api:sizeName>M</api:sizeName>
        <api:count>2</api:count>
    </api:prepackContentElement>
    <api:prepackContentElement>
        <api:sizeName>L</api:sizeName>
        <api:count>2</api:count>
    </api:prepackContentElement>
    <api:prepackContentElement>
        <api:sizeName>XL</api:sizeName>
        <api:count>1</api:count>
    </api:prepackContentElement>
</api:size>
```

***

## XCustomerSizeNaming

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
<api:size>
    <api:name>M</api:name>
    <api:customerSizeNaming>
        <api:code>US</api:code>
        <api:sizeName>Medium (US 8-10)</api:sizeName>
    </api:customerSizeNaming>
    <api:customerSizeNaming>
        <api:code>UK</api:code>
        <api:sizeName>Medium (UK 12-14)</api:sizeName>
    </api:customerSizeNaming>
</api:size>
```

***

## XPriceInfoElement

Used for bulk price updates via the `updatePrices` operation.

### Fields

| Field              | Type                              | Required | Description                                       |
| ------------------ | --------------------------------- | -------- | ------------------------------------------------- |
| `productUniqueId`  | String                            | **Yes**  | Product style identifier                          |
| `productColorCode` | String                            | **Yes**  | Product color code                                |
| `currencyCode`     | String                            | **Yes**  | Currency code (EUR, USD, etc.)                    |
| `sizeName`         | String                            | No       | Size name for size-specific pricing               |
| `subSizeName`      | String                            | No       | Sub-size name (requires `sizeName`)               |
| `eanCode`          | String                            | No       | EAN code for SKU-specific pricing                 |
| `customerNo`       | String                            | No       | Customer number for customer-specific pricing     |
| `priceGroup`       | String                            | No       | Price group for tier pricing                      |
| `priceType`        | [XPriceType](enums.md#xpricetype) | No       | Type of price to update (RETAIL, WHOLESALE, etc.) |
| `price`            | Float                             | No       | Price value                                       |
| `startDate`        | DateTime                          | No       | Price validity start date                         |
| `endDate`          | DateTime                          | No       | Price validity end date                           |

{% hint style="info" %}
**Usage:** This type is used specifically with the `updatePrices` operation for efficient bulk price updates without sending full product data.
{% endhint %}

***

## XStockInfoElement

Used for bulk stock updates via the `updateStock` operation.

### Fields

| Field              | Type                                         | Required | Description                     |
| ------------------ | -------------------------------------------- | -------- | ------------------------------- |
| `productUniqueId`  | String                                       | No\*     | Product style identifier        |
| `productColorCode` | String                                       | No\*     | Product color code              |
| `eanCode`          | String                                       | No\*     | EAN code for SKU identification |
| `sizeName`         | String                                       | No       | Size name                       |
| `subSizeName`      | String                                       | No       | Sub-size name                   |
| `stockLevels`      | List<[XStockLevel](products.md#xstocklevel)> | No       | Stock quantities per date       |

{% hint style="warning" %}
**Identification:** Provide either `productUniqueId` + `productColorCode` + `sizeName`, or `eanCode` to identify the size to update.
{% endhint %}

{% hint style="info" %}
**Usage:** This type is used specifically with the `updateStock` operation for efficient bulk stock updates without sending full product data.
{% endhint %}

***

## XProductExtraFields

Used for bulk extra field updates via the `updateExtraFields` operation.

### Fields

| Field        | Type                                         | Required | Description                                               |
| ------------ | -------------------------------------------- | -------- | --------------------------------------------------------- |
| `uniqueId`   | String                                       | **Yes**  | Product style identifier                                  |
| `colorCode`  | String                                       | **Yes**  | Product color code                                        |
| `extraField` | List<[XExtraField](products.md#xextrafield)> | No       | Extra fields to set on this product (XML: `<extraField>`) |

{% hint style="info" %}
**Usage:** This type is used specifically with the `updateExtraFields` operation for efficient bulk extra field updates without sending full product data.
{% endhint %}

***

## XExtraFieldTranslation

Translation of extra field content for multilingual support.

### Fields

| Field      | Type                                                                   | Required | Description                                      |
| ---------- | ---------------------------------------------------------------------- | -------- | ------------------------------------------------ |
| `language` | String                                                                 | No       | IETF language tag (e.g., "en", "nl", "de", "fr") |
| `fields`   | [XTranslatedExtraFieldFields](products.md#xtranslatedextrafieldfields) | No       | Translated field content                         |

***

## XTranslatedExtraFieldFields

Contains the translated values for an extra field.

### Fields

| Field         | Type   | Required | Description                    |
| ------------- | ------ | -------- | ------------------------------ |
| `description` | String | No       | Translated display title/label |
| `value`       | String | No       | Translated field value         |

### Example

```xml
<api:extraField>
    <api:name>care_instructions</api:name>
    <api:description>Care Instructions</api:description>
    <api:value>Machine wash cold</api:value>
    <api:important>true</api:important>
    <api:translation>
        <api:language>nl</api:language>
        <api:fields>
            <api:description>Wasinstructies</api:description>
            <api:value>Machine wassen koud</api:value>
        </api:fields>
    </api:translation>
    <api:translation>
        <api:language>de</api:language>
        <api:fields>
            <api:description>Pflegehinweise</api:description>
            <api:value>Kalt waschen</api:value>
        </api:fields>
    </api:translation>
</api:extraField>
```
