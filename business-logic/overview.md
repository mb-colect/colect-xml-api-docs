# Business Logic — Overview

The Business Logic section documents **how the platform behaves** once your data is ingested — the features, configurations, and rules that operate on top of the raw XML documents. These pages are for ERP integrators who need to understand what data shape unlocks which feature, and for support staff explaining behavior to customers.

{% hint style="info" %}
**Documents tell you the shape, business logic tells you the consequences.** A feature like Multi Promotions doesn't change any field — it changes how the existing `<discountGroups>` block is presented to the user. This section explains the link between data and behavior.
{% endhint %}

***

## Pages in this section

| Page                                          | What it covers                                                                                              |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| [Multi Promotions](multi-promotions.md)       | User-selectable line-level discounts, driven by the customer's discount groups.                             |
| [Labels](labels.md)                           | Display labels surfaced on products, customers, or orders for filtering and visual grouping.                |
| [Approvals](approvals.md)                     | Order approval workflows — when orders need a manager to sign off before they reach the ERP.                |

***

## Cross-cutting principles

A few patterns recur across business-logic features:

* **Data-driven gating.** Most features turn on automatically when you supply the right data. There's rarely a separate "enable Multi Promotions" flag — the feature appears when a customer has two or more eligible discount groups.
* **Identifiers echo back.** Whenever a feature lets the user make a choice (which discount, which delivery window, which custom choice), the chosen identifier is written back on the [Orders output document](../documents/orders.md) so your ERP can audit what happened.
* **Collection-level configuration.** A handful of features have collection-level toggles your Colect contact configures (e.g. `discountGroupFilter` for Multi Promotions). These are noted on each feature's page.

***

## Where the Admin Documentation fits

The Admin Documentation (the GitBook section labelled **Admin Documentation**) covers the same features from the **end-user perspective** — how a sales rep or buyer interacts with the feature in the app or B2B Webstore.

When you're documenting an integration, both perspectives are useful:

| You are…                                  | Read first                                              |
| ----------------------------------------- | ------------------------------------------------------- |
| An ERP integrator                         | This section + the relevant [Documents](../documents/products.md) page |
| A salesperson or end-user                 | The Admin Documentation feature page                    |
| A support agent triaging a customer issue | Both — start with admin docs to understand the user flow, then come here for the data shape |

Where helpful, individual business-logic pages link out to the matching Admin Documentation page.
