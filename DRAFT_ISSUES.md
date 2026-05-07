# Draft GitHub issues

This file is a working scratchpad of issues to file against `mb-colect/colect-xml-api-docs`. Each block below is a `gh issue create` command — paste it into Terminal (with `gh` authenticated) and it'll file the issue. Delete this file once everything's filed; it should not be a permanent fixture in the repo.

You can also use these as the body for issues filed via the GitHub web UI.

---

## How to file all of these at once

Make sure `gh` is installed and authenticated:

```bash
brew install gh   # if you don't have it
gh auth login
cd ~/dev/colect-xml-api-docs
```

Then paste each command below.

---

## Verification (educated guesses I made — please confirm or correct)

```bash
gh issue create \
  --title "Verify SFTP host across getting-started/document-transfer.md" \
  --label "documentation,verification" \
  --body "Currently the SFTP connection table lists \`sftp.colect.services\` as the host. This was a placeholder. Please confirm the actual production host (or hosts, if it differs by collection) and update the table in \`getting-started/document-transfer.md\`."
```

```bash
gh issue create \
  --title "Verify file naming patterns" \
  --label "documentation,verification" \
  --body "Each document type is currently documented with a placeholder file-name pattern (\`products*.xml\`, \`customers*.xml\`, etc.) in:

- \`getting-started/document-transfer.md\` (the master table)
- Each \`documents/*.md\` page's \"Document at a glance\" table

Please confirm the actual patterns the Cloud Connector recognizes and correct any that differ."
```

```bash
gh issue create \
  --title "Verify HTTP POST endpoint shape in document-transfer.md" \
  --label "documentation,verification" \
  --body "The HTTP POST tab in \`getting-started/document-transfer.md\` shows a placeholder endpoint: \`POST /xml/inbox/products\` against \`connector.colect.services\`. This was a guess. If HTTP POST is supported, replace with the real endpoint, real headers, and real response shape. If it's not supported, strip the section."
```

```bash
gh issue create \
  --title "Verify SFTP directory layout (inbox/outbox/archive/error)" \
  --label "documentation,verification" \
  --body "\`getting-started/document-transfer.md\` shows a directory layout with \`inbox/\`, \`inbox/archive/\`, \`inbox/error/\`, \`outbox/\`, \`outbox/archive/\`. This was a sensible default. Please confirm the actual layout customers see, including:

- Whether archive/error subfolders exist by default
- Whether \`outbox/\` is per-document-type or flat
- The \`.err\` sidecar convention for rejected files (if any)"
```

---

## Content gaps to fill (small)

```bash
gh issue create \
  --title "Write business-logic/labels.md" \
  --label "documentation,enhancement" \
  --body "SUMMARY.md links to \`business-logic/labels.md\` but the file doesn't exist. Source material: \`Business Logic XML.pdf\` (the Labels section starting around page 3). Follow the same format as \`business-logic/multi-promotions.md\`:

1. Description / what it does
2. Where the data comes from (which document and field)
3. Requirements
4. Optional configurable settings
5. Example XML
6. How it's reported back (if applicable)
7. Related links"
```

```bash
gh issue create \
  --title "Write business-logic/approvals.md" \
  --label "documentation,enhancement" \
  --body "SUMMARY.md links to \`business-logic/approvals.md\` but the file doesn't exist. Source material: \`Business Logic XML.pdf\` (the Approvals section toward the end of the PDF). Follow the same format as \`business-logic/multi-promotions.md\`."
```

```bash
gh issue create \
  --title "Write getting-started/first-document.md" \
  --label "documentation,enhancement" \
  --body "SUMMARY.md and README.md link to \`getting-started/first-document.md\`. The file doesn't exist yet. Walk a customer through:

1. Get SFTP credentials from Colect Support
2. Build a minimal Products XML (we have one ready in \`examples/products-minimal.xml\`)
3. Validate locally with \`xmllint\` against the XSD
4. Upload to \`inbox/\` via SFTP
5. Verify it landed in \`inbox/archive/\` (or troubleshoot via \`inbox/error/\`)
6. Pull the resulting Orders document from \`outbox/\` once a test order is placed

Aim for the 'I just got my credentials, what now?' moment — concrete commands, not just concepts."
```

