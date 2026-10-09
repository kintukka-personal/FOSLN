# Structured data for FOSLN product pages

Research for [#12](https://github.com/Kintukka2/FOSLN-WATCH/issues/12). Sources read on 2026-10-09.

## Answer

- **Each Model page needs one `ProductGroup`.** It lists one `Product` per Dial in `hasVariant`. Each Dial gets its own `Offer`, with a USD price, availability and a `?variant=` URL, plus its own SKU, `color` and `itemCondition`. The brand is FOSLN.
- **Standard delivery and return terms go in one place:** `OnlineStore` markup, via `hasShippingService` and `hasMerchantReturnPolicy`. Google ranks Merchant Center and Search Console settings above any markup.
- **Shopify's Dawn and Horizon themes give the group, variant and offer skeleton for free.** It comes from `{{ product | structured_data }}`. They leave out `variesBy`, `color`, `itemCondition`, strikethrough prices, delivery and return policies, and reviews.
- **Merchant Center files watches under Apparel & Accessories.** So each Dial also needs `color`, `gender`, `age_group`, `item_group_id`, a brand, and a GTIN or MPN. US listings also need a delivery cost.

---

## 1. What Google reads, and which source wins

**Merchant listings are the type that applies.** Google has two Product result types. *Product snippets* are "for product pages where people can't directly purchase the product". *Merchant listings* are "for pages where customers can purchase products from you" ([Google: Product structured data](https://developers.google.com/search/docs/appearance/structured-data/product)). FOSLN's PDPs are merchant-listing pages, and the same markup also makes them eligible for product snippets (same source).

**Markup and a feed work best together.** Google accepts page markup, a Merchant Center feed, or both. It says providing both "maximizes your eligibility to experiences and helps Google correctly understand and verify your data" ([same source](https://developers.google.com/search/docs/appearance/structured-data/product)).

**Required for a merchant listing:**
- On `Product`: `name`, `image` and `offers`.
- On `Offer`: `price` and `priceCurrency`. The price must be greater than zero, and it must be an `Offer`, not an `AggregateOffer` ([Google: Merchant listing](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)).

**Recommended for a merchant listing** ([same source](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)):
- On `Product`: `brand.name`, `color`, `description`, `gtin`, `mpn`, `sku`, `material`, `audience`, `isVariantOf`, `inProductGroupWithID`, `aggregateRating` and `review`.
- On `Offer`: `availability`, `itemCondition`, `shippingDetails`, `hasMerchantReturnPolicy`, `priceValidUntil` and `url`.
- Images: Google recommends several high-resolution images in 16x9, 4x3 and 1x1 ratios.

**Precedence, strongest first.** Google's merchant listing page gives this order ([Google: Merchant listing](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)):
1. Product-level feeds submitted in Merchant Center
2. Content API for Shopping settings
3. Settings in Merchant Center or Search Console
4. Product-level merchant listing markup
5. Organization-level markup

The shipping-policy and return-policy pages give the same order without the first item. They say plainly that when markup and Search Console settings both exist, "Google will only use the information provided in Search Console" ([shipping policy](https://developers.google.com/search/docs/appearance/structured-data/shipping-policy), [return policy](https://developers.google.com/search/docs/appearance/structured-data/return-policy)).

So markup is the weakest source for delivery and returns. It still matters for two reasons:
- It is the only source if no Merchant Center or Search Console settings exist.
- Google checks that sources agree. A listing "might be rejected" if Google finds a lower delivery cost on the listing than on the website ([Search Console Help: Shipping and return settings](https://support.google.com/webmasters/answer/14907594?hl=en)).

**Markup should be in the initial HTML.** Google warns that "dynamically-generated markup can make Shopping crawls less frequent and less reliable" ([Google: Merchant listing](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)). Shopify renders its JSON-LD server-side in Liquid. The demo pages in section 3 returned it in the raw HTML, with no JavaScript run.

**Markup can feed Merchant Center directly.** Merchant Center can build products automatically from a site's structured data: "We read the structured data on those pages to understand product details like title, price, and availability" ([Merchant Center Help: Add products automatically](https://support.google.com/merchants/answer/12158480)). Products already in another data source, such as a Shopify app feed, are not added twice (same source).

## 2. The target shape for a FOSLN Model page

### How FOSLN's terms map to schema.org

| FOSLN term | schema.org | Notes |
|---|---|---|
| Model (ACIER, CADRE) | `ProductGroup` | A `ProductGroup` is "a group of Products that vary only in certain well-described ways" ([schema.org/ProductGroup](https://schema.org/ProductGroup)). It needs a `name`. Google recommends `productGroupID`, `variesBy`, `hasVariant`, `brand`, `description`, and `url` (single-page sites only) ([Google: Product variants](https://developers.google.com/search/docs/appearance/structured-data/product-variants)). |
| Dial (Chrome, Onyx, black, champagne) | `Product` inside `hasVariant` | "Each variant must have a unique ID", given as `sku` or `gtin`. Variant names should be more specific than the group name ([Google: Product variants](https://developers.google.com/search/docs/appearance/structured-data/product-variants)). |
| What the Dials differ by | `variesBy: https://schema.org/color` | Google supports only six values: `color`, `size`, `suggestedAge`, `suggestedGender`, `material` and `pattern` ([same source](https://developers.google.com/search/docs/appearance/structured-data/product-variants)). Of those, `color` fits a Dial best. |
| Price in the US Market | `Offer.price` + `priceCurrency: "USD"` | One `Offer` per Dial. Google's examples nest an `offers` object inside each variant ([same source](https://developers.google.com/search/docs/appearance/structured-data/product-variants)). |
| FOSLN | `brand.name` and `OnlineStore` | Google recommends the `OnlineStore` subtype for an ecommerce site ([Google: Organization](https://developers.google.com/search/docs/appearance/structured-data/organization)). |
| Dispatch | `handlingTime` | schema.org defines handling time as "the typical delay between the receipt of the order and the goods either leaving the warehouse or being prepared for pickup" ([schema.org/ShippingDeliveryTime](https://schema.org/ShippingDeliveryTime)). |
| The carrier leg after Dispatch | `transitTime` | "The typical delay the order has been sent for delivery and the goods reach the final customer" (same source). Dispatch is not delivery: Google's delivery time is handling time plus transit time. |

**Glossary note.** schema.org and Merchant Center both call the field `color`, and a Dial's name is the natural value for it. That is a field mapping. It is no reason to start calling Dials "colours".

### Single-page variants

Google describes two ways to build variant pages ([Google: Product variants](https://developers.google.com/search/docs/appearance/structured-data/product-variants)):

- **Single-page.** One page per group. Here "there must be only one distinct canonical URL for the overall `ProductGroup`". The site "must have the ability to preselect each variant directly with a distinct URL", and the preselected page must show that variant's image, price and availability.
- **Multi-page.** Each variant has its own page, and every page repeats the full group markup.

Shopify's default is the single-page pattern. On the Dawn demo store, `/products/studio-denim?variant=…` declares `/products/studio-denim` as its canonical URL. The filter gives each variant its own `?variant=` offer URL (observed 2026-10-09; see section 3).

### Illustrative skeleton (evidence, not a deliverable)

Values in angle brackets are placeholders. Prices, delivery terms and return terms are not decided here.

```json
{
  "@context": "https://schema.org",
  "@type": "ProductGroup",
  "@id": "https://fosln.com/products/acier#product",
  "name": "ACIER",
  "url": "https://fosln.com/products/acier",
  "description": "<PDP description>",
  "brand": { "@type": "Brand", "name": "FOSLN" },
  "productGroupID": "<parent ID>",
  "variesBy": ["https://schema.org/color"],
  "hasVariant": [
    {
      "@type": "Product",
      "name": "ACIER - Chrome",
      "sku": "<Chrome SKU>",
      "mpn": "<MPN, if there is no GTIN>",
      "color": "Chrome",
      "image": ["<1x1 URL>", "<4x3 URL>", "<16x9 URL>"],
      "offers": {
        "@type": "Offer",
        "url": "https://fosln.com/products/acier?variant=<id>",
        "price": "<price>",
        "priceCurrency": "USD",
        "availability": "https://schema.org/InStock",
        "itemCondition": "https://schema.org/NewCondition"
      }
    }
  ]
}
```

The Onyx variant repeats the `Product` block. CADRE follows the same pattern once its Dial names are chosen.

The site-wide policies go once, in the organization markup:

```json
{
  "@context": "https://schema.org",
  "@type": "OnlineStore",
  "name": "FOSLN",
  "url": "https://fosln.com",
  "hasShippingService": {
    "@type": "ShippingService",
    "handlingTime": {
      "@type": "ServicePeriod",
      "duration": { "@type": "QuantitativeValue", "minValue": 0, "maxValue": "<days>", "unitCode": "DAY" }
    },
    "shippingConditions": {
      "@type": "ShippingConditions",
      "shippingDestination": { "@type": "DefinedRegion", "addressCountry": "US" },
      "shippingRate": { "@type": "MonetaryAmount", "value": "<cost>", "currency": "USD" },
      "transitTime": {
        "@type": "ServicePeriod",
        "duration": { "@type": "QuantitativeValue", "minValue": "<min>", "maxValue": "<max>", "unitCode": "DAY" }
      }
    }
  },
  "hasMerchantReturnPolicy": {
    "@type": "MerchantReturnPolicy",
    "applicableCountry": "US",
    "returnPolicyCategory": "https://schema.org/MerchantReturnFiniteReturnWindow",
    "merchantReturnDays": "<days>",
    "returnMethod": "https://schema.org/ReturnByMail",
    "returnFees": "<FreeReturn | ReturnFeesCustomerResponsibility | ReturnShippingFees>",
    "merchantReturnLink": "<shop.refund_policy.url>"
  }
}
```

The policy structures above follow Google's shipping and return pages:

- **Delivery.** `ShippingService` needs `shippingConditions`. Handling time and transit time are each a `ServicePeriod` with a `duration` in `DAY`s ([Google: Shipping policy](https://developers.google.com/search/docs/appearance/structured-data/shipping-policy)).
- **Returns, option A.** `MerchantReturnPolicy` needs `applicableCountry` and `returnPolicyCategory`, and `merchantReturnDays` when the window is finite ([Google: Return policy](https://developers.google.com/search/docs/appearance/structured-data/return-policy)).
- **Returns, option B.** A `merchantReturnLink` alone satisfies the requirement (same source).
- **The policy page URL.** Shopify's Liquid `shop` object exposes `refund_policy` and `shipping_policy` ([Shopify: shop object](https://shopify.dev/docs/api/liquid/objects/shop)).
- **Org-level by default.** Google recommends "a global return policy for your business under `Organization` markup". It reserves offer-level policies for products with their own terms ([Google: Merchant listing](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)). The same split applies to delivery ([Google: Shipping policy](https://developers.google.com/search/docs/appearance/structured-data/shipping-policy)).
- **The schema.org types exist.** `ShippingService` and the `hasShippingService` property on `Organization` are in schema.org V30.1 ([schema.org/ShippingService](https://schema.org/ShippingService)).
- **Placement.** Google suggests the home page or a single about page for Organization markup, and says "You don't need to include it on every page of your site" ([Google: Organization](https://developers.google.com/search/docs/appearance/structured-data/organization)).

## 3. What Shopify gives for free

### Theme source

| Theme | Product JSON-LD | Organization JSON-LD | Other |
|---|---|---|---|
| Dawn 16.0.0, `main` at `258f00f`, 2026-08-10 | `{{ product \| structured_data }}` in [`sections/main-product.liquid` L749–751](https://github.com/Shopify/dawn/blob/258f00f64365e2018ca4c62778a6bf55a5d3cd18/sections/main-product.liquid#L749-L751) and in `featured-product.liquid` | Hand-written in [`sections/header.liquid` L461–482](https://github.com/Shopify/dawn/blob/258f00f64365e2018ca4c62778a6bf55a5d3cd18/sections/header.liquid#L461-L482): `Organization` with `name`, `logo`, `sameAs` and `url`, on every page | `WebSite` with `SearchAction` on the home page only ([L484–499](https://github.com/Shopify/dawn/blob/258f00f64365e2018ca4c62778a6bf55a5d3cd18/sections/header.liquid#L484-L499)) |
| Horizon 4.2.0, `main` at `5acd1b6`, 2026-09-21 | `{{ closest.product \| structured_data }}` in [`sections/product-information.liquid` L1–3](https://github.com/Shopify/horizon/blob/5acd1b6b66c02f61d3216e3adace5dd9e0404fc9/sections/product-information.liquid#L1-L3) and in the featured-product sections | [`sections/header.liquid` L326–336](https://github.com/Shopify/horizon/blob/5acd1b6b66c02f61d3216e3adace5dd9e0404fc9/sections/header.liquid#L326-L336): `Organization` with `name`, `logo` and `url` only | None |

Neither theme contains `BreadcrumbList`, `shippingDetails`, `MerchantReturnPolicy`, `hasShippingService`, `OnlineStore`, `priceSpecification` or `variesBy` anywhere in its Liquid, JSON or JS (grep of both repos at the commits above).

### What the filter itself does

Shopify's docs say a product is output "as a schema.org `Product` if they have no variants" and "a `ProductGroup` if they have one or more variants". The filter takes no options, and the only example shown is a single-variant `Product` ([Shopify: structured_data filter](https://shopify.dev/docs/api/liquid/filters/structured_data)). The filter runs on Shopify's servers, so its output is set by the platform, not the theme version.

To see the real multi-variant output, I fetched two product pages from Shopify's own Dawn demo store on 2026-10-09. That store runs Dawn 15.0.0, according to its `Shopify.theme` object:
- `theme-dawn-demo.myshopify.com/products/studio-denim`, with one Color option
- `…/products/louise-slide-sandal`, with Color and Size options and a compare-at price of 430.00 over a price of 395.00

| Property | Google's status | Filter output |
|---|---|---|
| `ProductGroup` with `name`, `url`, `productGroupID` and `hasVariant` | required or recommended | **Yes.** `productGroupID` is the Shopify product ID. |
| `brand` | recommended | **Yes,** from the product's vendor field. The demo vendor "Mlouye" appears as the brand. |
| `description` | recommended | Yes. It is presumably an empty string when the product has none. |
| `category` | recommended | Emitted as an empty string `""` on both products. Which Shopify field fills it is not verified. |
| `variesBy` | recommended | **No,** even with Color and Size options. |
| Variant `name` | required | **Yes,** as "Product - Variant", for example "Studio Bag - Denim". |
| Variant `sku` | the unique ID Google requires (or a `gtin`) | **Only when a SKU is set.** The sandal's variants have blank SKUs and carry no `sku`. |
| Variant `gtin` | recommended | Appears on one variant. It presumably comes from the barcode field; the mapping is inferred. |
| Variant `image` | required | Yes, one image per variant. Google prefers several, in three ratios. |
| Variant `color`, `size` | recommended | **No.** |
| `mpn` | recommended | **No.** |
| Offer `price`, `priceCurrency`, `availability` and `url` | required or recommended | **Yes.** Each `url` preselects its variant. |
| Offer `itemCondition` | recommended | **No.** |
| Strikethrough `priceSpecification` | optional | **No.** The compare-at price of 430.00 does not appear. |
| `shippingDetails` and `hasMerchantReturnPolicy` | recommended | **No.** |
| `aggregateRating` and `review` | recommended | **No.** |

**Organization output.** Dawn's block gave `name` (the shop name), `logo` and `url`. It also gave a `sameAs` array with empty strings for the unset social links. That is harmless, but it breaks the "omit empty fields" rule in the `schema-markup` skill.

### The gaps for FOSLN, and the cheapest fix for each

1. **`variesBy`, `color` per Dial, `itemCondition`, `mpn`, and extra images.** The filter takes no options, so closing these means replacing `{{ product | structured_data }}` with hand-built Liquid JSON-LD in the product section. A second block that "adds to" the filter's output is the alternative. I did not verify whether Google merges two blocks that share an `@id`, so replacing the filter is the safer path.
2. **Delivery and return policies.** These are org-level, so they belong in the theme's existing `Organization` block in `header.liquid`. That block should also change its type to `OnlineStore` and add `contactPoint`. This is a small edit and leaves the product markup alone.
3. **Store data the filter reads.** These need no code:
   - The vendor must be "FOSLN", not the Supplier's name. Dropshipping import tools may fill the vendor field from the Supplier; I have not verified this.
   - The shop name must be "FOSLN".
   - Every Dial needs a unique SKU.
   - A product category should be set, so `category` isn't blank. That it fills `category` is not verified.

## 4. Delivery and returns: what the markup can and can't say

**What the markup can say.** It can express:
- the handling time: the Supplier's Dispatch window
- the transit time
- the delivery cost to the US, including free delivery (`value: 0`)
- the return window, method and fees
- the country returns go to (`returnPolicyCountry`)
- a link to the policy page

Sources: [Google: Shipping policy](https://developers.google.com/search/docs/appearance/structured-data/shipping-policy), [Google: Return policy](https://developers.google.com/search/docs/appearance/structured-data/return-policy). A delivery origin (`shippingOrigin`) is optional (shipping policy, same source).

**What the facts support today.** Every test order so far was dispatched within 24 hours, which supports a handling time of 0–1 days. Nothing yet supports a transit time, because Dispatch says nothing about delivery. The committed terms belong to [#25](https://github.com/Kintukka2/FOSLN-WATCH/issues/25) and [#26](https://github.com/Kintukka2/FOSLN-WATCH/issues/26). Until they're set, the markup should leave out `transitTime` rather than guess it.

**Where Google expects the terms:**
- **Delivery costs:** Merchant Center requires a delivery cost for US Shopping ads and free listings ([Merchant Center: Product data specification](https://support.google.com/merchants/answer/7052112?hl=en), `shipping` attribute).
- **Delivery settings from Shopify:** Shopify's Google & YouTube app can import delivery settings automatically, but "only shipping rates in your General shipping profile can sync to Google Merchant Center" ([Shopify Help: Google & YouTube requirements](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/requirements)).
- **Return policies:** These are optional in Merchant Center: "you may also choose to provide information about your return and refund policies" ([Merchant Center: Set up return policies](https://support.google.com/merchants/answer/14011730?hl=en)). Google tells Shopify users to set the policy in Shopify and then add it again in Merchant Center. It does not say the app syncs it ([Merchant Center: Return policies for Shopify users](https://support.google.com/merchants/answer/14232691?hl=en)).
- **Consistency:** Google asks merchants to "confirm that the return policies are consistent in Merchant Center and across your website" ([support.google.com/merchants/answer/14011730](https://support.google.com/merchants/answer/14011730?hl=en)).
- **Search Console:** Delivery time, delivery cost, return window and return cost can also be set in Search Console without a Merchant Center account ([Search Console Help](https://support.google.com/webmasters/answer/14907594?hl=en)).

## 5. Merchant Center requirements for watches (US Market)

**Watches count as apparel.** Google's taxonomy puts watches at `201 - Apparel & Accessories > Jewelry > Watches`, under `166 - Apparel & Accessories` ([Google product taxonomy, version 2021-09-21](https://www.google.com/basepages/producttype/taxonomy-with-ids.en-US.txt)). The apparel rules below therefore apply.

The `age_group` page lists the subcategories where the attribute is optional, and it names watch bands, watch winders and watch stickers. The `gender` page's list names watch bands. Neither list names watches ([age_group](https://support.google.com/merchants/answer/6324463?hl=en), [gender](https://support.google.com/merchants/answer/6324479?hl=en)). If no category is set, Google assigns one automatically ([google_product_category](https://support.google.com/merchants/answer/6324436?hl=en)).

| Attribute | Status for a FOSLN Dial | Source |
|---|---|---|
| `id`, `title`, `description`, `link`, `image_link`, `availability`, `price` | Required | [Product data specification](https://support.google.com/merchants/answer/7052112?hl=en) |
| `brand` | Required. Use your store name "if you manufacture the product" or "if your product is a private-label product". That makes the brand FOSLN. | [brand](https://support.google.com/merchants/answer/6324351?hl=en) |
| `gtin` | Submit it only if the watch has a GTIN from its manufacturer. Private-label products "generally won't have a GTIN". "Don't guess or make up a value." | [gtin](https://support.google.com/merchants/answer/6324461?hl=en) |
| `mpn` | Required "only if your product does not have a manufacturer assigned GTIN". The brand page says to include an MPN "with a unique identifier number of your choice". | [spec](https://support.google.com/merchants/answer/7052112?hl=en), [brand](https://support.google.com/merchants/answer/6324351?hl=en) |
| `identifier_exists` | Set it to `no` only for custom or one-of-a-kind goods. Google warns when identifiers do exist. | [identifier_exists](https://support.google.com/merchants/answer/6324478?hl=en) |
| `color` | Required for Apparel & Accessories (166) in US Shopping ads and in free listings, and for any product that varies by colour. Submit the name the landing page uses, such as "Chrome", and put a standard colour word in the title. Up to 3 colours, separated by `/`. | [color](https://support.google.com/merchants/answer/6324487?hl=en) |
| `gender` | Required for Apparel & Accessories in the US. The values are `male`, `female` or `unisex`. | [gender](https://support.google.com/merchants/answer/6324479?hl=en) |
| `age_group` | Required for Apparel & Accessories in the US. The value for a FOSLN watch would be `adult`. | [age_group](https://support.google.com/merchants/answer/6324463?hl=en) |
| `size` | Not required. It applies only to Clothing (1604) and Shoes (187), or to products sold in different sizes. | [spec](https://support.google.com/merchants/answer/7052112?hl=en) |
| `item_group_id` | Required in the US when the product is a variant. Each Dial carries its Model's group ID. | [spec](https://support.google.com/merchants/answer/7052112?hl=en) |
| `condition` | Optional for new products | [spec](https://support.google.com/merchants/answer/7052112?hl=en) |
| `shipping` | Delivery costs are required in the US | [spec](https://support.google.com/merchants/answer/7052112?hl=en) |
| `min_handling_time`, `max_handling_time`, transit time, `ships_from_country` | Optional | [spec](https://support.google.com/merchants/answer/7052112?hl=en) |
| `sale_price` | Optional. See section 6. | [spec](https://support.google.com/merchants/answer/7052112?hl=en) |

**Shopify store requirements.** Shopify's Google & YouTube app adds these on top ([Shopify Help](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/requirements)):
- a payment provider
- a refund policy and terms of service
- contact information on a page
- a verified domain not claimed by another account
- variant options in English
- a GTIN, or else a brand and MPN

## 6. Sale prices in markup

The reference designs show $97 with $225 or $249 struck through ([map #4](https://github.com/Kintukka2/FOSLN-WATCH/issues/4)). Whether and how FOSLN shows a sale price belongs to [#13](https://github.com/Kintukka2/FOSLN-WATCH/issues/13) and [#24](https://github.com/Kintukka2/FOSLN-WATCH/issues/24). These are the structured-data facts those tickets need:

- **The markup mechanism.** The old price goes in a `UnitPriceSpecification` inside the offer's `priceSpecification`, with `priceType: https://schema.org/StrikethroughPrice`. Per Google, "no other values are supported", though `ListPrice` is allowed for a transition period. The current price stays in the offer and carries no `priceType` ([Google: Merchant listing](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing)).
- **Shopify's default.** It emits no strikethrough price. A compare-at price is not output (section 3).
- **Merchant Center's sale annotation in the US:**
  - The discount must be "greater than 5% and less than 90%".
  - Both prices should show on the landing page.
  - For **US Shopping ads** only, the base price "must be charged" for 5 of the past 30 days or 15 of the past 200 days.
  - **US free listings** list no duration rule.
  - Google "regularly validates the sale price on your website".
  - Source: [Merchant Center: About sale price annotations](https://support.google.com/merchants/answer/9017019?hl=en).

  These rules point one way: a struck-through price that was never actually charged would not earn the annotation in US Shopping ads.

## 7. Reviews, FAQ and breadcrumbs

- **Reviews.** Add `aggregateRating` and `review` only when real reviews exist and are visible on the page. Google says "Don't include fake or undisclosed incentivized reviews" and "Don't aggregate reviews or ratings from other websites" ([Google: Review snippet](https://developers.google.com/search/docs/appearance/structured-data/review-snippet)). Social proof at launch is still open on the map.
- **FAQ.** `FAQPage` markup no longer produces anything: "The FAQ rich result feature is no longer shown in Google Search results". Google's FAQ documentation URL now redirects to that notice ([Google Search Central: documentation updates](https://developers.google.com/search/updates#removing-faq-rich-result)). A PDP FAQ is still fine as on-page content.
- **Breadcrumbs.** `BreadcrumbList` needs at least two `ListItem`s. Google says the feature "is available on desktop in all regions" and doesn't mention mobile ([Google: Breadcrumb](https://developers.google.com/search/docs/appearance/structured-data/breadcrumb)). Neither Dawn nor Horizon emits it. It is cheap to add but low value for a two-Model store.

## 8. Trade-offs for other tickets, and which way they point

| Question | Facts | Points to | Owner |
|---|---|---|---|
| One Shopify product per Model with Dials as variants, or one product per Dial? | With variants, the filter already outputs a correct `ProductGroup` on Google's single-page pattern: one canonical URL and `?variant=` preselection. With one product per Dial, the filter outputs unrelated `Product`s, and the multi-page group markup must be written by hand on every page. | Dials as variants | [#18](https://github.com/Kintukka2/FOSLN-WATCH/issues/18) and the product data model |
| Keep the filter, or replace it? | The filter is correct but thin and takes no options (section 3). | Replace it in the product section. Add policies to the `Organization` block. | Build effort, under [#21](https://github.com/Kintukka2/FOSLN-WATCH/issues/21) |
| Where do delivery and return terms live for Google? | Merchant Center and Search Console outrank markup. Shopify syncs only General-profile rates. Return policies must be added in Merchant Center by hand. | Merchant Center as the source, mirrored in `OnlineStore` markup and the policy pages | [#25](https://github.com/Kintukka2/FOSLN-WATCH/issues/25) |
| GTIN or MPN? | Use a GTIN only if the manufacturer assigned one. Otherwise brand FOSLN plus an MPN. Never invent one. | Ask the Supplier whether the watches carry GTINs | [#16](https://github.com/Kintukka2/FOSLN-WATCH/issues/16), [#26](https://github.com/Kintukka2/FOSLN-WATCH/issues/26) |
| Dial names | `color` should match the landing-page name. CADRE's Dial names aren't chosen yet. | Choose the names before the CADRE markup and feed are built | Glossary / [#16](https://github.com/Kintukka2/FOSLN-WATCH/issues/16) |
| Images | Google recommends several images per product in 1x1, 4x3 and 16x9. | Plan all three ratios for each Dial | [#17](https://github.com/Kintukka2/FOSLN-WATCH/issues/17) |
| A non-Shopify platform | The target shape in section 2 stays the same. Only the free baseline in section 3 changes. | Neutral | [#21](https://github.com/Kintukka2/FOSLN-WATCH/issues/21) |

## 9. Not verified

- **Which Shopify field fills the filter's `category`.** It was blank on both demo products, and I didn't observe a product with a Shopify category set.
- **Other store data.** I didn't verify whether category metafields, such as a colour metafield, change the filter's output.
- **Barcode to `gtin`.** That the barcode field becomes `gtin` is inferred from one demo variant. The demo's public `products.json` doesn't expose barcodes.
- **Merged blocks.** I didn't verify whether Google merges two JSON-LD blocks that share an `@id`.
- **Supplier GTINs.** I don't know whether the Supplier's watches carry GTINs.
- **Return-policy sync.** I don't know whether Shopify's Google & YouTube app syncs return policies. Google's Shopify page describes a manual step.
- **The MPN sentence.** On the brand page, I saw the "MPN … of your choice" sentence out of context, so its exact scope is unclear.
- **Default theme.** I didn't check which theme Shopify gives new stores by default in 2026. Both Dawn and Horizon behave the same for product JSON-LD.
- **The Content API.** Google's precedence lists still name the Content API for Shopping. I didn't check its current status, and it doesn't affect FOSLN.
- **Point-in-time observations.** The live filter output was observed once, on 2026-10-09. Shopify can change the filter's output without a theme release.

## Sources

- Google Search Central:
  - [Product structured data](https://developers.google.com/search/docs/appearance/structured-data/product), updated 2025-12-10
  - [Merchant listing](https://developers.google.com/search/docs/appearance/structured-data/merchant-listing), updated 2026-09-08
  - [Product variants](https://developers.google.com/search/docs/appearance/structured-data/product-variants), updated 2026-09-08
  - [Shipping policy](https://developers.google.com/search/docs/appearance/structured-data/shipping-policy), updated 2026-09-08
  - [Return policy](https://developers.google.com/search/docs/appearance/structured-data/return-policy), updated 2026-09-08
  - [Organization](https://developers.google.com/search/docs/appearance/structured-data/organization), updated 2026-09-08
  - [Review snippet](https://developers.google.com/search/docs/appearance/structured-data/review-snippet), updated 2026-09-08
  - [Breadcrumb](https://developers.google.com/search/docs/appearance/structured-data/breadcrumb), updated 2026-09-08
  - [Documentation updates: FAQ removal](https://developers.google.com/search/updates#removing-faq-rich-result)
- [Search Console Help: Shipping and return settings](https://support.google.com/webmasters/answer/14907594?hl=en)
- schema.org V30.1:
  - [ProductGroup](https://schema.org/ProductGroup)
  - [OfferShippingDetails](https://schema.org/OfferShippingDetails)
  - [ShippingService](https://schema.org/ShippingService)
  - [ShippingDeliveryTime](https://schema.org/ShippingDeliveryTime)
  - [MerchantReturnPolicy](https://schema.org/MerchantReturnPolicy)
- Merchant Center Help:
  - [Product data specification](https://support.google.com/merchants/answer/7052112?hl=en)
  - [brand](https://support.google.com/merchants/answer/6324351?hl=en)
  - [gtin](https://support.google.com/merchants/answer/6324461?hl=en)
  - [identifier_exists](https://support.google.com/merchants/answer/6324478?hl=en)
  - [color](https://support.google.com/merchants/answer/6324487?hl=en)
  - [gender](https://support.google.com/merchants/answer/6324479?hl=en)
  - [age_group](https://support.google.com/merchants/answer/6324463?hl=en)
  - [google_product_category](https://support.google.com/merchants/answer/6324436?hl=en)
  - [Return policies](https://support.google.com/merchants/answer/14011730?hl=en)
  - [Return policies for Shopify users](https://support.google.com/merchants/answer/14232691?hl=en)
  - [Sale price annotations](https://support.google.com/merchants/answer/9017019?hl=en)
  - [Add products automatically](https://support.google.com/merchants/answer/12158480)
  - [Google product taxonomy](https://www.google.com/basepages/producttype/taxonomy-with-ids.en-US.txt)
- Shopify:
  - [structured_data filter](https://shopify.dev/docs/api/liquid/filters/structured_data)
  - [shop object](https://shopify.dev/docs/api/liquid/objects/shop)
  - [Google & YouTube requirements](https://help.shopify.com/en/manual/online-sales-channels/marketplaces/google/requirements)
  - [Shopify/dawn @ 258f00f](https://github.com/Shopify/dawn/tree/258f00f64365e2018ca4c62778a6bf55a5d3cd18)
  - [Shopify/horizon @ 5acd1b6](https://github.com/Shopify/horizon/tree/5acd1b6b66c02f61d3216e3adace5dd9e0404fc9)
  - Dawn demo store pages, observed 2026-10-09: `theme-dawn-demo.myshopify.com/products/studio-denim` and `/products/louise-slide-sandal`
