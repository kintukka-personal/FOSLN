# Charging USD from an Australian Shopify store

Research for [Choose the platform and architecture](https://github.com/Kintukka2/FOSLN-WATCH/issues/21), on the map [fosln.com v1: build-ready site spec](https://github.com/Kintukka2/FOSLN-WATCH/issues/4). Done on 10 October 2026. It checks the cost question left open by [ADR 0001](../adr/0001-price-in-usd-for-the-australian-market.md): can a USD base currency or a USD payout avoid Shopify's 2% conversion fee? It also checks whether a theme can show a USD price with a converted estimate beside it.

The setup assumed throughout: an Australian resident, "Individual" on Shopify Payments with no ABN, an Online Store 2.0 theme, one Market (Australia), Australian delivery addresses only, and prices set and charged in USD (ACIER US$149, CADRE US$169). This note reports what Shopify's own pages say. It is not tax or legal advice.

## Short answer

- **USD store currency is allowed.** An Australian store can pick any listed currency as its store currency. Shopify's docs keep store currency (prices, admin, reports) apart from payout currency (what lands in the bank), and give an example of a French store using USD with EUR payouts ([store vs payout currency][sp-understand]; [selling and getting paid in different currencies][sp-diffcur]). Changing it later is allowed but messy: prices keep their numbers with a new currency label, gift cards stop working, and refunds and reports are affected ([changing your store currency][sp-change]).
- **On Basic, payouts are AUD only.** Basic, Grow and Shopify plans "aren't eligible for Multi-Currency Payouts". Canada is the only country where a Basic store can take USD payouts ([payouts in multiple currencies][sp-mcp]). An Australian store needs Advanced (A$575/mo) or Plus to be paid in USD, and then pays a 1.5% Multi-Currency Payout fee (1% on Plus) ([same page][sp-mcp]; [Shopify AU pricing][au-pricing]).
- **The 2% fee depends on the payout currency, not the store currency.** Shopify charges it "when a customer pays in a currency different from your Shopify Payments payout currency" (2% for Australian stores) ([local currencies][mk-localcur]). So on Basic, every USD sale pays it, whether the store currency is AUD or USD. Only USD payouts on Advanced avoid it, and they swap it for the 1.5% payout fee plus about A$519 a month more in plan cost. On Basic, an ACIER sale on an Australian card costs about **A$8.20 (3.8%)** in total fees, against A$3.93 if it were charged in AUD.
- **An Australia Market can be priced in USD, and checkout then charges USD.** A single-country market starts in its local currency, but "You can change this to any supported currency". With Shopify Payments, the customer pays in the market's currency ([local currencies][mk-localcur]; [international payments][intl-payments]). An Australian-issued card still gets the domestic card rate, because Shopify goes by where the card was issued ([fees and costs][fees]).
- **A "USD first, estimate beside it" switcher has to be built in the theme.** Liquid has no exchange-rate data: the `currency` object holds only `iso_code`, `name` and `symbol` ([currency object][liq-currency]). The theme must get rates from somewhere else and show the estimate as plain text. Shopify's currency selector changes the cart and checkout currency, which is the opposite of what's wanted ([multiple currencies in themes][dev-multicur]). Display-only converter apps exist, such as Nova (free), Beast (free, or US$9.95/mo) and Zoomifi (free to US$19.99/mo). But their listings describe replacing the price, not showing the USD price and the estimate together.
- **Watch these conflicts:** the Australia market defaults to AUD and must be switched to USD. Shopify's help pages also say the primary market follows the store currency, so a USD store may start with the US as its primary market. Reports are shown only in the store currency, so a USD store reports in USD while GST and costs are in AUD. And the AU requirements page says the payout account must be an Australian bank account in AUD, with some fintech accounts refused. That makes a Wise USD account doubtful even on Advanced.

---

## 1. Store currency, payout currency, and changing them

### Store currency can be USD

- **Definition.** "Your store currency is the currency used within your Shopify admin for pricing products and running reports." "You can select any currency from the available options in your store settings based on your customer base's location" ([Understanding store and payout currencies][sp-understand]).
- **It needn't match the country.** Shopify's own example is a store in France with mostly US customers: "set your store currency to United States dollars (USD)", while "Your payout currency will be Euros (EUR)" ([Selling and getting paid in different currencies][sp-diffcur]). I found no Shopify page that says an Australian store must use AUD as its store currency.
- **Requirements.** Shopify Payments needs two-step authentication on the account. "The payout currencies you can choose from are dependant on the location of your store" ([Store currency overview][sp-storecur]).

### Payout currency is separate, and on Basic it's AUD

- **Definition.** "The currency your deposits from Shopify Payments orders are made in is your payout currency." The bank account "must generally be in the same country as your store" ([Understanding store and payout currencies][sp-understand]).
- **Australia's options.** The default is AUD. The non-domestic payout currencies for Australia are CAD, CHF, DKK, EUR, GBP, HKD, JPY, NOK, NZD, SEK, SGD and USD ([Supported payout currencies][sp-payoutcur]). "You can receive payouts in AUD on all subscription plans" ([Payouts in Australia][au-payouts]).
- **Plan gate.** "If your store is on the Basic, Grow, or Shopify plan, then you aren't eligible for Multi-Currency Payouts." The only exception is Canada: Basic or Grow stores there can choose USD as their default payout currency ([Payouts in multiple currencies][sp-mcp]). On the Advanced or Plus plan, an Australian store "can choose any supported payout currency", including as its default ([Payouts in Australia][au-payouts]).
- **Multi-Currency Payout fee.** It applies to payouts in any currency other than AUD. "The Multi-Currency Payout fee is based on the payout currency, not whether a currency conversion occurs." The rates for "Global regions", which I take to include Australia (the table doesn't name it), are 1.5% on Advanced and 1% on Plus ([Payouts in multiple currencies][sp-mcp]).
- **Where a USD account can be.** For an Australian entity, "CAD, EUR, GBP, NZD, and USD payouts can use an Australia-based bank account". The Australian payouts table lists USD account locations as "Australia, United States" ([Payouts in multiple currencies][sp-mcp]; [Payouts in Australia][au-payouts]).
- **Wise USD details: doubtful.** The Australian requirements page says:
  - "The bank account must be with a bank in Australia in AUD currency."
  - "Accounts that support multiple currencies simultaneously aren't supported."
  - "Services that mimic bank accounts, such as some fintech platforms, can't be used with Shopify Payments."
  - "Although non-Shopify virtual accounts are supported, you can experience more payout failures with these accounts."

  ([Shopify Payments Australia requirements][au-req]). A Wise balance holds many currencies at once, so the "multiple currencies simultaneously" rule may exclude it. That is my reading, not Shopify's statement. The question only matters on Advanced or Plus anyway.
- **"Individual" with no ABN is a valid business type.** "If you don't have an ABN, then you must select the Individual business type" ([AU requirements][au-req]). Nothing on that page links the business type to store or payout currency.

### Changing store currency after orders exist

- **Steps.** Settings > General > Store defaults > Currency display > Change store currency. Only the store owner can do it ([Changing your store currency][sp-change]).
- **What Shopify warns about** ([same page][sp-change]):
  - Product and discount prices keep their numbers but take the new label: 20 USD becomes 20 CAD.
  - Shipping rates don't update. You delete them and add them again.
  - Gift cards with a balance in the old currency stop working.
  - "Changing your store currency might affect orders with pending payments", so pending payments should be resolved first.
  - Refunds processed after the change are affected by the difference in value, and reports may be affected.
  - "Changing your store currency can have legal implications". Export products and discounts to CSV first, because the admin keeps no record of the old prices.
  - "If you aren't using Shopify Payments, then update your markets when you change your store currency." With Shopify Payments, market currencies stay as they were.
- **First-sale lock: forum reports only.** The official page doesn't mention a lock after the first sale. Several Shopify Community threads report the admin error "Currency cannot be switched from your current currency after your first sale", including after a refunded test order, and say the change then has to go through Support ([Community thread][comm-lock]; [another][comm-lock2]). These are user posts, not Shopify docs. **Set USD before the first order, test orders included.**

---

## 2. Fees: what each setup costs

### What triggers the 2% fee

- "When you accept payment in a currency that is different from your payout currency, then a currency conversion occurs." The fee is charged "in your payout currency … when you capture a payment" ([Fees and costs][fees]).
- **Rate.** "2% for stores in the UK, the EEA, Canada, Australia, New Zealand, and other regions"; 1.5% for US stores ([Local currencies, Conversion fees][mk-localcur]).
- **When it doesn't apply.** When "the customer pays in your payout currency", for example "if your payout currency is USD and the customer pays in USD, then there's no conversion and no fee" ([Local currencies][mk-localcur]).
- **How it's worked out.** Since 6 April 2026, "the currency conversion fee is now calculated directly on the gross order amount", before Shopify Payments fees come off ([Currency conversion calculation][sp-ccc]).
- **Refunds.** "Currency conversion fees and credit card fees are not returned to you when you issue a refund", but there's no second conversion fee on the refund or a chargeback ([Fees and costs][fees]).
- **Exchange rate.** "The foreign exchange rate used is always the rate at the time of the transaction" ([Exchange rates][fx-rates]). Shopify doesn't name its rate source or any markup beyond the 2%.
- **Effect.** The fee depends on the payout currency. **Changing the store currency to USD doesn't avoid it** while payouts are AUD. ADR 0001's line "While the store's base currency is AUD, Shopify takes a 2% conversion fee" should read "while payouts are in AUD".

### Card rates (Basic and Advanced, AU)

| Plan (monthly / yearly billing) | Domestic card | Amex | International card | Source |
|---|---|---|---|---|
| Basic, A$56 / A$42 per month | 1.7% + 30¢ | 2.9% + 30¢ | 3.5% + 30¢ | [Shopify AU pricing][au-pricing] |
| Grow, A$149 / A$114 per month | 1.55% + 30¢ | 2.8% + 30¢ | 3.4% + 30¢ | same |
| Advanced, A$575 / A$431 per month | 1.35% + 30¢ | 2.7% + 30¢ | 3.3% + 30¢ | same |

- **Domestic or international?** It depends on where the card was issued: domestic means "issued in the same country or region as your store" ([Fees and costs][fees]). So an Australian card paying in USD should get the domestic rate. That's my reading; Shopify doesn't give this exact case.
- **GST on the fees.** "In Australia, GST at the current rate of 10% is included in all Shopify Payments processing fees" ([AU tax reporting][au-tax]).

### Cost per ACIER sale (US$149 ≈ A$213.55)

At US$1 = A$1.4332, the rate used in [the AU/UK/CA note](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/au-uk-ca-markets/docs/research/au-uk-ca-markets.md). All figures are my arithmetic from the rates above. The Multi-Currency Payout fee is applied to the net payout.

| Setup | Plan | Fees on an Australian card | Fees on a foreign card | Monthly plan | Notes |
|---|---|---|---|---|---|
| **A. USD store currency, Australia market in USD, AUD payout** | Basic | 1.7% + 30¢ = A$3.93, plus 2% conversion = A$4.27. **Total A$8.20 (3.8%)** | 3.5% + 30¢ = A$7.77, plus A$4.27. **Total A$12.05 (5.6%)** | A$56 | Prices stay exactly $149. The fee comes out of the payout ([Local currencies][mk-localcur]) |
| **B. USD store currency, USD payout** | Advanced only | 1.35% + 30¢ = A$3.18, plus 1.5% payout fee ≈ A$3.16. **Total ≈ A$6.34 (3.0%)** | 3.3% + 30¢ = A$7.35, plus ≈ A$3.09. **Total ≈ A$10.44** | A$575 | Saves about A$1.86 a sale over A. That covers the extra A$519 a month only above roughly **280 sales a month** (about 210 on yearly billing). FOSLN then holds USD and pays its own FX to cover AUD costs |
| **C. AUD store currency, Australia market priced in USD** | Basic | Same as A: **A$8.20** | Same as A: **A$12.05** | A$56 | USD prices float. "If your primary market currency is different than your store currency, then your base prices are automatically converted using a daily currency conversion rate" ([Managing markets][mk-managing], read via search extract). "Manual conversion rates can't be applied to your primary market" ([Exchange rates][fx-rates]). With automatic rates, the 2% is added to the price shown ([Local currencies][mk-localcur]). So ACIER's USD price would be its AUD base price converted at the day's rate plus 2%, and would change from day to day rather than staying at $149, unless a fixed price can be set on the primary market (see Open) |
| *Baseline: everything in AUD* | Basic | 1.7% + 30¢ = **A$3.93 (1.8%)** | 3.5% + 30¢ = A$7.77 | A$56 | For comparison only. ADR 0001 rejected it |

- **No setup on Basic avoids the 2%.** Setup A is the cheapest way to charge USD on Basic and the only one that keeps prices at exactly $149 and $169. Setup C costs the same and makes the prices drift.
- **At payout (setups A and C):** Shopify converts USD to AUD when the payment is captured, at its rate at that moment, and takes the 2% then ([Fees and costs][fees]; [Exchange rates][fx-rates]). No further conversion happens at payout.
- **At payout (setup B):** no conversion happens. The 1.5% payout fee applies, and FOSLN's bank or Wise sets the rate when FOSLN converts USD to AUD itself. That cost isn't in the table.
- **Customers' bank fees aren't FOSLN's.** "International transaction fees are charged by banks to customers making cross-border purchases, but these fees aren't charged to you as the merchant" ([Fees and costs][fees]). The section 6 table in [the selling-in-Australia note](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/selling-au-unregistered/docs/research/selling-au-unregistered.md) lists 2.2–3.5% at the big four banks.

---

## 3. The Australia Market charging USD

- **A market can use a currency other than its country's.** "When you create a market for a single country, the market's currency is automatically set to that country's local currency. You can change this to any supported currency." The steps are Markets > the market > add a currency > choose Dynamic or Manual ([Local currencies][mk-localcur]).
- **Checkout charges the market currency.** "Only stores with Shopify Payments or Adyen as the primary gateway can process payments in a customer's local currency." The compatible methods are Shopify Payments cards, Shop Pay, Apple Pay, Google Pay and PayPal Express ([International payments][intl-payments]). "Local currency" here means the market's currency. So an Australia market set to USD charges Australian cards in USD. That's my reading; Shopify's pages use the term "local currency" throughout.
- **When the market currency equals the store currency (setup A), nothing is converted.** The store price is the market price, so the 2% fee isn't built into it. The fee comes out of the AUD payout instead.
- **Primary market.** Shopify's help says: "Your primary market is determined by your store currency in Settings > General". Its example is a US-address store set to CAD, which gets Canada as its primary market ([Managing markets][mk-managing2]). A 2023 changelog post says merchants "can change your primary market to a country and currency of your preference", but the primary market "still can only contain one country" ([Changelog, 30 Aug 2023][cl-primary]). So with a USD store currency, check that Australia is the primary market, and set it if not.
- **GST display.** Each market has a Duties and taxes setting with three options: Dynamic tax display ("Product prices include taxes in regions where appropriate, such as Australia"), Show as included, or Show as line item. "To collect sales tax for a market, you must first enter a tax registration for the regions that the market includes" ([Duties and taxes][mk-tax]).
  - **Before GST registration**, with no tax registration in Shopify, Shopify collects no GST. The $149 is the whole price.
  - **After registration**, with tax-inclusive display, "The subtotal and total are the same", and tax is backed out of the price: Tax = (Rate × Price) / (1 + Rate) ([Including or excluding tax][tax-incl]). On ACIER that's US$13.55 of GST in the $149. In AUD that's about A$19.41 at 1.4332, which is my arithmetic.
  - Shopify's tax pages don't say what currency the GST figure is worked out or reported in. The ATO wants it in AUD ([selling-in-Australia note, section 6(b)](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/selling-au-unregistered/docs/research/selling-au-unregistered.md)).
  - "You can view your reports only in your store currency" ([International pricing limitations][intl-limits]). In setup A that's USD, so the owner must convert for BAS.
  - Australia isn't covered by Shopify Tax. It uses "Shopify's Basic Tax service" ([Taxes overview][taxes]).
- **Checkout showing "$149.00 USD".** "Regardless of the currency format that you choose, your store's checkout always displays decimal places." Currency formatting settings apply only to the online store, emails and order printer, and "only apply to your store's base currency" ([Currency formatting][fmt]). I found no page that says how checkout shows the currency code. See Open.

---

## 4. The estimate switcher

### What Shopify gives a theme

- **No rates in Liquid.**
  - The `currency` object has only `iso_code`, `name` and `symbol` ([Liquid currency][liq-currency]).
  - `shop.currency` is "The currency of the store"; `shop.enabled_currencies` is "The currencies that the store accepts". Neither carries a rate ([Liquid shop][liq-shop]).
  - `cart.currency` is the customer's presentment currency on multi-currency stores ([Liquid cart][liq-cart]).
  - `localization` exposes the available countries, the selected country and the market, but no rates ([Liquid localization][liq-local]).
- **Money filters format, they don't convert.** `money` "Formats a given price based on the store's HTML without currency setting" ([money filter][liq-money]).
- **Shopify's selector changes what's charged.** The theme country selector sits in a localization form, and customers then "browse the shop, and checkout, in their preferred currency" ([Support multiple currencies and languages][dev-multicur]). That's Markets currency switching, which charges in the converted currency. The brief rules that out. With only Australia/USD enabled, the native selector would have nothing else to offer anyway.
- **`Shopify.currency.rate` is undocumented.** I found no shopify.dev page for the `Shopify.currency` storefront JavaScript object. It appears only in community posts ([Community thread][comm-shopifycur]). It reflects the presentment currency against the store currency. With USD as the only enabled currency it would hold no AUD rate (my inference, not tested).
- **A legacy rates file exists but is undocumented.** `https://cdn.shopify.com/s/javascripts/currencies.js` returns `var Currency={rates:{USD:1,…,AUD:.695948,…}}`, which works out to US$1 ≈ A$1.437 when fetched on 10 October 2026. Old themes used it for display-only converters. I found no shopify.dev page for it, and its update frequency is unknown. Don't rely on it.
- **So the theme needs its own rate source.** Either a third-party FX API called from the browser, or a rate stored in a shop metafield and updated on a schedule by an app or a Flow automation. The estimate is then text the theme draws next to the Liquid USD price, for example `$149 · ≈ A$214`. The cart and checkout templates never draw it. This is a design inference. Shopify has no page on this pattern.

### Shopify's guidance on third-party converters

- "Some currency conversion apps in the Shopify App Store aren't compatible with selling in local currencies with Shopify Payments. Some apps can cause prices to be displayed in a customer's chosen currency, but at checkout the currency reverts back to the store's default currency" ([International pricing limitations][intl-limits]). For FOSLN that "revert" is the intended behaviour. The risk is a mismatch between the price shown and the price charged, which is why the brief keeps USD first and labels the estimate.
- When a converter is display-only, "There are no currency conversion fees applied by Shopify" ([International payments][intl-payments]).

### Display-only converter apps (read 10 October 2026)

| App | Developer | Price | Checkout | Install method | Source |
|---|---|---|---|---|---|
| Nova: Multi Currency Converter | Nova Apps | Free | Shows "the customers local currency" and "still use your shop currency at the checkout" | Not stated | [listing][app-nova] |
| Beast Currency Converter | Grizzly Apps SRL | Free plan. Elite US$9.95/mo or US$95.52/yr | "Shopify doesn't allow developers to convert prices on the Checkout page". Elite adds a "Checkout currency notification". Built for Shopify | "placed automatically in the header of any Shopify theme". Theme app embed not stated | [listing][app-beast] |
| Zoomifi – Currency Converter | Zoomifi | Free, Starter US$9.99/mo, Standard US$19.99/mo | Not stated | "an Online Store 2.0 theme app embed so you can go live without editing theme code" | [listing][app-zoomifi] |

- **Not display-only (avoid for this brief):**
  - Currency Converter Bear (US$9.99/mo) "Integrates with Shopify Markets to support all currencies in checkout" ([listing][app-bear]).
  - KILATECH (free, or US$4.99/mo) lists "Local currency checkout" ([listing][app-kilatech]).
  - Hoppy (free) mentions "Checkout Conversion" ([listing][app-hoppy]).
- **Fit with the brief.** None of the three display-only listings says it can show the USD price and the estimate side by side ("$149 · ≈ A$214"). Their screenshots and copy describe swapping the shown price for the converted one, and the brief forbids a converted amount on its own. My reading is that a small custom theme block meets the brief more reliably than any of these apps. Confirm with each developer before relying on an app.

---

## 5. Other conflicts in Shopify

- **The Australia market defaults to AUD.** A single-country market is "automatically set to that country's local currency" ([Local currencies][mk-localcur]). Even with a USD store currency, the Australia market must be set to USD by hand, or Australians are shown and charged auto-converted AUD prices with the 2% built in.
- **Market currencies don't follow a store currency change on Shopify Payments.** Only stores not on Shopify Payments have their market currencies updated when the store currency changes ([Changing your store currency][sp-change]). After switching the store to USD, check the Australia market's currency separately.
- **Primary market follows the store currency.** A USD store currency may make the US the primary market ([Managing markets][mk-managing2]). Set it back to Australia ([Changelog][cl-primary]).
- **Payout account rules.** The AU requirements ask for an Australian bank, an AUD account, and BECS eligibility ([AU requirements][au-req]). On Basic this is the only option. It rules out a USD account for the default payout.
- **No rule ties store currency to payout country.** I found none. The only country rules are on the payout bank account's location ([Understanding store and payout currencies][sp-understand]).
- **Reports in USD.** Reports are shown only in the store currency ([International pricing limitations][intl-limits]). In setup A, the admin, reports and shipping rates are all in USD, while Supplier costs, GST and payouts are in AUD.
- **Shipping rates** are entered in the store currency, so in USD under setup A. They don't convert when the store currency changes ([Changing your store currency][sp-change]).
- **Payment methods.** Shopify lists Shopify Payments cards, Shop Pay, Apple Pay, Google Pay and PayPal Express as supporting local-currency payment ([International payments][intl-payments]). Other methods may not support a USD charge in an Australian market. See Open.
- **Checkout restrictions.** Shopify can switch the checkout currency to match the shipping country on one-page checkout ([Checkout restrictions][mk-checkout]). With Australia as the only market and AU-only shipping, that shouldn't happen.
- **Refunds move with the exchange rate.** Under setups A and C, a refund goes back in USD, and the AUD it costs depends on the rate at the time. Shopify's local-currencies page has sections on rate moves for and against the merchant ([Local currencies][mk-localcur]).

---

## Open or unverified

- **First-sale currency lock.** It's reported only in Shopify Community threads. The official page lists consequences but no lock.
- **Fixed USD prices on a primary market whose store currency is AUD (setup C).** The 2023 changelog says the primary market allows "custom pricing", but I found no current help page that confirms fixed prices on the primary market. If they work, C would show exactly $149 but still pay 2%.
- **Australia in the Multi-Currency Payout "Global regions" row.** The fee table doesn't name Australia. I assume the 1.5% Advanced and 1% Plus rates apply.
- **Wise USD account details** (US routing details held by Wise) for a USD payout on Advanced. The requirements page both supports "non-Shopify virtual accounts" and refuses multi-currency accounts and "services that mimic bank accounts".
- **Card rate for an Australian card paying USD.** Domestic is my reading of "issued in the same country or region as your store". No page addresses this exact case.
- **How the 30¢ AUD per-transaction fee is charged** on a USD transaction paid out in USD (setup B).
- **Shopify's FX rate source and any spread** beyond the 2% fee.
- **How checkout shows the currency code**, for example "$149.00 USD" or "USD $149.00". No help page describes it. Test it on a dev store.
- **What currency Shopify's GST figure is in** for a USD-priced Australian market, and whether Shopify's reports give GST in AUD.
- **Afterpay, Zip, Klarna and PayPal in a USD-priced Australia market.** Not covered on the AU payment methods page.
- **`Shopify.currency` and `currencies.js`.** Both are undocumented. How they behave with USD as the only enabled currency hasn't been tested.
- **Display-only apps.** Whether any listed app supports a "USD first, estimate beside it" layout, and whether Nova installs as a theme app embed.
- **Whether Shopify's AUD plan prices include GST.** Carried over, still open, from the selling-in-Australia note.

---

## Sources

All pages were read on 10 October 2026. Shopify Help Center pages were read with WebFetch unless marked as a search extract.

[sp-storecur]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency
[sp-understand]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/understanding-store-and-payout-currencies
[sp-diffcur]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/selling-and-getting-paid-in-different-currencies
[sp-change]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/changing-your-store-currency
[sp-payoutcur]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/supported-payout-currencies
[sp-mcp]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/payouts-in-multiple-currencies
[sp-ccc]: https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/currency-conversion-calculation
[au-payouts]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payouts
[au-req]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/requirements
[au-tax]: https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/tax-reporting
[au-pricing]: https://www.shopify.com/au/pricing
[fees]: https://help.shopify.com/en/manual/international/pricing/fees
[fx-rates]: https://help.shopify.com/en/manual/international/pricing/exchange-rates
[intl-payments]: https://help.shopify.com/en/manual/international/payments
[intl-limits]: https://help.shopify.com/en/manual/international/pricing/limitations
[mk-localcur]: https://help.shopify.com/en/manual/markets/customizations/local-currencies
[mk-managing]: https://help.shopify.com/en/manual/markets/managing-markets
[mk-managing2]: https://help.shopify.com/en/manual/markets/getting-started/managing-markets
[mk-tax]: https://help.shopify.com/en/manual/markets/customizations/duties-and-taxes
[mk-checkout]: https://help.shopify.com/en/manual/markets/pricing/checkout-restrictions
[fmt]: https://help.shopify.com/en/manual/markets/pricing/currency-formatting
[tax-incl]: https://help.shopify.com/en/manual/taxes/include-exclude-taxes
[taxes]: https://help.shopify.com/en/manual/taxes
[cl-primary]: https://changelog.shopify.com/posts/stores-primary-market-can-now-be-customized
[dev-multicur]: https://shopify.dev/docs/storefronts/themes/markets/multiple-currencies-languages
[liq-currency]: https://shopify.dev/docs/api/liquid/objects/currency
[liq-shop]: https://shopify.dev/docs/api/liquid/objects/shop
[liq-cart]: https://shopify.dev/docs/api/liquid/objects/cart
[liq-local]: https://shopify.dev/docs/api/liquid/objects/localization
[liq-money]: https://shopify.dev/docs/api/liquid/filters/money
[app-nova]: https://apps.shopify.com/auto-currency-switcher-1
[app-beast]: https://apps.shopify.com/doubly-currency-converter
[app-zoomifi]: https://apps.shopify.com/easy-currency-converter
[app-bear]: https://apps.shopify.com/auto-multi-currency-converter
[app-kilatech]: https://apps.shopify.com/currency-converter-ultimate
[app-hoppy]: https://apps.shopify.com/hoppy-currency-converter
[comm-lock]: https://community.shopify.com/c/technical-q-a/there-is-1-error-currency-cannot-be-switched-from-your-current/td-p/1913759
[comm-lock2]: https://community.shopify.com/c/shopify-discussions/how-can-i-change-my-store-currency-after-the-first-order/td-p/2398713
[comm-shopifycur]: https://community.shopify.com/t/shopify-country-mismatch-with-shopify-currency/344687

**Shopify Payments: currency and payouts**
- *Store currency* overview. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency
- *Understanding store and payout currencies*. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/understanding-store-and-payout-currencies
- *Selling and getting paid in different currencies*. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/selling-and-getting-paid-in-different-currencies
- *Changing your store currency*. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/changing-your-store-currency
- *Supported payout currencies*. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/supported-payout-currencies
- *Payouts in multiple currencies*. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/payouts-in-multiple-currencies
- *Currency conversion calculation*. https://help.shopify.com/en/manual/payments/shopify-payments/store-currency/currency-conversion-calculation
- *Payouts and settlement times in Australia*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/payouts
- *Shopify Payments Australia requirements*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/requirements
- *Tax reporting with Shopify Payments for Australia*. https://help.shopify.com/en/manual/payments/shopify-payments/supported-countries/australia/tax-reporting
- *Shopify AU pricing*. https://www.shopify.com/au/pricing

**International selling and Markets**
- *Fees and costs*. https://help.shopify.com/en/manual/international/pricing/fees
- *Exchange rates*. https://help.shopify.com/en/manual/international/pricing/exchange-rates
- *International payments*. https://help.shopify.com/en/manual/international/payments
- *International pricing limitations*. https://help.shopify.com/en/manual/international/pricing/limitations
- *Setting up currencies for markets* (local currencies). https://help.shopify.com/en/manual/markets/customizations/local-currencies
- *Managing markets* (read via search extract). https://help.shopify.com/en/manual/markets/managing-markets
- *Managing markets* (getting started). https://help.shopify.com/en/manual/markets/getting-started/managing-markets
- *Duties and taxes* for markets. https://help.shopify.com/en/manual/markets/customizations/duties-and-taxes
- *Checkout restrictions*. https://help.shopify.com/en/manual/markets/pricing/checkout-restrictions
- *Currency formatting*. https://help.shopify.com/en/manual/markets/pricing/currency-formatting
- *Including or excluding tax on product prices*. https://help.shopify.com/en/manual/taxes/include-exclude-taxes
- *Taxes* overview. https://help.shopify.com/en/manual/taxes
- Shopify Changelog, *Stores' Primary market can now be customized* (30 Aug 2023). https://changelog.shopify.com/posts/stores-primary-market-can-now-be-customized

**Theme development (shopify.dev)**
- *Support multiple currencies and languages*. https://shopify.dev/docs/storefronts/themes/markets/multiple-currencies-languages
- Liquid `currency`, `shop`, `cart` and `localization` objects; `money` filter. https://shopify.dev/docs/api/liquid
- Legacy `currencies.js` (undocumented; fetched directly). https://cdn.shopify.com/s/javascripts/currencies.js

**Shopify App Store**
- Nova: Multi Currency Converter. https://apps.shopify.com/auto-currency-switcher-1
- Beast Currency Converter. https://apps.shopify.com/doubly-currency-converter
- Zoomifi – Currency Converter. https://apps.shopify.com/easy-currency-converter
- Currency Converter Bear. https://apps.shopify.com/auto-multi-currency-converter
- KILATECH Currency Converter. https://apps.shopify.com/currency-converter-ultimate
- Hoppy Multi Currency Converter. https://apps.shopify.com/hoppy-currency-converter

**Shopify Community (not official; used only where marked)**
- *Currency cannot be switched … after your first sale*. https://community.shopify.com/c/technical-q-a/there-is-1-error-currency-cannot-be-switched-from-your-current/td-p/1913759
- *How can I change my store currency after the first order?* https://community.shopify.com/c/shopify-discussions/how-can-i-change-my-store-currency-after-the-first-order/td-p/2398713
- *Shopify.country mismatch with Shopify.currency*. https://community.shopify.com/t/shopify-country-mismatch-with-shopify-currency/344687
