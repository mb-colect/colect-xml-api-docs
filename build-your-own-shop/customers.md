# Customers & Access

Last piece: the people who can actually log in and place orders. This step pushes two documents — Customers (who they are) and Customer Access (which sales rep can sell to which customer) — since Customer Access references customers that must already exist.

***

## The customers at a glance

| customerNo | Name                     | Country | Currency | Sales rep                |
| ---------- | ------------------------ | ------- | -------- | ------------------------- |
| `ATL-C001` | Surf & Sun Amsterdam     | NL      | EUR      | sophie.martin (EU)        |
| `ATL-C002` | La Plage Knokke          | BE      | EUR      | sophie.martin (EU)        |
| `ATL-C003` | Côte Bleue Nice          | FR      | EUR      | sophie.martin (EU)        |
| `ATL-C004` | Marina Boutique Hamburg  | DE      | EUR      | sophie.martin (EU)        |
| `ATL-C005` | Sealine London           | GB      | GBP      | james.walker (UK)         |
| `ATL-C006` | Pacific Swim Miami       | US      | USD      | james.walker (US)         |

***

## Step — Write the documents

Create `atoll_customers.xml` — 6 boutique retailers across 5 countries, each with a buyer + shop manager contact and a shipping location. Côte Bleue Nice has two shipping locations (its Nice store and a Saint-Tropez location).

