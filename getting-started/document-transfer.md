# Document Transfer

The Colect XML API moves data between your ERP and the Colect platform as **XML documents**. The documents themselves are the same regardless of how they are transferred — what changes between integrations is the transport channel.

***

## Supported transports

| Transport                    | When to use                                                                       | Notes                                                       |
| ---------------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Colect Secure FTP (SFTP)** | Default for most ERP integrations. Good fit for batch exports, scheduled imports. | Credentials issued by Colect Support. Encrypted in transit. |
| **Other file-transfer**      | Custom integrations (S3 drop, internal file share, etc.) handled per project.     | Discussed during integration kick-off.                      |

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

<table><thead><tr><th>Document</th><th width="260.859375">Recommended pattern</th><th width="181.59375">Root element</th><th>Direction</th></tr></thead><tbody><tr><td>Products</td><td><code>products*.xml</code></td><td><code>&#x3C;products></code></td><td>ERP → Colect</td></tr><tr><td>Customers</td><td><code>customers*.xml</code></td><td><code>&#x3C;customers></code></td><td>ERP → Colect</td></tr><tr><td>Customer Access</td><td><code>customerAccess*.xml</code></td><td><code>&#x3C;customerAccesses></code></td><td>ERP → Colect</td></tr><tr><td>Product Access</td><td><code>productAccess*.xml</code></td><td><code>&#x3C;productAccesses></code></td><td>ERP → Colect</td></tr><tr><td>Product Relations <em>(new in v1.5)</em></td><td><code>productRelations*.xml</code></td><td><code>&#x3C;productRelations></code></td><td>ERP → Colect</td></tr><tr><td>Prices (sub-feed)</td><td><code>prices*.xml</code></td><td><code>&#x3C;prices></code></td><td>ERP → Colect</td></tr><tr><td>Sizes &#x26; Stock (sub-feed)</td><td><code>sizes*.xml</code></td><td><code>&#x3C;sizes></code></td><td>ERP → Colect</td></tr><tr><td>Extra Fields (sub-feed)</td><td><code>extraFields*.xml</code></td><td><code>&#x3C;extraFields></code></td><td>ERP → Colect</td></tr><tr><td>Invoices</td><td><code>invoices*.xml</code></td><td><code>&#x3C;invoices></code></td><td>ERP → Colect</td></tr><tr><td>Historical Orders</td><td><code>historicalOrders*.xml</code></td><td><code>&#x3C;historicalOrders></code></td><td>ERP → Colect</td></tr><tr><td>Orders</td><td><code>[orderNumber]-[random].xml</code></td><td>—</td><td>Colect → ERP</td></tr></tbody></table>

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
Subfolders may be added during implementation for specific integration needs (e.g. staging vs. production paths). Confirm the exact layout with your Colect contact during onboarding.
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
