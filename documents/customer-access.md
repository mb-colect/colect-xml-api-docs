# Customer Access

The **Customer Access** input document maps app users (sales reps, agents) to the customers they're allowed to sell to. It's the access-control layer for the Sales App and B2B Webstore — without an entry here, a user can't see or transact on a given customer.

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<customerAccess>`                                                                    |
| **File name pattern**   | `customerAccess*.xml`                                                                 |
| **Sync mode**           | Full set — access entries not in the document are revoked                              |
| **XSD**                 | [Customer\_Access\_XSD.xsd](../schema/snapshots/2026-05-06/Customer_Access_XSD.xsd)   |
| **Identifier**          | `email` of the user                                                                   |

{% hint style="info" %}
**Pairs with [Product Access](product-access.md):** Customer Access controls *which customers a user can sell to*; Product Access controls *which products a customer can see*. Both must be configured for an order to be possible.
{% endhint %}

***

## Top-level access fields

The root carries one `<access>` element per app user.

| Element                | Type     | Mandatory | Description                                                                                                  |
| ---------------------- | -------- | --------- | ------------------------------------------------------------------------------------------------------------ |
| `email`                | `string` | **YES**   | Email address of the user (sales rep, agent, B2B login). Used as the user identifier across Colect surfaces. |
| `defaultPriceGroup`    | `string` | NO        | Price group used when the user has no agreement with a specific customer.                                    |
| `defaultCurrency`      | `string` | NO        | Currency used when the user has no agreement with a specific customer.                                       |

***

## Customers sub-block

Each `<access>` carries a list of `<customer>` entries — the customers this user is allowed to work with. Each entry can optionally narrow access to specific shipping locations.

| Element                  | Type     | Mandatory | Description                                                                                            |
| ------------------------ | -------- | --------- | ------------------------------------------------------------------------------------------------------ |
| `customerNo`             | `string` | **YES**   | The `customerNo` from the Customers document.                                                          |
| `shippingLocationCode`   | `string` | NO        | Restrict access to one or more shipping locations. Repeat the element for each allowed location.       |

{% hint style="info" %}
If `shippingLocationCode` is omitted, the user has access to **all** shipping locations on that customer. If one or more are specified, the user is restricted to those locations only.
{% endhint %}

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<customerAccess>
  <access>
    <email>jane.rep@example.com</email>
    <defaultPriceGroup>WHOLESALE_EU</defaultPriceGroup>
    <defaultCurrency>EUR</defaultCurrency>
    <customers>
      <customer>
        <customerNo>C-00001</customerNo>
        <!-- No shippingLocationCode → all locations allowed -->
      </customer>
      <customer>
        <customerNo>C-00042</customerNo>
        <shippingLocationCode>WAREHOUSE</shippingLocationCode>
        <shippingLocationCode>POPUP-NYC</shippingLocationCode>
      </customer>
    </customers>
  </access>

  <access>
    <email>tom.manager@example.com</email>
    <defaultPriceGroup>WHOLESALE_GLOBAL</defaultPriceGroup>
    <defaultCurrency>USD</defaultCurrency>
    <customers>
      <!-- Tom can sell to every customer in the document -->
      <customer>
        <customerNo>C-00001</customerNo>
      </customer>
      <customer>
        <customerNo>C-00042</customerNo>
      </customer>
      <customer>
        <customerNo>C-00100</customerNo>
      </customer>
    </customers>
  </access>
</customerAccess>
```

***

## Validation tips

{% hint style="warning" %}
**Customer numbers must already exist** in the most recent Customers document at the time access is evaluated. If a Customer Access entry references an unknown `customerNo`, that entry is silently ignored.
{% endhint %}

{% hint style="info" %}
**Email matching is case-insensitive** but normalize to lowercase in your ERP exports for predictable diffing between snapshots.
{% endhint %}
