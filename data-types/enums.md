# Enumerations

Complete reference for all enum types in the Colect SOAP API.

***

## Product Enums

### XProductStatus

Product visibility and ordering status.

| Value       | Description                                 |
| ----------- | ------------------------------------------- |
| `ACTIVE`    | Product is orderable (default)              |
| `CANCELED`  | Product canceled, cannot be ordered         |
| `SOLDOUT`   | Product sold out, cannot be ordered         |
| `INVISIBLE` | Hidden, useful for historical order display |
| `BLOCKED`   | Product blocked, cannot be ordered          |
| `BLOCKED_1` | Blocked variant 1                           |
| `BLOCKED_2` | Blocked variant 2                           |
| `BLOCKED_3` | Blocked variant 3                           |

### XColor

Basic color categories for filtering.

| Value        |
| ------------ |
| `BLACK`      |
| `WHITE`      |
| `GRAY`       |
| `RED`        |
| `GREEN`      |
| `BLUE`       |
| `YELLOW`     |
| `ORANGE`     |
| `PURPLE`     |
| `BROWN`      |
| `PINK`       |
| `MULTICOLOR` |
| `BEIGE`      |
| `GOLD`       |
| `SILVER`     |
| `UNKNOWN`    |

### XMediumType

Media/image types for products.

| Value                  | Description                   |
| ---------------------- | ----------------------------- |
| `IMAGE_PRIMARY`        | Main product image (required) |
| `IMAGE_SWATCH`         | Color swatch thumbnail        |
| `IMAGE_BACK`           | Back view                     |
| `IMAGE_MODEL`          | Model wearing product         |
| `IMAGE_MODEL_BACK`     | Model back view               |
| `IMAGE_LEFT`           | Left side view                |
| `IMAGE_RIGHT`          | Right side view               |
| `IMAGE_TOP`            | Top view                      |
| `IMAGE_BOTTOM`         | Bottom view                   |
| `IMAGE_PACK`           | Packaging view                |
| `IMAGE_FIT`            | Fit guide                     |
| `IMAGE_STAMP_LEFT`     | Left corner overlay           |
| `IMAGE_STAMP_RIGHT`    | Right corner overlay          |
| `IMAGE_ADDITIONAL_1`   | Additional image 1            |
| `IMAGE_ADDITIONAL_2`   | Additional image 2            |
| `IMAGE_ADDITIONAL_3`   | Additional image 3            |
| `IMAGE_ADDITIONAL_4`   | Additional image 4            |
| `IMAGE_ADDITIONAL_5`   | Additional image 5            |
| `VIDEO`                | Product video                 |
| `VIDEO_MODEL`          | Model video                   |
| `VIDEO_MODEL_LEAD_IN`  | Model video intro             |
| `VIDEO_MODEL_LEAD_OUT` | Model video outro             |
| `HTML`                 | HTML content                  |

### ProductRelationType

Relationship types between products.

| Value          | Description               |
| -------------- | ------------------------- |
| `MATCHING_SET` | Products that go together |
| `SUCCESSOR`    | New version replaces old  |

***

## Customer Enums

### XCustomerStatus

Customer account status.

| Value                  | Description                       |
| ---------------------- | --------------------------------- |
| `ACTIVE`               | Normal active customer (default)  |
| `WARNING`              | Active with warning message popup |
| `BLOCKED`              | Cannot be selected                |
| `BLOCKED_FOR_ORDERING` | Can view but cannot order         |

### XPaymentStatus

Payment requirement for customer.

| Value                     | Description                  |
| ------------------------- | ---------------------------- |
| `PAYMENT_AFTERWARDS`      | Pay after delivery (default) |
| `PREPAYMENT`              | Prepayment required          |
| `PREPAYMENT_REORDER_ONLY` | Prepayment for reorders only |

### XAccessType

B2B webshop access level.

| Value  | Description |
| ------ | ----------- |
| `FULL` | Full access |
| `NONE` | No access   |

### XUserType

User role types.

| Value                   | Description            |
| ----------------------- | ---------------------- |
| `PARTNER`               | Partner user           |
| `COMPANY_ADMIN`         | Company administrator  |
| `COMPANY_ADMIN_BACKEND` | Backend administrator  |
| `COMPANY_SALESREP`      | Sales representative   |
| `SALES_AGENT`           | Sales agent (mobile)   |
| `SALES_AGENT_WEB`       | Sales agent (web)      |
| `RETAILER`              | Retailer user          |
| `RETAILER_INSTOREMODE`  | Retailer in-store mode |
| `RETAILER_WEB`          | Retailer web user      |

***

## Order Enums

### XOrderStatus

Order processing status.