```bash
gh issue create \
  --title "Rewrite getting-started/error-handling.md for XML/SFTP" \
  --label "documentation,enhancement" \
  --body "The current \`getting-started/error-handling.md\` was inherited from the SOAP repo and references SOAP envelopes / fault structures. Rewrite it for the XML API:

- How rejected files are surfaced (\`.err\` sidecar in \`inbox/error/\`?)
- Common validation errors (mandatory field missing, unknown customerNo, schema violation)
- Retry strategy
- When to file a support ticket vs. fix locally
- Reference to \`xmllint\` for pre-validation"
```

```bash
gh issue create \
  --title "Write changelog.md" \
  --label "documentation,enhancement" \
  --body "SUMMARY.md links to \`changelog.md\`. Adapt the existing \`Colect_XML_API_1.5.html\` changelog table into a markdown page in this repo. Add a header note explaining that the schema versioning (v3.x) and the document revision (v1.5) are separate axes — schema changes ripple through both XML and SOAP, document revisions are XML-side only.

Source: \`changelog.html\` (provided in the original spec drop)."
```

---

## Polish items

```bash
gh issue create \
  --title "Add 'view example' links from each documents/*.md to its examples/*.xml" \
  --label "documentation,polish" \
  --body "Each \`documents/*.md\` page should link to its corresponding \`examples/*.xml\` file at the top, replacing the placeholder external links to \`assets.apptitude.nl/Documentation/sample-xml/...\` (which currently 404).

Files to update:

- documents/products.md → examples/products.xml
- documents/customers.md → examples/customers.xml
- documents/customer-access.md → examples/customer-access.xml
- documents/product-access.md → examples/product-access.xml
- documents/product-relations.md → examples/product-relations-matching-set.xml + product-relations-successor.xml
- documents/prices.md → examples/prices.xml
- documents/sizes-stock.md → examples/sizes-stock.xml
- documents/extra-fields.md → examples/extra-fields.xml
- documents/invoices.md → examples/invoices.xml
- documents/historical-orders.md → examples/historical-orders.xml
- documents/order-rules.md → examples/order-rules.xml
- documents/orders.md → examples/orders-output.xml"
```

```bash
gh issue create \
  --title "Adapt or replace data-types/* pages for XML" \
  --label "documentation,enhancement" \
  --body "The \`data-types/\` folder was inherited from the SOAP repo and uses SOAP-specific framing (\`XProduct_3_0\`, etc.). Decide between:

A) **Adapt** — keep the type taxonomy, rewrite each page in XML-doc framing rather than SOAP-RPC framing
B) **Replace** — collapse the section into one cross-reference page (since the field tables already live on each \`documents/*.md\` page)
C) **Delete** — the field tables on each document page may be sufficient

Recommendation: probably (B) — one consolidated reference page that maps \`XProduct_3_0\` etc. to their XML element shapes, since the field-level details already live on each document page."
```

```bash
gh issue create \
  --title "Snapshot v1.5 XSDs once published with v3.4 schema changes" \
  --label "schema,maintenance" \
  --body "The current snapshot at \`schema/snapshots/2026-05-06/\` was taken from the v1.4 hosted XSDs at \`assets.apptitude.nl\` plus the new \`ProductRelations_Feed_XSD.xsd\`. Once the v1.5/v3.4 versions are published, take a fresh snapshot following the procedure in \`schema/README.md\`:

\`\`\`bash
DATE=\$(date +%Y-%m-%d)
SNAP=schema/snapshots/\$DATE
BASE=https://assets.apptitude.nl/Documentation
mkdir -p \"\$SNAP\"
for f in Product_Feed_XSD.xsd Customer_Feed_XSD.xsd ...; do
  curl -fsSL -o \"\$SNAP/\$f\" \"\$BASE/\$f\"
done
\`\`\`

Then \`diff -r\` against the previous snapshot to spot the actual changes."
```

```bash
gh issue create \
  --title "Validate all examples/*.xml against the XSDs in schema/snapshots/" \
  --label "examples,quality" \
  --body "The example files in \`examples/\` were hand-written based on the field tables — they should validate against the corresponding XSDs but haven't been mechanically checked yet. Run:

\`\`\`bash
for f in examples/*.xml; do
  echo \"--- \$f ---\"
  xsd=\$(...)  # mapping per file
  xmllint --noout --schema \"\$xsd\" \"\$f\"
done
\`\`\`

Fix any examples that don't validate. Add the validation script (or a Make target) so future contributions are checked automatically."
```

---

## When all are filed, delete this file

```bash
git rm DRAFT_ISSUES.md
git commit -m "Remove DRAFT_ISSUES.md — issues now tracked in GitHub"
git push
```
