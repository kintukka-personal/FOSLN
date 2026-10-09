# Price log

The record of every regular price and every sale, for each Model. It is FOSLN's evidence that a was price was real ([ADR 0003](adr/0003-sales-need-no-minimum-hold.md)).

Add a row the same day a price changes in Shopify. Edit this file in GitHub's web editor, or ask a Claude session to do it. Never edit or delete a past row; correct a mistake with a new row. Each month, alongside the RBA rate check, confirm the latest rows match the store.

- **Kind:** `regular` for a new regular price, or `sale` for a sale.
- **Ended:** leave blank while the price still applies.
- **Reason:** why the price changed, for example "launch", "AUD reprice" or "Christmas sale, store-wide 20%".

## Prices

| Model | Kind | Price (USD) | Was price (USD) | Started | Ended | Reason |
| --- | --- | --- | --- | --- | --- | --- |
| ACIER | regular | 149 | | _launch day_ | | launch |
| CADRE | regular | 169 | | _launch day_ | | launch |
