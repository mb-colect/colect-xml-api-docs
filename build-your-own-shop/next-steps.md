# What's Next

If you've pushed all four documents in this section, you have a working shop: an 8-product catalog with real stock, a matching set, and two sales reps who each see only their assigned customers. From here:

***

## Fill in what this walkthrough skipped

The Atoll dataset intentionally left some things out to keep each step short. These are documented and ready whenever you want them:

| Feature                  | Where to add it                                                          |
| ------------------------- | ------------------------------------------------------------------------- |
| Product images            | [Media objects](../documents/products.md#media-objects) — most customers use backend settings instead (see the note on that page) |
| Extra fields (materials, care, etc.) | [Extra Fields](../documents/extra-fields.md)                   |
| Invoices                  | [Invoices](../documents/invoices.md)                                      |
| Historical orders         | [Historical Orders](../documents/historical-orders.md)                    |
| Order-level discount/delivery rules (per customer) | [Order Discount & Delivery Cost Rules](../documents/order-rules.md) |
| Product access restrictions | [Product Access](../documents/product-access.md)                        |

***

## Close the loop: receive an order

Everything so far has been ERP → Colect. Once a rep places an order in the Sales App or B2B Webstore, Colect writes it back the other way — one file per order, dropped into `orderfiles/` on your SFTP account. See [Orders (output)](../documents/orders.md) for the format, and [Document Transfer](../getting-started/document-transfer.md#output-documents-orders) for the polling setup.

***

## Reference material

- [Documents](../documents/products.md) — full field reference for every document type, one comprehensive example each
- [XSD Reference](../xsd/overview.md) — schema fragments and live schema links, useful for validating a file before you push it
- [Error Handling](../getting-started/error-handling.md) — what a rejected file looks like on the Connector Status page and how to read the message
