# Customer Types

Complete reference for all customer-related data types.

{% hint style="info" %}
**Field Length Limits:** All string fields have maximum length limits. Key limits: `customerNo` (32), `name` (128), `email` (128), `postalCode` (32), `userDefinedField` (2048). See [Field Length Limits](overview.md#field-length-limits) for the complete reference.
{% endhint %}

---

## XCustomer

The main customer entity representing a B2B account.

### Mandatory Fields

| Field | Type | Description |
|-------|------|-------------|
| `customerNo` | String | Unique customer identifier (from ERP) |
| `name` | String | Company/customer name |
| `email` | String | Primary email address |
| `currencyCode` | String | Default currency. Must exactly match the currency codes used in collection settings and product/price data (e.g., EUR, USD, GBP). |

### Address Fields

| Field | Type | Description |
|-------|------|-------------|
| `address` | String | Full address (use when structured fields unavailable) |
| `street` | String | Street name |
| `houseNumber` | String | House/building number |
| `houseNumberSuffix` | String | Apartment, suite, etc. |
| `state` | String | State/province |
| `postalCode` | String | Postal/ZIP code |
| `city` | String | City |
| `country` | String | Country |
| `phone` | String | Phone number |
| `gln` | String | Global Location Number (GLN) |

### Pricing Fields

| Field | Type | Description |
|-------|------|-------------|
| `activePriceGroup` | String | Current price group |
| `nextPriceGroup` | String | Upcoming price group |
| `nextPriceGroupDate` | DateTime | Date next group activates |
| `retailCurrencyCode` | String | Currency for retail prices |

### Display Fields

| Field | Type | Description |
|-------|------|-------------|
| `searchName` | String | Searchable text |
| `language` | String | Language code (en, nl, de, fr, da, no) |
| `channel` | String | Sales channel |
| `userDefinedField` | String | Custom field |
| `orderFooter` | String | Text on order confirmations |

### Status Fields

| Field | Type | Description |
|-------|------|-------------|
| `status` | [XCustomerStatus](enums.md#xcustomerstatus) | Customer status |
| `statusMessage` | String | Message shown for WARNING status |
| `paymentStatus` | [XPaymentStatus](enums.md#xpaymentstatus) | Payment requirement |
| `taxPercentage` | Float | Tax rate for prepayment |

### Order Settings

| Field | Type | Description |
|-------|------|-------------|
| `minOrderValue` | Float | Minimum order value |
| `maxOrderValue` | Float | Maximum order value |
| `shippingTime` | Integer | Days to add to delivery date |
| `minimumQuantitiesKey` | String | Key for product minimum quantities |
| `sizeNamingCode` | String | Code for customer-specific size naming. See [XCustomerSizeNaming](products.md#xcustomersizeNaming). |
| `externalReference` | String | External system reference |

### Nested Collections

| Field | Type | XML Element | Description |
|-------|------|-------------|-------------|
| `shippingLocations` | List&lt;[XLocation](#xlocation)&gt; | `<shippingLocation>` | Shipping addresses |
| `contacts` | List&lt;[XContact](#xcontact)&gt; | `<contact>` | Customer contacts |
| `agreements` | List&lt;[XAgreement](#xagreement)&gt; | `<agreement>` | Customer agreements |
| `discountGroups` | List&lt;[XDiscountGroup](#xdiscountgroup)&gt; | `<discountGroup>` | Discount configurations |
| `marginGroups` | List&lt;[XMarginGroup](#xmargingroup)&gt; | `<marginGroup>` | Margin configurations |
| `approvalGroups` | List&lt;[XCustomerApprovalGroup](#xcustomerapprovalgroup)&gt; | `<approvalGroup>` | Approval workflows |
| `budgetComponents` | List&lt;[XBudgetComponent](#xbudgetcomponent)&gt; | `<budgetComponent>` | Budget tracking |
| `extraFields` | List&lt;[XExtraField](products.md#xextrafield)&gt; | `<extraField>` | Custom display fields |
| `tags` | List&lt;[XTag](#xtag)&gt; | `<tag>` | Customer tags |
| `customChoices` | List&lt;[XCustomChoice](#xcustomchoice)&gt; | `<customChoice>` | Custom options |
| `sizeAccessCodes` | List&lt;String&gt; | `<sizeAccessCode>` | Size visibility codes |
| `accessibleDeliveryWindowCodes` | List&lt;String&gt; | `<accessibleDeliveryWindowCode>` | Delivery window access |
| `primaryUserEmailAddresses` | List&lt;String&gt; | `<primaryUserEmailAddress>` | Primary user emails |
| `orderDiscountRules` | List&lt;[XOrderAmountModificationRule](#xorderamountmodificationrule)&gt; | `<orderDiscountRule>` | Order-level discounts |
| `orderDeliveryCostRules` | List&lt;[XOrderAmountModificationRule](#xorderamountmodificationrule)&gt; | `<orderDeliveryCostRule>` | Shipping charges |

---

## XLocation

Shipping location/address for a customer.

### Mandatory Fields

| Field | Type | Description |
|-------|------|-------------|
| `code` | String | Unique location identifier |
| `name` | String | Location name |
| `defaultLocation` | Boolean | Is default shipping location |

### Address Fields

| Field | Type | Description |
|-------|------|-------------|
| `address` | String | Full address |
| `street` | String | Street |
| `houseNumber` | String | Number |
| `houseNumberSuffix` | String | Suffix |
| `state` | String | State/province |
| `postalCode` | String | Postal code |
| `city` | String | City |
| `country` | String | Country |

### Contact Fields

| Field | Type | Description |
|-------|------|-------------|
| `email` | String | Email |
| `telephone` | String | Phone |
| `contactFirstName` | String | Contact first name |
| `contactLastName` | String | Contact last name |
| `contactTelephone` | String | Contact phone |
| `gln` | String | Global Location Number (GLN) |

### Other Fields

| Field | Type | Description |
|-------|------|-------------|
| `externalReference` | String | External system reference |

---

## XContact

Contact person for a customer.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | **Yes** | Unique contact identifier |
| `firstName` | String | No | First name |
| `lastName` | String | No | Last name |
| `role` | String | No | Role/title |
| `phone` | String | No | Phone number |
| `email` | String | No | Email address |
| `accessType` | [XAccessType](enums.md#xaccesstype) | No | B2B webshop access (FULL/NONE) |

---

## XAgreement

Customer-specific agreement with terms and pricing.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | **Yes** | Agreement identifier (returned on orders) |
| `description` | String | **Yes** | Display description |
| `channel` | String | No | Sales channel |
| `activePriceGroup` | String | No | Price group for agreement |
| `nextPriceGroup` | String | No | Upcoming price group |
| `nextPriceGroupDate` | DateTime | No | Activation date |
| `currencyCode` | String | No | Agreement currency |
| `orderFooter` | String | No | Text for confirmations |
| `marginGroups` | List&lt;[XMarginGroup](#xmargingroup)&gt; | No | Margin configurations |
| `discountGroups` | List&lt;[XDiscountGroup](#xdiscountgroup)&gt; | No | Discount configurations |

---

## XDiscountGroup

Automatic discount configuration.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | No | Matches product's discountGroupCode (default: '*') |
| `percentage` | Float | **Yes** | Discount percentage |
| `minimumQuantity` | Integer | No | Minimum qty for discount |
| `startDate` | DateTime | No | Discount start date |
| `endDate` | DateTime | No | Discount end date |
| `description` | String | No | Display description |
| `identifier` | String | No | Identifier returned on orders |

---

## XMarginGroup

Margin-based pricing (calculate wholesale from retail).

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | No | Matches product's marginGroupCode (default: '*') |
| `amount` | Float | **Yes** | Margin multiplier (e.g., 2.3 = retail/2.3) |

---

## XOrderAmountModificationRule

Rules for order-level discounts or shipping charges.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `identification` | String | **Yes** | Rule identifier |
| `description` | String | **Yes** | Display description |
| `warning` | String | No | Warning message |
| `group` | String | No | Rule group |
| `exclusive` | Boolean | **Yes** | Only one rule in group applies |
| `minimumQuantity` | Integer | No | Minimum order quantity |
| `minimumOrderAmount` | Float | No | Minimum order value |
| `startDate` | DateTime | No | Rule start date |
| `endDate` | DateTime | No | Rule end date |
| `evaluationMethod` | [XRuleEvaluationMethod](enums.md#xruleevaluationmethod) | **Yes** | When to evaluate |
| `amount` | Float | No | Fixed amount |
| `discountPercentage` | Float | No | Percentage discount |

---

## XCustomerApprovalGroup

Links customer to approval workflows.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `approvalGroupCode` | String | **Yes** | Matches product's approval group |
| `approverTeamCode` | String | **Yes** | Team responsible for approval |

---

## XBudgetComponent

Budget tracking for customer.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `key` | String | **Yes** | Unique component identifier |
| `amount` | Float | **Yes** | Budget amount |
| `actual` | Float | **Yes** | Actual spent |
| `canceled` | Float | **Yes** | Canceled amount |
| `season` | String | No | Season |
| `seasonSortCode` | Integer | No | Season sort order |
| `productGroup` | String | No | Product group |
| `category` | String | No | Category |
| `salesPerson` | String | No | Salesperson |
| `editable` | Boolean | **Yes** | User can edit budget |

---

## XCustomChoice

Custom option/dropdown for customer.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | **Yes** | Choice identifier |
| `value` | String | **Yes** | Display value |
| `group` | String | **Yes** | Choice group |
| `sortCode` | Integer | No | Display order |
| `defaultChoice` | Boolean | **Yes** | Is default selection |
| `userTypes` | List&lt;[XUserType](enums.md#xusertype)&gt; | No | Applicable user types |

---

## XTag

Simple tag for categorization.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | String | **Yes** | Tag name |

---

## Access Rule Types

### XProductAccessRule

Controls which products a customer can see.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | **Yes** | Customer identifier |
| `type` | [XProductAccessRuleType](enums.md#xproductaccessruletype) | No | ADD or REMOVE (default: ADD) |
| `operation` | [XProductAccessRuleMatchOperation](enums.md#xproductaccessrulematchoperation) | No | EQUALS or CONTAINS |
| `environment` | [XEnvironment](enums.md#xenvironment) | No | APP, B2B, or both |
| `uniqueId` | String | No | Product filter |
| `colorCode` | String | No | Color filter |
| `categoryCode` | String | No | Category filter |
| `productGroupCode` | String | No | Product group filter |
| `brandId` | String | No | Brand filter |
| `seasonCode` | String | No | Season filter |
| `sortCode` | Integer | No | Rule evaluation order |

### XCustomerAccessRecord

Agent's access to a customer.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | **Yes** | Customer identifier |
| `shippingLocationCode` | String | No | Restrict to specific location |

### XAgentCustomerAccess

Agent's complete customer access configuration.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `agentEmail` | String | **Yes** | Agent email address |
| `agentDefaultCurrency` | String | No | Default browsing currency |
| `agentDefaultPriceGroup` | String | No | Default browsing price group |
| `customerAccessRecords` | List&lt;[XCustomerAccessRecord](#xcustomeraccessrecord)&gt; | No | Accessible customers |