| Value                       | Description                 |
| --------------------------- | --------------------------- |
| `PROCESSING`                | Being processed             |
| `PROCESSING_MOVED`          | Processed and moved         |
| `COOLING`                   | In cooling period           |
| `COOLING_MOVED`             | Cooling completed and moved |
| `AT_CACHE`                  | Stored in cloud cache       |
| `AT_CACHE_MOVED`            | Cache processed and moved   |
| `AT_CONNECTOR`              | Available via API           |
| `AT_CONNECTOR_MOVED`        | Retrieved and moved         |
| `AT_DESTINATION`            | Marked as processed         |
| `AT_DESTINATION_MOVED`      | Processed and archived      |
| `NO_DESTINATION`            | No connector configured     |
| `NO_DESTINATION_MOVED`      | No destination, archived    |
| `DUPLICATE`                 | Duplicate order detected    |
| `DUPLICATE_MOVED`           | Duplicate, archived         |
| `DRAFT`                     | Draft order                 |
| `DRAFT_MOVED`               | Draft, archived             |
| `TO_BE_EDITED`              | Needs editing               |
| `TO_BE_EDITED_MOVED`        | Edited, archived            |
| `TO_BE_APPROVED`            | Awaiting approval           |
| `TO_BE_APPROVED_MOVED`      | Approval processed          |
| `APPROVAL_DECLINED`         | Approval rejected           |
| `APPROVAL_DECLINED_MOVED`   | Declined, archived          |
| `WAITING_FOR_PAYMENT`       | Awaiting prepayment         |
| `WAITING_FOR_PAYMENT_MOVED` | Payment processed           |
| `PAYMENT_FAILED`            | Payment failed              |
| `PAYMENT_FAILED_MOVED`      | Failed, archived            |

{% hint style="info" %}
**\_MOVED suffix:** Indicates the order has been processed/moved but retained for reference.
{% endhint %}

### XOrderLineType

Type of order line.

| Value          | Description                     |
| -------------- | ------------------------------- |
| `STOCK_ORDER`  | Regular stock order             |
| `PRE_ORDER`    | Pre-order (future availability) |
| `RETURN_ORDER` | Return/credit                   |

### XInternalOrderType

Internal classification of order.

| Value    | Description   |
| -------- | ------------- |
| `ORDER`  | Regular order |
| `RETURN` | Return order  |

***

## Access Rule Enums

### XProductAccessRuleType

Type of access rule.

| Value    | Description                                 |
| -------- | ------------------------------------------- |
| `ADD`    | Grant access to matching products (default) |
| `REMOVE` | Deny access to matching products            |

### XProductAccessRuleMatchOperation

How to match products.

| Value      | Description           |
| ---------- | --------------------- |
| `EQUALS`   | Exact match (default) |
| `CONTAINS` | Substring match       |

### XEnvironment

Target environment for rules.

| Value | Description       |
| ----- | ----------------- |
| `APP` | Mobile app only   |
| `B2B` | B2B webstore only |

{% hint style="info" %}
Omit `environment` to apply rule to both APP and B2B.
{% endhint %}

***

## Pricing Enums

### XPriceType

Price type for updatePrices operation.

| Value                | Description                          |
| -------------------- | ------------------------------------ |
| `RETAIL`             | Suggested retail price               |
| `WHOLESALE`          | Wholesale price                      |
| `ORIGINAL_WHOLESALE` | Original wholesale (before discount) |
| `PURCHASE`           | Purchase/cost price                  |

### XRuleEvaluationMethod

When to evaluate order rules.

| Value                             | Description                 |
| --------------------------------- | --------------------------- |
| `EVALUATE_BASED_ON_TODAY`         | Use current date            |
| `EVALUATE_BASED_ON_DELIVERY_DATE` | Use requested delivery date |

***

## Usage Examples

### Product Status

```xml
<api:product>
    <api:uniqueId>STYLE-001</api:uniqueId>
    <api:colorCode>BLK</api:colorCode>
    <api:status>ACTIVE</api:status>
    <!-- or -->
    <api:status>SOLDOUT</api:status>
</api:product>
```

### Customer Status with Message

```xml
<api:customer>
    <api:customerNo>CUST-001</api:customerNo>
    <api:status>WARNING</api:status>
    <api:statusMessage>Payment overdue - please contact accounting before placing orders.</api:statusMessage>
</api:customer>
```

### Order Line Types

```xml
<api:orderLine>
    <api:type>STOCK_ORDER</api:type>
    <!-- Regular in-stock order -->
</api:orderLine>

<api:orderLine>
    <api:type>PRE_ORDER</api:type>
    <!-- Future availability -->
</api:orderLine>

<api:orderLine>
    <api:type>RETURN_ORDER</api:type>
    <!-- Return/credit -->
</api:orderLine>
```

### Access Rules

```xml
<api:productAccessRule>
    <api:customerNo>CUST-001</api:customerNo>
    <api:type>REMOVE</api:type>
    <api:operation>EQUALS</api:operation>
    <api:environment>B2B</api:environment>
    <api:categoryCode>PREMIUM</api:categoryCode>
</api:productAccessRule>
```
