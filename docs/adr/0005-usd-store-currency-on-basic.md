# 5. USD store currency on Basic, accepting the 2% fee

- **Status:** accepted
- **Date:** 10 October 2026
- **Decided in:** [Choose the platform and architecture](https://github.com/Kintukka2/FOSLN-WATCH/issues/21)
- **Builds on:** [ADR 0001](0001-price-in-usd-for-the-australian-market.md)

## Context

ADR 0001 prices and charges Australian customers in USD. The owner is an Australian Individual on Shopify Payments, so payouts go to an Australian bank account in AUD.

Shopify charges a 2% conversion fee whenever a customer pays in a currency other than the payout currency. On the Basic, Grow and Shopify plans, an Australian store can only be paid out in AUD. USD payouts need Advanced or Plus.

## Decision

The store runs on **Basic**, with **USD as the store currency** and AUD payouts. Every sale pays the 2% conversion fee.

- The store currency is set to USD before any order, test orders included.
- Australia is set as the primary Market by hand, in USD, because Shopify may otherwise default the primary Market to the US.
- Prices are fixed USD amounts, such as $149 and $169, entered directly.
- The currency switcher is built into the theme. It shows the USD price first, then an "≈ A$" estimate from the European Central Bank's daily rates (via Frankfurter), saved in the browser for a day. It never changes the cart or checkout. If the rate feed fails, the estimate is hidden.

## Alternatives considered

- **AUD store currency, with the Australia Market priced in USD.** Same fees, but Shopify converts the USD price from the AUD price every day, so prices drift away from amounts ending in 9 and the price log stops matching. Rejected.
- **Advanced, with USD payouts.** This avoids the 2% fee but adds a 1.5% payout fee and about A$519 a month. It only pays off above roughly 280 sales a month, and a Wise USD account may be refused as the payout account. Rejected for launch.
- **A display-only currency app** (Nova, Beast, Zoomifi). None of their listings shows the USD price and the estimate side by side, and each one adds a script to every page. Rejected in favour of the theme-built switcher.

## Consequences

- **About A$8.20 in total fees on an ACIER sale** on an Australian card, against about A$3.93 if it were charged in AUD. The 55% price floor from "Decide the launch price for each Model" already covers this.
- **Changing the store currency later is costly.** Prices keep their numbers under a new currency label, gift cards stop working, and refunds and reports are affected. Treat USD as fixed.
- **Reports are in USD**, while costs and, later, GST are in AUD.
- **Revisit at Advanced volume.** If sales pass roughly 280 a month, USD payouts on Advanced become cheaper.

Sources: [Charging USD from an Australian Shopify store](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/shopify-usd-currency/docs/research/shopify-usd-currency.md).
