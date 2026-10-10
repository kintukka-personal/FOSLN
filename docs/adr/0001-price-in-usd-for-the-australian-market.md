# 1. Price in USD for the Australian Market

- **Status:** accepted
- **Date:** 9 October 2026
- **Decided in:** [Decide the launch Markets](https://github.com/Kintukka2/FOSLN-WATCH/issues/23)

## Context

FOSLN's only launch Market is Australia. Costs are in AUD: the Supplier charges A$30 per watch plus A$10 flat shipping. The owner lives in Australia, and the store pays out in AUD.

The owner wants FOSLN to look like a global brand from the first day. They also want one price list that stays the same when other Markets, such as the US, are added later.

## Decision

Prices are set and charged in **USD**, including for Australian customers.

- A price is shown as a number with a "$" sign, for example "$97". There is no rule that every price must carry "US$" or "USD".
- Every page has a **currency switcher**. It shows that the currency is USD. A shopper can switch it to see converted amounts, such as AUD, but checkout always charges in USD.
- Marketing and ads state the price currency in a disclaimer from now on.
- After FOSLN registers for GST, the USD price includes GST. GST is worked out in AUD at a consistent exchange rate.

## Alternatives considered

- **AUD only (for example "A$139").** No conversion fees and no exchange-rate exposure, and the safest choice under the Australian Consumer Law. The owner rejected it: it doesn't look global, and every price would need changing when FOSLN goes global.
- **"US$97" on every price.** This keeps the USD price and removes the ambiguity of a bare "$". The owner rejected it as a hard rule because it looks untidy. It remains the fallback if a regulator raises the issue.

## Consequences

- **Accepted legal risk.** In 2023 the Federal Court ordered Airbnb to pay A$15 million for showing Australians USD prices with a bare "$". Airbnb also had a currency selector. The owner accepts this risk. A bare "$" with a switcher is the same pattern. The fallback is to add "US$" to prices.
- **Cost to the customer.** Australian banks add 2.2–3.5% to a USD charge. The customer's bank, not FOSLN, sets the AUD amount the customer finally pays.
- **Cost to FOSLN.** While payouts are in AUD, Shopify takes a 2% conversion fee on every USD sale, whatever the store currency. On Basic, payouts are AUD only, so the fee applies ([ADR 0005](0005-usd-store-currency-on-basic.md)).
- **Exchange-rate exposure.** Costs are in AUD and revenue is in USD, so a stronger AUD cuts the margin. "Decide the launch price for each Model" sets when a currency move triggers a reprice.
- **No repricing when going global.** A new Market that uses USD can reuse the same prices.

Sources: [Selling in Australia before registering a business](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/selling-au-unregistered/docs/research/selling-au-unregistered.md), section 6.
