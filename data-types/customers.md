# Customer Types

Complete reference for all customer-related data types.

{% hint style="info" %}
**Field Length Limits:** All string fields have maximum length limits. Key limits: `customerNo` (32), `name` (128), `email` (128), `postalCode` (32), `userDefinedField` (2048). See [Field Length Limits](overview.md#field-length-limits) for the complete reference.
{% endhint %}

---

## Customer

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
| `status` | [Customer Status](enums.md#customer-status) | Customer status |
| `statusMessage` | String | Message shown for WARNING status |
| `paymentStatus` | [Payment Status](enums.md#payment-status) | Payment requirement |
| `taxPercentage` | Float | Tax rate for prepayment |

### Order Settings

| Field | Type | Description |
|-------|------|-------------|
| `minOrderValue` | Float | Minimum order value |
| `maxOrderValue` | Float | Maximum order value |
| `shippingTime` | Integer | Days to add to delivery date |
| `minimumQuantitiesKey` | String | Key for product minimum quantities |
| `sizeNamingCode` | String | Code for customer-specific size naming. See [Customer Size Naming](products.md#customer-size-naming). |
| `externalReference` | String | External system reference |

### Nested Collections

| Field | Type | XML Element | Description |
|-------|------|-------------|-------------|
| `shippingLocations` | List&lt;[Location](#location)&gt; | `<shippingLocation>` | Shipping addresses |
| `contacts` | List&lt;[Contact](#contact)&gt; | `<contact>` | Customer contacts |
| `agreements` | List&lt;[Agreement](#agreement)&gt; | `<agreement>` | Customer agreements |
| `discountGroups` | List&lt;[Discount Group](#discount-group)&gt; | `<discountGroup>` | Discount configurations |
| `marginGroups` | List&lt;[Margin Group](#margin-group)&gt; | `<marginGroup>` | Margin configurations |
| `approvalGroups` | List&lt;[Customer Approval Group](#customer-approval-group)&gt; | `<approvalGroup>` | Approval workflows |
| `budgetComponents` | List&lt;[Budget Component](#budget-component)&gt; | `<budgetComponent>` | Budget tracking |
| `extraFields` | List&lt;[Extra Field](products.md#extra-field)&gt; | `<extraField>` | Custom display fields |
| `tags` | List&lt;[Tag](#tag)&gt; | `<tag>` | Customer tags |
| `customChoices` | List&lt;[Custom Choice](#custom-choice)&gt; | `<customChoice>` | Custom options |
| `sizeAccessCodes` | List&lt;String&gt; | `<sizeAccessCode>` | Size visibility codes |
| `accessibleDeliveryWindowCodes` | List&lt;String&gt; | `<accessibleDeliveryWindowCode>` | Delivery window access |
| `primaryUserEmailAddresses` | List&lt;String&gt; | `<primaryUserEmailAddress>` | Primary user emails |
| `orderDiscountRules` | List&lt;[Order Amount Modification Rule](#order-amount-modification-rule)&gt; | `<orderDiscountRule>` | Order-level discounts |
| `orderDeliveryCostRules` | List&lt;[Order Amount Modification Rule](#order-amount-modification-rule)&gt; | `<orderDeliveryCostRule>` | Shipping charges |

---

## Location

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

## Contact

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
| `accessType` | [Access Type](enums.md#access-type) | No | B2B webshop access (FULL/NONE) |

---

## Agreement

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
| `marginGroups` | List&lt;[Margin Group](#margin-group)&gt; | No | Margin configurations |
| `discountGroups` | List&lt;[Discount Group](#discount-group)&gt; | No | Discount configurations |

---

## Discount Group

Automatic discount configuration.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | No | Matches product's discountGroupCode (default: '*') |
| `amount` | Float | **Yes** | Discount percentage |
| `minQuantity` | Integer | No | Minimum qty for discount |
| `startDate` | DateTime | No | Discount start date |
| `endDate` | DateTime | No | Discount end date |
| `description` | String | No | Display description |
| `identifier` | String | No | Identifier returned on orders |

---

## Margin Group

Margin-based pricing (calculate wholesale from retail).

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | No | Matches product's marginGroupCode (default: '*') |
| `amount` | Float | **Yes** | Margin multiplier (e.g., 2.3 = retail/2.3) |

---

## Order Amount Modification Rule

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
| `evaluationMethod` | [Rule Evaluation Method](enums.md#rule-evaluation-method) | **Yes** | When to evaluate |
| `amount` | Float | No | Fixed amount |
| `discountPercentage` | Float | No | Percentage discount |

---

## Customer Approval Group

Links customer to approval workflows.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `approvalGroupCode` | String | **Yes** | Matches product's approval group |
| `approverTeamCode` | String | **Yes** | Team responsible for approval |

---

## Budget Component

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

## Custom Choice

Custom option/dropdown for customer.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `code` | String | **Yes** | Choice identifier |
| `value` | String | **Yes** | Display value |
| `group` | String | **Yes** | Choice group |
| `sortCode` | Integer | No | Display order |
| `defaultChoice` | Boolean | **Yes** | Is default selection |
| `userTypes` | List&lt;[User Type](enums.md#user-type)&gt; | No | Applicable user types |

---

## Tag

Simple tag for categorization.

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | String | **Yes** | Tag name |

---

## Access Rule Types

### Product Access Rule

Controls which products a customer can see.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | **Yes** | Customer identifier |
| `type` | [Product Access Rule Type](enums.md#product-access-rule-type) | No | ADD or REMOVE (default: ADD) |
| `operation` | [Product Access Rule Match Operation](enums.md#product-access-rule-match-operation) | No | EQUALS or CONTAINS |
| `environment` | [Environment](enums.md#environment) | No | APP, B2B, or both |
| `uniqueId` | String | No | Product filter |
| `colorCode` | String | No | Color filter |
| `categoryCode` | String | No | Category filter |
| `productGroupCode` | String | No | Product group filter |
| `brandId` | String | No | Brand filter |
| `seasonCode` | String | No | Season filter |
| `sortCode` | Integer | No | Rule evaluation order |

### Customer Access Record

Agent's access to a customer.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | String | **Yes** | Customer identifier |
| `shippingLocationCode` | String | No | Restrict to specific location |

### Agent Customer Access

Agent's complete customer access configuration.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `agentEmail` | String | **Yes** | Agent email address |
| `agentDefaultCurrency` | String | No | Default browsing currency |
| `agentDefaultPriceGroup` | String | No | Default browsing price group |
| `customerAccessRecords` | List&lt;[Customer Access Record](#customer-access-record)&gt; | No | Accessible customers |
