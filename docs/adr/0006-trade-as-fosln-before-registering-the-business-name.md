# 6. Trade as FOSLN before registering the business name

- **Status:** accepted
- **Date:** 11 October 2026
- **Decided in:** [Decide the warranty's name and address](https://github.com/Kintukka2/FOSLN-WATCH/issues/47)

## Context

[Decide the launch Markets](https://github.com/Kintukka2/FOSLN-WATCH/issues/23) launches the owner as an Individual on Shopify Payments, with no ABN. The ABN and the "FOSLN" business name follow in the first month with more than 5 orders.

[Can FOSLN sell under its brand before registering the business name?](https://github.com/Kintukka2/FOSLN-WATCH/issues/46) found that this almost certainly breaks the Business Names Registration Act 2011:

- s 18 makes it a strict-liability offence to carry on a business under a name that isn't registered.
- An individual is exempt only for their own name, which ASIC says means "your first name and surname and nothing else".
- A store whose checkout, emails, receipts and card statement say "FOSLN" is trading under that name. A trade mark or the domain doesn't change that.
- The maximum penalty is 30 penalty units, A$10,920 from 1 July 2026. There is no infringement notice, so a penalty needs a prosecution.
- ASIC says it writes to the business first. It acts only alongside other unlawful conduct or repeated non-compliance. No prosecution of a small sole trader was found.

Registering is cheap and allowed before launch. The ABN is free and usually instant. The business name costs A$47 for 1 year and takes about 15 minutes online.

## Decision

FOSLN launches as planned, selling as FOSLN before the business name is registered.

- **The trigger is unchanged.** The ABN and the "FOSLN" business name are registered in the first month with more than 5 orders.
- **The warranty names the legal person.** Until registration it reads "This warranty is given by [the owner's full legal name], who sells as FOSLN", on the Warranty page, on the card in the box and in Terms.
- **Nowhere else carries the owner's name.** Shopify's legal business name stays blank, so Contact, Privacy and checkout show FOSLN only.
  - _Amended by [Decide the Shopify Payments and GST setup](https://github.com/Kintukka2/FOSLN-WATCH/issues/42):_ this holds only until the switch to Sole trader. From then on, Shopify's legal business name has to match the ABR entity name, which is the owner's own name. The card statement name stays "FOSLN".

## Alternatives considered

- **Register the ABN and business name before launch.** This removes the offence for A$47 and starts Shopify Payments as a Sole trader. The owner turned it down, keeping registration until sales justify it.
- **Launch under the owner's legal name only, with no FOSLN branding** until registration. This is lawful but gives up the brand at launch.

## Consequences

- **Accepted legal risk.** Until registration, FOSLN commits a strict-liability offence each day it trades, with a maximum of A$10,920. The owner accepts this alongside the currency risk in [ADR 0001](0001-price-in-usd-for-the-australian-market.md) and the sale risk in [ADR 0003](0003-sales-need-no-minimum-hold.md).
- **The fallback** if ASIC writes: register the ABN and business name the same day.
- **Registration checklist**, for the first month with more than 5 orders:
  - register the ABN and the "FOSLN" business name;
  - switch Shopify Payments from Individual to Sole trader, which means turning Payments off first. Do it on a weekday morning with the store behind its password page, keep the same payout bank account so there's no 3–5 business day payout hold, enter the details exactly as the ABR shows them, and reopen as soon as Payments is reactivated;
  - change the warranty name to "[legal name] trading as FOSLN" on the Warranty page, in Terms and on a reprinted card;
  - show the registered name and the ABN on invoices and receipts (s 19).
- **Cards are printed in small runs**, because the warranty name changes at registration.
