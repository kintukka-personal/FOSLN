# 4. A Shopify theme, not headless

- **Status:** accepted
- **Date:** 10 October 2026
- **Decided in:** [Choose the platform and architecture](https://github.com/Kintukka2/FOSLN-WATCH/issues/21)

## Context

fosln.com needs a commerce platform that carries the locked visual direction (Direction D, Two Metals) and the PDP layout (Layout E), keeps checkout on fosln.com, and never asks the owner to use a terminal. Claude Code sessions do all the building, and the owner runs the store from GUIs.

Both reference designs are plain HTML, CSS and JavaScript. The live quartz hands, the metal bands, the spec drawing and the wrist guide all run in the browser, so no stack renders them better than another.

## Decision

fosln.com v1 is a **Shopify Online Store 2.0 theme**: a custom theme built from scratch on JSON templates, sections and theme blocks, with Shopify's Horizon theme as a reference only. Headless is ruled out for v1, not kept as a fallback.

- The theme sits at the root of this repo's `main`, next to `CLAUDE.md`, `GLOSSARY.md` and `docs/`. `main` is connected to the live theme through Shopify's GitHub integration, and theme-editor saves commit back as the `shopify` bot.
- Changes go straight to live. Each session runs Theme Check and renders the changed pages, pulls the bot's commits, then opens and merges its own pull request, so GitHub's **Revert** button undoes any change.
  - _Amended by [Set performance and accessibility targets](https://github.com/Kintukka2/FOSLN-WATCH/issues/55#issuecomment-6142775088):_ changes go to a `preview` branch first, connected to an unpublished theme. The session runs Theme Check, Lighthouse and axe against that theme's preview link, and merges to `main` only when all pass. Revert stays the rollback.
- The owner edits store data in the admin, and copy and images in the theme editor. Layout, the metal treatments, motion, the spec drawing and the wrist guide are code-only.
- An app is added only when a ticket decides one is needed. It must install as a theme app block or app embed.

## Alternatives considered

- **Shopify headless (Hydrogen on Oxygen, or Next.js on Vercel).** Rejected. It loses the theme editor and theme app blocks, needs the cart code rewritten, moves checkout to a subdomain such as `checkout.fosln.com`, and needs a Claude Code session for every quarterly API version and framework patch. It doesn't fix the one performance risk found, the hero's slow largest paint, because that comes from the design.
- **Squarespace, Webflow, BigCommerce or WooCommerce.** Rejected. Each fails a test that matters: two-way GitHub sync, links to China-based Suppliers, sales caps, or upkeep the owner can't do without a terminal.
- **Forking Horizon or starting from Skeleton.** Rejected. Horizon brings a large codebase the design doesn't use. Skeleton has no sections or JSON templates, so it has nowhere for the editable content and misses Shopify's streamed HTML.

## Consequences

- **Lock-in.** Liquid only runs on Shopify. Leaving Shopify means rebuilding the theme, though product data can be exported.
- **Checkout styling is limited.** Below Plus, the checkout can be restyled (logo, colours, fonts) but not redesigned.
- **No preview step.** A bad push reaches the live store immediately. Theme Check, a render check and the Revert button are the safety net.
  - _Amended by [Set performance and accessibility targets](https://github.com/Kintukka2/FOSLN-WATCH/issues/55#issuecomment-6142775088):_ the preview theme now catches a bad change before it reaches the live store. The release gates are a Lighthouse mobile score of 90 or more and WCAG 2.2 AA.
- **Shopify's scripts stay.** `content_for_header` can't be removed, so every app embed adds weight to every page.

Sources: [Can a Shopify theme carry the FOSLN designs?](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/theme-vs-headless/docs/research/theme-vs-headless.md), [How Shopify's GitHub sync works with this repo](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/shopify-github-sync/docs/research/shopify-github-sync.md), [Is there a better platform than Shopify?](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/platform-alternatives/docs/research/platform-alternatives.md), [Putting checkout on fosln.com via Cloudflare](https://github.com/Kintukka2/FOSLN-WATCH/blob/research/domain-checkout-cloudflare/docs/research/domain-checkout-cloudflare.md).
