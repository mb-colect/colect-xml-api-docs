# Enumerations

Complete reference for all enumeration types used in the Colect XML integration.

***

## Product Enums

### Product Status

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

### Color

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

### Medium Type

The `type` field on `<medium>` uses lowercase short names in the import XML. (Colect stores them internally as `IMAGE_PRIMARY` etc., but the import format requires the values below.)

| Value                  | Description                                   |
| ---------------------- | --------------------------------------------- |
| `primary`              | Primary product image                         |
| `swatch`               | Color swatch                                  |
| `model`                | Model shot (front)                            |
| `model_back`           | Model shot (back)                             |
| `left`                 | Left side view                                |
| `right`                | Right side view                               |
| `back`                 | Back view                                     |
| `pack`                 | Packaging shot                                |
| `top`                  | Top view                                      |
| `bottom`               | Bottom view                                   |
| `fit`                  | Fit/detail shot                               |
| `stamp_left`           | Stamp overlay (left)                          |
| `stamp_right`          | Stamp overlay (right)                         |
| `additional_1`         | Additional image slot 1                       |
| `additional_2`         | Additional image slot 2                       |
| `additional_3`         | Additional image slot 3                       |
| `additional_4`         | Additional image slot 4                       |
| `additional_5`         | Additional image slot 5                       |
| `video`                | Product video                                 |
| `video_model`          | Model video                                   |
| `video_model_lead_in`  | Model video lead-in clip                      |
| `video_model_lead_out` | Model video lead-out clip                     |
| `html`                 | Embedded HTML content                         |

### ProductRelationType

Relationship types between products.

| Value          | Description               |
| -------------- | ------------------------- |
| `MATCHING_SET` | Products that go together |
| `SUCCESSOR`    | New version replaces old  |

***

## Customer Enums

### Customer Status

Customer account status.

| Value                  | Description                       |
| ---------------------- | --------------------------------- |
| `ACTIVE`               | Normal active customer (default)  |
| `WARNING`              | Active with warning message popup |
| `BLOCKED`              | Cannot be selected                |
| `BLOCKED_FOR_ORDERING` | Can view but cannot order         |

### Payment Status

Payment requirement for customer.

| Value                     | Description                  |
| ------------------------- | ---------------------------- |
| `PAYMENT_AFTERWARDS`      | Pay after delivery (default) |
| `PREPAYMENT`              | Prepayment required          |
| `PREPAYMENT_REORDER_ONLY` | Prepayment for reorders only |

### Access Type

B2B webshop access level.

| Value  | Description |
| ------ | ----------- |
| `FULL` | Full access |
| `NONE` | No access   |

### User Type

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

### Order Status

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

### Order Line Type

Type of order line.

| Value          | Description                     |
| -------------- | ------------------------------- |
| `STOCK_ORDER`  | Regular stock order             |
| `PRE_ORDER`    | Pre-order (future availability) |
| `RETURN_ORDER` | Return/credit                   |

### Internal Order Type

Internal classification of order.

| Value    | Description   |
| -------- | ------------- |
| `ORDER`  | Regular order |
| `RETURN` | Return order  |

***

## Access Rule Enums

### Product Access Rule Type

Type of access rule.

| Value    | Description                                 |
| -------- | ------------------------------------------- |
| `ADD`    | Grant access to matching products (default) |
| `REMOVE` | Deny access to matching products            |

### Product Access Rule Match Operation

How to match products.

| Value      | Description           |
| ---------- | --------------------- |
| `EQUALS`   | Exact match (default) |
| `CONTAINS` | Substring match       |

### Environment

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

### Price Type

Price type identifier for price records.

| Value                | Description                          |
| -------------------- | ------------------------------------ |
| `RETAIL`             | Suggested retail price               |
| `WHOLESALE`          | Wholesale price                      |
| `ORIGINAL_WHOLESALE` | Original wholesale (before discount) |
| `PURCHASE`           | Purchase/cost price                  |

### Rule Evaluation Method

When to evaluate order rules.

| Value                             | Description                 |
| --------------------------------- | --------------------------- |
| `EVALUATE_BASED_ON_TODAY`         | Use current date            |
| `EVALUATE_BASED_ON_DELIVERY_DATE` | Use requested delivery date |

***

## Usage Examples

### Product Status

```xml
<product>
    <uniqueId>STYLE-001</uniqueId>
    <colorCode>BLK</colorCode>
    <status>ACTIVE</status>
    <!-- or -->
    <status>SOLDOUT</status>
</product>
```

### Customer Status with Message

```xml
<customer>
    <customerNo>CUST-001</customerNo>
    <status>WARNING</status>
    <statusMessage>Payment overdue - please contact accounting before placing orders.</statusMessage>
</customer>
```

### Order Line Types

```xml
<orderLine>
    <type>STOCK_ORDER</type>
    <!-- Regular in-stock order -->
</orderLine>

<orderLine>
    <type>PRE_ORDER</type>
    <!-- Future availability -->
</orderLine>

<orderLine>
    <type>RETURN_ORDER</type>
    <!-- Return/credit -->
</orderLine>
```

### Access Rules

```xml
<productAccessRule>
    <customerNo>CUST-001</customerNo>
    <type>REMOVE</type>
    <operation>EQUALS</operation>
    <environment>B2B</environment>
    <categoryCode>PREMIUM</categoryCode>
</productAccessRule>
```