{% hint style="success" %}
Every Atoll product carries `discountGroupCode>SWIM` and `marginGroupCode>STD` (see [The Catalog](catalog.md)), but those codes were never defined until now. `ATL-C001` (Surf & Sun Amsterdam) gets a two-tier volume discount on `SWIM` — 10% at 6+ units, 15% at 12+ — plus a `STD` margin override, matching the [Discount Groups](../documents/customers.md#discount-groups) and [Margin Groups](../documents/customers.md#margin-groups) reference. This also live-tested `minQuantity` — see the confirmed results in the verify step below.
{% endhint %}

<details>

<summary>Show full atoll_customers.xml (6 customers)</summary>

{% code title="atoll_customers.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<customers xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <!-- ============================================================
       Atoll SS26 — Customer demo file
       6 boutique retailers across EU / UK / US
       ============================================================ -->

  <!-- NL — Amsterdam surf boutique, EU sizing, EUR -->
  <customer>
    <customerNo>ATL-C001</customerNo>
    <name>Surf &amp; Sun Amsterdam</name>
    <contacts>
      <contact>
        <code>ATL-C001-BUYER</code>
        <firstName>Emma</firstName>
        <lastName>de Vries</lastName>
        <role>Buyer</role>
        <phone>+31206123456</phone>
        <email>emma.devries@surfandsun.nl</email>
      </contact>
      <contact>
        <code>ATL-C001-MGR</code>
        <firstName>Lars</firstName>
        <lastName>Bakker</lastName>
        <role>Shopmanager</role>
        <phone>+31206123400</phone>
        <email>lars.bakker@surfandsun.nl</email>
      </contact>
    </contacts>
    <email>emma.devries@surfandsun.nl</email>
    <password/>
    <searchName/>
    <address>Kalverstraat 58</address>
    <postalCode>1012 NX</postalCode>
    <primaryUserEmailAddresses/>
    <city>Amsterdam</city>
    <country>NL</country>
    <phone>+31206123456</phone>
    <language>nl</language>
    <channel>L3</channel>
    <activePriceGroup>1</activePriceGroup>
    <currencyCode>EUR</currencyCode>
    <status>ACTIVE</status>
    <sizeNamingCode>EU</sizeNamingCode>
    <discountGroups>
      <discountGroup amount="10" code="SWIM" minQuantity="6" identifier="swim.6"/>
      <discountGroup amount="15" code="SWIM" minQuantity="12" identifier="swim.12"/>
    </discountGroups>
    <marginGroups>
      <marginGroup amount="2.3" code="STD"/>
    </marginGroups>
    <shippingLocations>
      <shippingLocation>
        <code>ATL-C001-SHP</code>
        <name>Surf &amp; Sun Amsterdam</name>
        <address>Kalverstraat 58</address>
        <postalCode>1012 NX</postalCode>
        <city>Amsterdam</city>
        <country>NL NETHERLANDS</country>
        <email>logistics@surfandsun.nl</email>
      </shippingLocation>
    </shippingLocations>
  </customer>

  <!-- BE — Coastal boutique Knokke, FR sizing, EUR -->
  <customer>
    <customerNo>ATL-C002</customerNo>
    <name>La Plage Knokke</name>
    <contacts>
      <contact>
        <code>ATL-C002-BUYER</code>
        <firstName>Nathalie</firstName>
        <lastName>Dubois</lastName>
        <role>Buyer</role>
        <phone>+3250612345</phone>
        <email>nathalie.dubois@laplage.be</email>
      </contact>
      <contact>
        <code>ATL-C002-MGR</code>
        <firstName>Pierre</firstName>
        <lastName>Laurent</lastName>
        <role>Shopmanager</role>
        <phone>+3250612300</phone>
        <email>pierre.laurent@laplage.be</email>
      </contact>
    </contacts>
    <email>nathalie.dubois@laplage.be</email>
    <password/>
    <searchName/>
    <address>Zeedijk 42</address>
    <postalCode>8300</postalCode>
    <primaryUserEmailAddresses/>
    <city>Knokke-Heist</city>
    <country>BE</country>
    <phone>+3250612345</phone>
    <language>fr</language>
    <channel>L2</channel>
    <activePriceGroup>1</activePriceGroup>
    <currencyCode>EUR</currencyCode>
    <status>ACTIVE</status>
    <sizeNamingCode>FR</sizeNamingCode>
    <shippingLocations>
      <shippingLocation>
        <code>ATL-C002-SHP</code>
        <name>La Plage Knokke</name>
        <address>Zeedijk 42</address>
        <postalCode>8300</postalCode>
        <city>Knokke-Heist</city>
        <country>BE BELGIUM</country>
        <email>logistics@laplage.be</email>
      </shippingLocation>
    </shippingLocations>
  </customer>

  <!-- FR — Riviera boutique Nice, FR sizing, EUR, two shipping locations -->
  <customer>
    <customerNo>ATL-C003</customerNo>
    <name>Côte Bleue Nice</name>
    <contacts>
      <contact>
        <code>ATL-C003-BUYER</code>
        <firstName>Isabelle</firstName>
        <lastName>Moreau</lastName>
        <role>Buyer</role>
        <phone>+33493881234</phone>
        <email>isabelle.moreau@cotebleue.fr</email>
      </contact>
      <contact>
        <code>ATL-C003-MGR</code>
        <firstName>Julien</firstName>
        <lastName>Bernard</lastName>
        <role>Shopmanager</role>
        <phone>+33493881200</phone>
        <email>julien.bernard@cotebleue.fr</email>
      </contact>
    </contacts>
    <email>isabelle.moreau@cotebleue.fr</email>
    <password/>
    <searchName/>
    <address>5 Promenade des Anglais</address>
    <postalCode>06000</postalCode>
    <primaryUserEmailAddresses/>
    <city>Nice</city>
    <country>FR</country>
    <phone>+33493881234</phone>
    <language>fr</language>
    <channel>L4</channel>
    <activePriceGroup>2</activePriceGroup>
    <currencyCode>EUR</currencyCode>
    <status>ACTIVE</status>
    <sizeNamingCode>FR</sizeNamingCode>
    <shippingLocations>
      <shippingLocation>
        <code>ATL-C003-SHP1</code>
        <name>Côte Bleue — Promenade</name>
        <address>5 Promenade des Anglais</address>
        <postalCode>06000</postalCode>
        <city>Nice</city>
        <country>FR FRANCE</country>
        <email>logistique@cotebleue.fr</email>
      </shippingLocation>
      <shippingLocation>
        <code>ATL-C003-SHP2</code>
        <name>Côte Bleue — Saint-Tropez</name>
        <address>12 Rue Gambetta</address>
        <postalCode>83990</postalCode>
        <city>Saint-Tropez</city>
        <country>FR FRANCE</country>
        <email>logistique@cotebleue.fr</email>
      </shippingLocation>
    </shippingLocations>
  </customer>

  <!-- DE — Hamburg marina boutique, EU sizing, EUR -->
  <customer>
    <customerNo>ATL-C004</customerNo>
    <name>Marina Boutique Hamburg</name>
    <contacts>
      <contact>
        <code>ATL-C004-BUYER</code>
        <firstName>Katharina</firstName>
        <lastName>Weber</lastName>
        <role>Buyer</role>
        <phone>+4940301234</phone>
        <email>katharina.weber@marina-boutique.de</email>
      </contact>
      <contact>
        <code>ATL-C004-MGR</code>
        <firstName>Stefan</firstName>
        <lastName>Hoffmann</lastName>
        <role>Shopmanager</role>
        <phone>+4940301200</phone>
        <email>stefan.hoffmann@marina-boutique.de</email>
      </contact>
    </contacts>
    <email>katharina.weber@marina-boutique.de</email>
    <password/>
    <searchName/>
    <address>Jungfernstieg 14</address>
    <postalCode>20354</postalCode>
    <primaryUserEmailAddresses/>
    <city>Hamburg</city>
    <country>DE</country>
    <phone>+4940301234</phone>
    <language>de</language>
    <channel>L3</channel>
    <activePriceGroup>1</activePriceGroup>
    <currencyCode>EUR</currencyCode>
    <status>ACTIVE</status>
    <sizeNamingCode>EU</sizeNamingCode>
    <shippingLocations>
      <shippingLocation>
        <code>ATL-C004-SHP</code>
        <name>Marina Boutique Hamburg</name>
        <address>Jungfernstieg 14</address>
        <postalCode>20354</postalCode>
        <city>Hamburg</city>
        <country>DE GERMANY</country>
        <email>logistics@marina-boutique.de</email>
      </shippingLocation>
    </shippingLocations>
  </customer>

  <!-- GB — London swimwear boutique, UK sizing, GBP -->
  <customer>
    <customerNo>ATL-C005</customerNo>
    <name>Sealine London</name>
    <contacts>
      <contact>
        <code>ATL-C005-BUYER</code>
        <firstName>Charlotte</firstName>
        <lastName>Hughes</lastName>
        <role>Buyer</role>
        <phone>+442071234567</phone>
        <email>charlotte.hughes@sealine.co.uk</email>
      </contact>
      <contact>
        <code>ATL-C005-MGR</code>
        <firstName>Oliver</firstName>
        <lastName>Bennett</lastName>
        <role>Shopmanager</role>
        <phone>+442071234500</phone>
        <email>oliver.bennett@sealine.co.uk</email>
      </contact>
    </contacts>
    <email>charlotte.hughes@sealine.co.uk</email>
    <password/>
    <searchName/>
    <address>34 King's Road</address>
    <postalCode>SW3 4UD</postalCode>
    <primaryUserEmailAddresses/>
    <city>London</city>
    <country>GB</country>
    <phone>+442071234567</phone>
    <language>en</language>
    <channel>L5</channel>
    <activePriceGroup>5</activePriceGroup>
    <currencyCode>GBP</currencyCode>
    <status>ACTIVE</status>
    <sizeNamingCode>UK</sizeNamingCode>
    <shippingLocations>
      <shippingLocation>
        <code>ATL-C005-SHP</code>
        <name>Sealine London</name>
        <address>34 King's Road</address>
        <postalCode>SW3 4UD</postalCode>
        <city>London</city>
        <country>GB UNITED KINGDOM</country>
        <email>logistics@sealine.co.uk</email>
      </shippingLocation>
    </shippingLocations>
  </customer>

  <!-- US — Miami resort boutique, US sizing, USD -->
  <customer>
    <customerNo>ATL-C006</customerNo>
    <name>Pacific Swim Miami</name>
    <contacts>
      <contact>
        <code>ATL-C006-BUYER</code>
        <firstName>Gabriela</firstName>
        <lastName>Torres</lastName>
        <role>Buyer</role>
        <phone>+13052345678</phone>
        <email>gabriela.torres@pacificswim.com</email>
      </contact>
      <contact>
        <code>ATL-C006-MGR</code>
        <firstName>Marcus</firstName>
        <lastName>Reid</lastName>
        <role>Shopmanager</role>
        <phone>+13052345600</phone>
        <email>marcus.reid@pacificswim.com</email>
      </contact>
    </contacts>
    <email>gabriela.torres@pacificswim.com</email>
    <password/>
    <searchName/>
    <address>1111 Lincoln Road</address>
    <postalCode>FL 33139</postalCode>
    <primaryUserEmailAddresses/>
    <city>Miami Beach</city>
    <country>US</country>
    <phone>+13052345678</phone>
    <language>en</language>
    <channel>L1</channel>
    <activePriceGroup>10</activePriceGroup>
    <currencyCode>USD</currencyCode>
    <status>ACTIVE</status>
    <sizeNamingCode>US</sizeNamingCode>
    <shippingLocations>
      <shippingLocation>
        <code>ATL-C006-SHP</code>
        <name>Pacific Swim Miami</name>
        <address>1111 Lincoln Road</address>
        <postalCode>FL 33139</postalCode>
        <city>Miami Beach</city>
        <country>US UNITED STATES</country>
        <email>logistics@pacificswim.com</email>
      </shippingLocation>
    </shippingLocations>
  </customer>

</customers>
```
{% endcode %}

</details>

Then create `atoll_customer_access.xml`, mapping two sales reps to their territories. `sophie.martin` covers the four EU customers; `james.walker` covers the UK customer under GBP and the US customer under USD — the same rep, two `<access>` entries, because each entry carries a single `defaultCurrency`.

{% code title="atoll_customer_access.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<customerAccess xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <!-- ============================================================
       Atoll SS26 — Customer Access demo file
       sophie.martin: EU territory (NL, BE, FR, DE) — EUR
       james.walker:  UK territory (GB) — GBP
                      US territory (US) — USD
       ============================================================ -->

  <!-- Sophie Martin — EU customers (EUR) -->
  <access>
    <email>sophie.martin@atoll-brand.com</email>
    <defaultPriceGroup/>
    <defaultCurrency>EUR</defaultCurrency>
    <customers>
      <customer><customerNo>ATL-C001</customerNo></customer>
      <customer><customerNo>ATL-C002</customerNo></customer>
      <customer><customerNo>ATL-C003</customerNo></customer>
      <customer><customerNo>ATL-C004</customerNo></customer>
    </customers>
  </access>

  <!-- James Walker — UK customers (GBP) -->
  <access>
    <email>james.walker@atoll-brand.com</email>
    <defaultPriceGroup/>
    <defaultCurrency>GBP</defaultCurrency>
    <customers>
      <customer><customerNo>ATL-C005</customerNo></customer>
    </customers>
  </access>

  <!-- James Walker — US customers (USD) -->
  <access>
    <email>james.walker@atoll-brand.com</email>
    <defaultPriceGroup/>
    <defaultCurrency>USD</defaultCurrency>
    <customers>
      <customer><customerNo>ATL-C006</customerNo></customer>
    </customers>
  </access>

</customerAccess>
```
{% endcode %}

***

## Step — Push it

```bash
sftp your-username@sftp.colect.services
sftp> cd datafiles
sftp> put atoll_customers.xml
sftp> put atoll_customer_access.xml
sftp> bye
```

***

## Step — Verify

Log into the Sales App or B2B Webstore as `sophie.martin@atoll-brand.com` — you should see the four EU customers (Amsterdam, Knokke, Nice, Hamburg) and be able to place an order in EUR. Logging in as `james.walker@atoll-brand.com` should show London (GBP) and Miami (USD) instead.

<!-- screenshot: customer picker filtered to a rep's assigned territory -->

Add Amsterdam (`ATL-C001`) to an order and put any `SWIM` product's quantity at 6 or more — the 10% discount should apply automatically. Below 6, no discount. At 12+, it should step up to 15%.

{% hint style="success" %}
**Confirmed live 2026-07-06** — this resolved [issue #42](https://github.com/mb-colect/colect-xml-api-docs/issues/42): `minQuantity` is the correct attribute name and applies exactly at the threshold.
{% endhint %}

| Quantity | Promotion | Unit price (WSP) |
| -------- | --------- | ------------------ |
| 5        | none      | EUR 39.11           |
| 6        | 10%       | EUR 35.20           |
| 12       | 15%       | EUR 33.24           |

![](<../.gitbook/assets/atoll-discount-5-units.png>)

![](<../.gitbook/assets/atoll-discount-6-units.png>)

![](<../.gitbook/assets/atoll-discount-12-units.png>)

***

That's the full shop: catalog, stock, matching sets, and customers who can log in and order. See [What's Next](next-steps.md) for where to go from here.
