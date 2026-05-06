# Order Types

Complete reference for order, historical order, and invoice data types.

{% hint style="info" %}
**Field Length Limits:** All string fields have maximum length limits. Key limits for historical orders: `comment` (2048), `externalUrl` (512), `orderNumber` (128). Key limits for invoices: `invoiceNumber` (128), `externalUrl` (512). See [Field Length Limits](overview.md#field-length-limits) for the complete reference.
{% endhint %}

---

## XOrder

Order placed through Colect app or B2B webstore.

### Identification Fields

| Field | Type | Description |
|-------|------|-------------|
| `collectionId` | String | Colect collection identifier |
| `orderNumber` | String | Unique 12-character order number |
| `trackingNumber` | String | Transaction tracking (same for related operations) |
| `canceledOrderNumber` | String | Original order if this is a cancellation |
| `draftOrderNumber` | String | Draft order reference |

### Timestamps

| Field | Type | Description |
|-------|------|-------------|
| `timestamp` | DateTime | Created on device |
| `serverTimestamp` | DateTime | Received by server |

### Customer Information

| Field | Type | Description |
|-------|------|-------------|
| `customerNo` | String | Customer identifier |
| `customerPriceGroup` | String | Customer's price group |
| `currency` | String | Order currency |
| `shippingLocation` | [XLocation](customers.md#xlocation) | Selected shipping address |
| `consumerLocation` | [XLocation](customers.md#xlocation) | Consumer address (instore mode) |
| `storeLocation` | [XLocation](customers.md#xlocation) | Store address (instore mode) |
| `contact` | [XContact](customers.md#xcontact) | Order contact person |
| `agreement` | [XAgreement](customers.md#xagreement) | Selected agreement |

### Order Details

| Field | Type | Description |
|-------|------|-------------|
| `signed` | Boolean | Order was signed |
| `signee` | String | Name of signer |
| `salesPerson` | String | Salesperson email |
| `salesPersonKey` | String | Salesperson technical key |
| `comment` | String | Customer comment |
| `internalComment` | String | Internal note (not on confirmation) |
| `customerOrderReference` | String | Customer's PO number |
| `erpOrderReference` | String | For storing ERP reference |

### Delivery Information

| Field | Type | Description |
|-------|------|-------------|
| `requestedDeliveryDate` | DateTime | Requested delivery date |
| `requestedDeliveryEndDate` | DateTime | Delivery window end |
| `numberOfBoxes` | Integer | Number of boxes (returns) |
| `returnsContact` | String | Returns contact (returns) |

### Totals

| Field | Type | Description |
|-------|------|-------------|
| `totalCount` | Integer | Total quantity |
| `totalAmount` | Float | Total value |
| `status` | [XOrderStatus](enums.md#xorderstatus) | Order status |
| `internalOrderType` | [XInternalOrderType](enums.md#xinternalordertype) | ORDER or RETURN |
| `orderTypeCode` | String | Order type dropdown value |
| `approvalInfo` | String | Approval information |

### Free Text Fields

| Field | Type | Description |
|-------|------|-------------|
| `freeTextField1` | String | Custom text field |
| `freeTextField2` | String | Custom text field |

### Nested Collections

| Field | Type | XML Element | Description |
|-------|------|-------------|-------------|
| `orderLines` | List&lt;[XOrderLine](#xorderline)&gt; | `<orderLine>` | Line items |
| `customChoices` | List&lt;[XCustomChoice](customers.md#xcustomchoice)&gt; | `<customChoice>` | Selected custom options |
| `discountRules` | List&lt;[XOrderAmountModificationRule](customers.md#xorderamountmodificationrule)&gt; | `<discountRule>` | Applied discount rules |
| `deliveryCostRules` | List&lt;[XOrderAmountModificationRule](customers.md#xorderamountmodificationrule)&gt; | `<deliveryCostRule>` | Applied shipping rules |
| `orderConfirmation` | [XOrderConfirmation](#xorderconfirmation) | N/A | PDF confirmation (getUnprocessedOrders only) |

---

## XOrderLine

Line item in an order.

### Product Identification

| Field | Type | Description |
|-------|------|-------------|
| `productUniqueId` | String | Product style |
| `productColorCode` | String | Color code |
| `productStyleCode` | String | Style code |
| `productCollectionId` | String | Product's collection |
| `sizeName` | String | Size |
| `subSize` | String | Sub-size (width/length) |
| `eanCode` | String | EAN/barcode |
| `prepackName` | String | Prepack name |
| `sizeCustomField` | String | Size custom field |

### Quantity and Pricing

| Field | Type | Description |
|-------|------|-------------|
| `type` | [XOrderLineType](enums.md#xorderlinetype) | STOCK_ORDER, PRE_ORDER, RETURN_ORDER |
| `numberOfPieces` | Integer | Quantity ordered |
| `prePackUnitCount` | Integer | Units per prepack |
| `retailPrice` | Float | Retail price |
| `wholesalePrice` | Float | Wholesale with customer discount |
| `grossWholesalePrice` | Float | Before all discounts |
| `netWholesalePrice` | Float | After all discounts |
| `lineAmount` | Float | Total line value |

### Discounts and Margins

| Field | Type | Description |
|-------|------|-------------|
| `discountPercentage` | Float | Manual discount |
| `discountGroupCode` | String | Applied discount group |
| `discountFromDiscountGroup` | Float | Discount percentage |
| `discountGroupIdentifier` | String | Discount group identifier |
| `margin` | Float | Manual margin |
| `marginGroupCode` | String | Applied margin group |
| `marginFromMarginGroup` | Float | Margin from group |

### Delivery

| Field | Type | Description |
|-------|------|-------------|
| `deliverySubBlock` | String | Delivery sub-block |
| `deliveryWindowCode` | String | Delivery window code |
| `deliveryDate` | DateTime | Line delivery date |
| `alternateDeliveryDate` | DateTime | Alternate date |

### Other Fields

| Field | Type | Description |
|-------|------|-------------|
| `remark` | String | Line comment |
| `note` | String | Salesperson note |
| `returnReason` | String | Return reason code |
| `orderLineTypeCode` | String | Order line type dropdown |
| `index` | Integer | Line index (for duplicates) |
| `customerSizeName` | String | Customer-specific size |
| `customerSubSizeName` | String | Customer-specific sub-size |
| `files` | List&lt;[XRemoteFile](#xremotefile)&gt; | Attached files (XML: `<remoteFile>`) |

---

## XOrderConfirmation

PDF confirmation document.

| Field | Type | Description |
|-------|------|-------------|
| `orderNumber` | String | Order number |
| `pdfData` | byte[] | Base64-encoded PDF |

---

## XHistoricalOrder

Past order for display in order history.

### Identification

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | **Yes** | Customer identifier |
| `orderNumber` | String | No | Colect order number |
| `erpOrderReference` | String | No | ERP order reference |
| `customerOrderReference` | String | No | Customer's PO |
| `timestamp` | DateTime | No | Order date |

### Details

| Field | Type | Description |
|-------|------|-------------|
| `salesPerson` | String | Salesperson email |
| `accountManager` | String | Account manager |
| `customerPriceGroup` | String | Price group |
| `collection` | String | Collection name |
| `channel` | String | Order channel |
| `comment` | String | Order comment |
| `externalUrl` | String | Link to external order |
| `internalOrderType` | [XInternalOrderType](enums.md#xinternalordertype) | ORDER or RETURN |
| `orderTypeCode` | String | External order type |

### Related Objects

| Field | Type | Description |
|-------|------|-------------|
| `shippingLocation` | [XLocation](customers.md#xlocation) | Shipping address |
| `agreement` | [XAgreement](customers.md#xagreement) | Agreement used |
| `orderLines` | List&lt;[XHistoricalOrderLine](#xhistoricalorderline)&gt; | Order line items (XML: `<orderLine>`) |

---

## XHistoricalOrderLine

Line item in historical order.

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `productUniqueId` | String | Product style |
| `productColorCode` | String | Color code |
| `productSize` | String | Size |
| `quantity` | Integer | Quantity ordered |
| `quantityDelivered` | Integer | Quantity shipped |
| `netLineAmount` | Float | Net line value |
| `returnAllowed` | Boolean | Can be returned |

### Product Information

| Field | Type | Description |
|-------|------|-------------|
| `productDescription` | String | Product name |
| `productCategory` | String | Category |
| `productGroup` | String | Product group |
| `productGender` | String | Gender |
| `productSeason` | String | Season |
| `productCollection` | String | Collection |
| `productYear` | String | Year |
| `productCrossReference` | String | Cross reference |
| `productImageUrl` | String | Image URL |
| `productSizeSortCode` | Long | Size sort order |
| `productSubSize` | String | Sub-size |
| `productSubSizeSortCode` | Long | Sub-size sort |
| `eanCode` | String | EAN code |

### Pricing

| Field | Type | Description |
|-------|------|-------------|
| `currency` | String | Currency |
| `netWholesalePrice` | Float | Net price |
| `grossWholesalePrice` | Float | Gross price |
| `grossLineAmount` | Float | Gross line value |
| `netPriceDelivered` | Float | Delivered net price |
| `netLineAmountDelivered` | Float | Delivered value |

### Fulfillment

| Field | Type | Description |
|-------|------|-------------|
| `status` | String | Line status (ORDERED, DELIVERED, etc.) |
| `type` | String | Line type (ST, PS, RE) |
| `shipmentDate` | DateTime | Ship date |
| `packingSlipNumber` | String | Packing slip |
| `invoiceNumber` | String | Invoice reference |
| `trackTraceUrl` | String | Tracking URL |
| `sortCode` | Long | Display order |
| `remark` | String | Comment |

### Custom Fields

| Field | Type | Description |
|-------|------|-------------|
| `productCustomField1` | String | Custom field 1 |
| `productCustomField2` | String | Custom field 2 |
| `productCustomField3` | String | Custom field 3 |
| `deliverySubBlock` | String | Delivery block |
| `customerSizeName` | String | Customer size name |
| `customerSubSizeName` | String | Customer sub-size |

### Collections

| Field | Type | XML Element | Description |
|-------|------|-------------|-------------|
| `extraFields` | List&lt;[XHistoricalOrderLineExtraField](#xhistoricalorderlineextrafield)&gt; | `<extraField>` | Custom fields |
| `files` | List&lt;[XRemoteFile](#xremotefile)&gt; | `<remoteFile>` | Attached files |

---

## XInvoice

Customer invoice.

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `customerNo` | String | Customer identifier |
| `invoiceNumber` | String | Invoice number |
| `date` | DateTime | Invoice date |
| `currency` | String | Currency code |
| `outstandingAmount` | Float | Amount owed |

### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `dueDate` | DateTime | Payment due date |
| `amountExcludingTax` | Float | Net amount |
| `taxAmount` | Float | Tax amount |
| `status` | String | Status (OPEN, PAID, OVERDUE) |
| `externalUrl` | String | Link to invoice PDF |
| `customField1` | String | Custom field |
| `customField2` | String | Custom field |
| `customField3` | String | Custom field |
| `customField4` | String | Custom field |

---

## XRemoteFile

Attached file reference.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `url` | String | **Yes** | Absolute URL to where the remote file is located |
| `name` | String | No | Display name of the remote file |
| `size` | Long | No | File size in bytes |
| `type` | String | No | MIME type of the remote file |

---

## XHistoricalOrderLineExtraField

Custom display field on historical order lines.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | String | No | Field identifier |
| `description` | String | No | Display title/label (used as the title when displaying the field) |
| `value` | String | No | Field value |
| `translations` | List&lt;[XExtraFieldTranslation](products.md#xextrafieldtranslation)&gt; | No | Translations of this extra field (XML: `<translation>`) |

{% hint style="info" %}
This type is similar to [XExtraField](products.md#xextrafield) but simplified for historical order display. It does not include grouping or visibility options.
{% endhint %}
