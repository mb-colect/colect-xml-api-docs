# Invoices

The **Invoices** input document pushes invoice data from your ERP into Colect for customer visibility. Customers can see their invoices and outstanding balances inside the Colect Sales App and B2B Webstore — but the ledger of record stays in your ERP.

{% hint style="success" %}
**Example:** [`examples/invoices.xml`](../examples/invoices.xml) — open, paid, and overdue invoices in EUR plus a USD invoice for a different customer.
{% endhint %}

{% hint style="info" %}
Colect does not generate invoices. It only displays the invoice data your ERP pushes here. Lifecycle changes (paid, voided, credited) are reflected by re-sending the relevant invoice with an updated `status` and `outstandingAmount`.
{% endhint %}

***

## Document at a glance

| Property                | Value                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------- |
| **Direction**           | ERP → Colect                                                                          |
| **Root element**        | `<invoices>`                                                                          |
| **File name pattern**   | `invoices*.xml`                                                                       |
| **Sync mode**           | Full set — invoices not in the document are removed from display                       |
| **XSD**                 | [Invoices\_Feed\_XSD.xsd](../schema/snapshots/2026-05-06/Invoices_Feed_XSD.xsd)       |
| **Identifier**          | `invoiceNumber`                                                                       |

***

## Invoice fields

| Element               | Type       | Mandatory | Description                                                                                                            |
| --------------------- | ---------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `invoiceNumber`       | `string`   | **YES**   | Unique invoice number from the ERP. Acts as the identifier across re-pushes.                                           |
| `customerNo`          | `string`   | **YES**   | Customer number — must match an existing customer in the [Customers](customers.md) document.                           |
| `status`              | `string`   | NO        | Invoice status (e.g. `OPEN`, `PAID`, `OVERDUE`, `VOIDED`, `CREDITED`). Free text — your collection's enums apply.      |
| `date`                | `dateTime` | **YES**   | Invoice date. Format `yyyyMMdd`.                                                                                       |
| `dueDate`             | `dateTime` | NO        | Due date for payment. Format `yyyyMMdd`.                                                                               |
| `currency`            | `string`   | **YES**   | Currency code (e.g. `EUR`, `USD`).                                                                                     |
| `amountExcludingTax`  | `float`    | NO        | Amount excluding tax.                                                                                                  |
| `taxAmount`           | `float`    | NO        | Tax amount.                                                                                                            |
| `outstandingAmount`   | `float`    | **YES**   | Amount still owed. Set to `0` when an invoice is paid.                                                                 |
| `externalUrl`         | `string`   | NO        | URL to a hosted PDF or invoice page. When set, the app surfaces a "View invoice" link.                                 |

***

## Basic XML structure

```xml
<?xml version="1.0" encoding="UTF-8"?>
<invoices>
  <invoice>
    <invoiceNumber>INV-2026-00425</invoiceNumber>
    <status>OPEN</status>
    <customerNo>C-00042</customerNo>
    <date>20260420</date>
    <dueDate>20260520</dueDate>
    <currency>EUR</currency>
    <amountExcludingTax>1250.00</amountExcludingTax>
    <taxAmount>262.50</taxAmount>
    <outstandingAmount>1512.50</outstandingAmount>
    <externalUrl>https://invoices.example.com/INV-2026-00425.pdf</externalUrl>
  </invoice>

  <invoice>
    <invoiceNumber>INV-2026-00318</invoiceNumber>
    <status>PAID</status>
    <customerNo>C-00042</customerNo>
    <date>20260315</date>
    <dueDate>20260415</dueDate>
    <currency>EUR</currency>
    <amountExcludingTax>800.00</amountExcludingTax>
    <taxAmount>168.00</taxAmount>
    <outstandingAmount>0</outstandingAmount>
  </invoice>
</invoices>
```

***

## Validation tips

{% hint style="warning" %}
**Customer numbers must already exist** in the most recent Customers document. An invoice referencing an unknown `customerNo` is silently dropped from the user-facing view.
{% endhint %}

{% hint style="info" %}
**To remove an invoice** (e.g. it was issued in error), simply omit it from the next document — the full-set sync model removes it from display. The ERP remains the source of truth.
{% endhint %}
