# 3. Sales need no minimum hold

- **Status:** accepted
- **Date:** 9 October 2026
- **Decided in:** [Decide how prices are presented](https://github.com/Kintukka2/FOSLN-WATCH/issues/24)
- **Supersedes in part:** [ADR 0002](0002-no-sale-at-launch.md), its line "A sale is possible only after the regular price has held for a reasonable period"

## Context

ADR 0002 launched each Model at one regular price, with no sale, and left the rules for a later sale to this decision.

The Australian Consumer Law applies. The ACCC's test for a was price is whether consumers really paid the regular price for a reasonable period just before the sale. In the Coles case in May 2026, the Federal Court talked about roughly 12 weeks, for groceries.

The owner wants room to run sales without fixed rules. Christmas sales start in December, possibly only weeks after launch. And if a new Model is added during a store-wide sale, such as 20% off everything, it must join that sale at once, or the sale looks broken.

## Decision

A sale can start at any time, with no minimum hold, and has no rules on how long it lasts or how often it runs.

- **The plan is still to launch at plain prices.** ACIER is $149 and CADRE is $169, and the regular price is the usual state. A sale on a Model's first day is allowed but almost never used.
- **The struck-through price is always the current regular price**, never a higher past price, an "RRP" or a "value".
- **An end date shows when a sale has one, and a stated end date is never extended.**
- **Offers**, which leave the shown price unchanged, have no rules at all.
- **Every regular price and every sale is recorded** in [`docs/price-log.md`](../price-log.md), together with Shopify's order history.

## Alternatives considered

- **Hold the regular price for 12 weeks before any sale.** This is the closest reading of the Coles case. The owner rejected it because it blocks Christmas and a new Model joining a store-wide sale.
- **Hold it for 2 weeks.** This was the owner's first answer, but it still blocks a new Model joining a running sale.
- **A longer hold, with Christmas run on offers only**, such as free express delivery or a gift. The owner rejected it to keep struck-through sales available.

## Consequences

- **Accepted legal risk.** The ACCC's test looks at the period just before each sale, not at how often the regular price applies over a year. The weakest case is a sale soon after a Model first goes on sale, when few or no customers have paid the regular price. The owner accepts this risk, alongside the currency risk in [ADR 0001](0001-price-in-usd-for-the-australian-market.md).
- **The fallback** if a regulator raises the issue: stop showing struck-through prices and saving claims, and run offers only.
- **The price log is the main defence.** It shows when each regular price applied and that most of the time was spent at it. Order history shows that customers actually paid it. The log is checked each month, alongside the RBA rate check.
- **The theme shows a strikethrough only during a sale.** Shopify's compare-at field stays empty at all other times.
