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

<table><thead><tr><th width="220">Property</th><th>Value</th></tr></thead><tbody><tr><td><strong>Host</strong></td><td><code>sftp.colect.services</code></td></tr><tr><td><strong>Port</strong></td><td><code>22</code></td></tr><tr><td><strong>Protocol</strong></td><td>SFTP (SSH File Transfer Protocol)</td></tr><tr><td><strong>Authentication</strong></td><td>Username + password, or SSH key (recommended)</td></tr><tr><td><strong>Encryption</strong></td><td>SSH (TLS not applicable to SFTP)</td></tr></tbody></table>

{% hint style="warning" %}
**Credentials are collection-specific.** Each Colect collection has its own SFTP user. Treat the credentials as confidential and never expose them in client-side code or public repositories.
{% endhint %}

***

## File naming and routing

Each input document type maps to a distinct file pattern. The Cloud Connector picks up files based on these patterns and routes them to the correct ingestion pipeline.

| Document                                  | File name pattern                  | Direction        |
| ----------------------------------------- | ---------------------------------- | ---------------- |
| Products                                  | `products*.xml`                    | ERP → Colect     |
| Customers                                 | `customers*.xml`                   | ERP → Colect     |
| Customer Access                           | `customerAccess*.xml`              | ERP → Colect     |
| Product Access                            | `productAccess*.xml`               | ERP → Colect     |
| Product Relations _(new in v1.5)_         | `productRelations*.xml`            | ERP → Colect     |
| Prices (sub-feed)                         | `prices*.xml`                      | ERP → Colect     |
| Sizes & Stock (sub-feed)                  | `sizes*.xml`                       | ERP → Colect     |
| Extra Fields (sub-feed)                   | `extraFields*.xml`                 | ERP → Colect     |
| Invoices                                  | `invoices*.xml`                    | ERP → Colect     |
| Historical Orders                         | `historicalOrders*.xml`            | ERP → Colect     |
| Orders                                    | `orders*.xml`                      | Colect → ERP     |

{% hint style="info" %}
The `*` in the pattern can include a timestamp, batch ID, or any string that helps you keep files distinct (e.g. `products_2026-05-06T0900.xml`). Colect uses the document's root element to determine its type — the file name is for your own bookkeeping and de-duplication.
{% endhint %}

***

## Directory layout

A typical SFTP layout looks like this:

```
/                  ← root (your SFTP user lands here)
├── inbox/         ← drop files here for Colect to ingest
│   └── archive/   ← Colect moves successfully ingested files here
│   └── error/     ← Colect moves rejected files here with a .err sidecar
└── outbox/        ← Colect drops files here for your ERP to pick up
    └── archive/   ← move processed files here from your ERP
```

Your integration's specific paths may differ — confirm with your Colect Support contact during onboarding.

***

## Ingestion lifecycle

```
   ┌──────────────────┐         ┌──────────────────┐         ┌──────────────────┐
   │ ERP writes file  │  ────▶  │  inbox/ on SFTP  │  ────▶  │ Cloud Connector  │
   └──────────────────┘         └──────────────────┘         └────────┬─────────┘
                                                                       │
                                            ┌──────────────────────────┴─────────────────────────┐
                                            │                                                    │
                                            ▼                                                    ▼
                                  ┌──────────────────┐                                ┌──────────────────┐
                                  │  Validation OK   │                                │ Validation FAIL  │
                                  │ → ingest data    │                                │ → write .err log │
                                  │ → move to        │                                │ → move to        │
                                  │   inbox/archive/ │                                │   inbox/error/   │
                                  └──────────────────┘                                └──────────────────┘
```

***

## Output documents (Orders)

Orders flow the other direction — Colect drops `orders*.xml` into your `outbox/`. Your ERP polls the outbox on a schedule and processes the files.

A common cadence is every 5–15 minutes. Faster polling is supported but rarely necessary; orders placed in the Sales App or B2B Webstore appear in the outbox within seconds of being signed.

***

## Verifying connectivity

The fastest way to confirm your SFTP credentials work is to upload a minimal Products document:

```bash
sftp colect-user@sftp.colect.services
sftp> cd inbox
sftp> put products_smoketest.xml
sftp> bye
```

After a few seconds, check `inbox/archive/` (success) or `inbox/error/` (failure with a `.err` sidecar describing the validation issue). See [Error Handling](error-handling.md) for how to interpret rejection messages.
