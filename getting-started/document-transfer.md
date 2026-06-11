# Document Transfer

The Colect XML API moves data between your ERP and the Colect platform as **XML documents**. The documents themselves are the same regardless of how they are transferred — what changes between integrations is the transport channel.

***

## Supported transports

| Transport                       | When to use                                                                       | Notes                                                       |
| ------------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Colect Secure FTP (SFTP)**    | Default for most ERP integrations. Good fit for batch exports, scheduled imports. | Credentials issued by Colect Support. Encrypted in transit. |
| **Other file-transfer**         | Custom integrations (S3 drop, internal file share, etc.) handled per project.     | Discussed during integration kick-off.                      |

{% hint style="info" %}
The XML document structure is **identical** regardless of transport channel. You write the document once and ship it however your environment supports.
{% endhint %}

***

## Connection details (SFTP)

<table><thead><tr><th width="220">Property</th><th>Value</th></tr></thead><tbody><tr><td><strong>Host</strong></td><td><code>sftp.colect.services</code></td></tr><tr><td><strong>Port</strong></td><td><code>22</code></td></tr><tr><td><strong>Protocol</strong></td><td>SFTP (SSH File Transfer Protocol)</td></tr><tr><td><strong>Authentication</strong></td><td>Username + password</td></tr><tr><td><strong>Encryption</strong></td><td>SSH (TLS not applicable to SFTP)</td></tr></tbody></table>

{% hint style="warning" %}
**Credentials are collection-specific.** Each Colect collection has its own SFTP user. Treat the credentials as confidential and never expose them in client-side code or public repositories.
{% endhint %}

***

## File naming and routing

The Cloud Connector identifies document type from the **root XML element** of each file — not from the filename. A file named `Product_sale.xml` is processed identically to `products_2026-06-01.xml` if both have `<products>` as the root element.

The patterns below are recommended conventions to keep `datafiles/` readable. You are free to extend them with timestamps, batch IDs, or collection codes to suit your ERP's export naming.

| Document                                  | Recommended pattern                | Root element          | Direction        |
| ----------------------------------------- | ---------------------------------- | --------------------- | ---------------- |
| Products                                  | `products*.xml`                    | `<products>`          | ERP → Colect     |
| Customers                                 | `customers*.xml`                   | `<customers>`         | ERP → Colect     |
| Customer Access                           | `customerAccess*.xml`              | `<customerAccesses>`  | ERP → Colect     |
| Product Access                            | `productAccess*.xml`               | `<productAccesses>`   | ERP → Colect     |
| Product Relations _(new in v1.5)_         | `productRelations*.xml`            | `<productRelations>`  | ERP → Colect     |
| Prices (sub-feed)                         | `prices*.xml`                      | `<prices>`            | ERP → Colect     |
| Sizes & Stock (sub-feed)                  | `sizes*.xml`                       | `<sizes>`             | ERP → Colect     |
| Extra Fields (sub-feed)                   | `extraFields*.xml`                 | `<extraFields>`       | ERP → Colect     |
| Invoices                                  | `invoices*.xml`                    | `<invoices>`          | ERP → Colect     |
| Historical Orders                         | `historicalOrders*.xml`            | `<historicalOrders>`  | ERP → Colect     |
| Orders                                    | `[orderNumber]-[random].xml`       | —                     | Colect → ERP     |

{% hint style="info" %}
**Colect → ERP files (Orders):** Each order is written as a separate file named `[orderNumber]-[random].xml` — for example `12345678-999.xml`. Your ERP should process any `.xml` file it finds in `orderfiles/` rather than matching a fixed pattern.
{% endhint %}

***

## Directory layout

The default SFTP layout looks like this:

```
/                  ← root (your SFTP user lands here)
├── datafiles/     ← drop XML files here for Colect to ingest
├── images/        ← product image assets
└── orderfiles/    ← Colect drops order files here for your ERP to pick up
```

{% hint style="info" %}
Subfolders may be added during implementation for specific integration needs (e.g. staging vs. production paths). Confirm the exact layout with your Colect Support contact during onboarding.
{% endhint %}

***

## Ingestion lifecycle

```
   ┌──────────────────┐         ┌──────────────────┐         ┌──────────────────┐
   │ ERP writes file  │  ────▶  │  datafiles/      │  ────▶  │ Cloud Connector  │
   └──────────────────┘         └──────────────────┘         └────────┬─────────┘
                                                                       │
                                            ┌──────────────────────────┴─────────────────────────┐
                                            │                                                    │
                                            ▼                                                    ▼
                                  ┌──────────────────┐                                ┌──────────────────┐
                                  │  Validation OK   │                                │ Validation FAIL  │
                                  │ → ingest data    │                                │ → error reported │
                                  └──────────────────┘                                └──────────────────┘
```

***

## Output documents (Orders)

Orders flow the other direction — Colect writes one file per order into `orderfiles/`, named `[orderNumber]-[random].xml`. Your ERP polls that folder on a schedule and processes any `.xml` files it finds.

A common cadence is every 5–15 minutes. Faster polling is supported but rarely necessary; orders placed in the Sales App or B2B Webstore appear in `orderfiles/` within seconds of being signed.

***

## Verifying connectivity

The fastest way to confirm your SFTP credentials work is to upload a minimal Products document:

```bash
sftp colect-user@sftp.colect.services
sftp> cd datafiles
sftp> put products_smoketest.xml
sftp> bye
```

After a few seconds the file should be picked up by the Connector. See [Error Handling](error-handling.md) for what to do if the document is rejected.
