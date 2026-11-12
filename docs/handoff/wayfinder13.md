# Handoff: the fosln.com v1 wayfinding chat ("wayfinder13")

Written 12 November 2026, to carry this chat into a new one. Paste the "Start a new chat" prompt at the end into the new chat.

## The goal

The map is **[fosln.com v1: build-ready site spec](https://github.com/Kintukka2/FOSLN-WATCH/issues/4)**, GitHub issue #4, labelled `wayfinder:map`.

Its destination is a spec for fosln.com v1, made for both Models (ACIER and CADRE), with nothing left to decide. A separate build effort then starts from it. The spec covers:

- the platform and architecture;
- the visual direction;
- the PDP, including the spec table and fit guidance;
- what launches: the pages and the Markets;
- domain, checkout and payments;
- shipping, returns and warranty, and how orders reach the Supplier.

**Planning only.** Tickets produce decisions, not deliverables. Building and launching the site is out of scope.

## Where things stand

**Every ticket on the map is closed except one:** [Verify the specs against the samples](https://github.com/Kintukka2/FOSLN-WATCH/issues/36) (#36, a task). The map has no fog left: "Not yet specified" reads "Nothing left: every open area is now a ticket."

Once #36 closes, the map has reached its destination.

## How we work (the conventions to keep)

### Method and voice

- **Skill:** `/anthropic-skills:wayfinder` with the map URL. Each ticket is a sub-issue of #4, labelled `wayfinder:research`, `prototype`, `grilling` or `task`.
- **Grilling:** rounds of numbered questions, each with a recommendation (❓ / ➡️). The owner usually answers "go with your recs", so make the recommendations strong.
- **Owner's standing rules:**
  - no terminal for the owner, ever;
  - the owner works in GUIs;
  - plain language;
  - refer to tickets by name, not number.

### Workflow on GitHub

- **Resolving a ticket:**
  1. Post a `## Resolution` comment.
  2. Close the ticket.
  3. Add one line to the map's "Decisions so far", above "## Not yet specified".
  4. Post `## Input from "<ticket>"` comments on any ticket the decision affects.
- **Claiming:** assign the ticket to `Kintukka2` before working on it.
- **Blocking:** use GitHub's native "blocked by" (the REST `dependencies/blocked_by` endpoint).
- **GitHub access:** in cloud sessions, use the REST `gh api` calls described in `docs/agents/issue-tracker.md`. GraphQL `gh issue` doesn't work there.

### Research and repo changes

- **Research tickets** are done by background subagents in their own git worktrees. Findings go to `docs/research/<name>.md` on a `research/<name>` branch, and the ticket links to it.
- **ADR amendments** go in as inline `_Amended by [ticket](link):_` sub-bullets, through a draft PR from the session branch. The owner merges.
- **Session branch:** reset to `origin/main` after each merge.
- **Git hygiene:** add `.claude/worktrees/` to `.git/info/exclude` so the stop hook doesn't flag agent worktrees.

### Vocabulary

- **Glossary terms:** Model, Dial, Supplier, Dispatch, Market, Regular price, Was price, Sale, Offer, Return, Altered, Fault claim (see `GLOSSARY.md`).
- **Banned claims:**
  - "gold": say "gold-tone";
  - "silver": say "silver-tone";
  - "stainless steel case", until the Supplier confirms it in writing;
  - "Hardlex";
  - any origin claim.

## What this chat did

Earlier chats made the first 25 or so decisions; all of them are on the map. This chat resolved the rest, in this order:

| Ticket | Decision in one line |
| --- | --- |
| [Design the home page](https://github.com/Kintukka2/FOSLN-WATCH/issues/34#issuecomment-6164255086) | Hero **B "One dial"**: one faint 60-tick dial spans the onyx and warm-black halves (steel ticks for ACIER, gold for CADRE), with the 12-to-6 axis as the seam. The **Relay** build-in runs ACIER then CADRE in about 2.1 s, once per visit, and is static under reduced motion. The **Columns** promise strip. **Onyx + Champagne** Dials |
| [Decide the product data model](https://github.com/Kintukka2/FOSLN-WATCH/issues/41#issuecomment-6120729060) | One product per Model with a "Dial" option, on one shared template. 20 typed `fosln` product metafields holding verified figures only; an empty field hides its row. Variant fields `statement` and `bench`. A Wrist photo metaobject. Gender Unisex, age Adult. Owner creates the definitions once and imports a CSV Claude generates. No app |
| [Decide the imagery production plan](https://github.com/Kintukka2/FOSLN-WATCH/issues/43#issuecomment-6134515185) | The owner photographs the real samples in a 40 cm LED tent with an Officeworks Otto tripod (about A$160). Claude edits. Renders and AI images are built only from those photos and must match the real watch, plate text included. The plate is never retouched. On-wrist photos are always real. No Supplier photos. Bench cut-outs on one fixed canvas, reused for the home hero and share images |
| [What analytics, consent and email capture involve…](https://github.com/Kintukka2/FOSLN-WATCH/issues/52#issuecomment-6136007313) (research) | No cookie-banner law. FOSLN is very likely exempt from the Privacy Act unless it shares customer data for a service. The Spam Act applies to everyone. A privacy policy is required by Shopify's terms anyway |
| [Search demand and the Merchant Center feed](https://github.com/Kintukka2/FOSLN-WATCH/issues/56#issuecomment-6136147943) (research) | Australians search colour and shape words ("gold watch", "square watch"). Google accepts a USD feed for Australia, but Shopify's channel may not, so test it. No GTIN: the SKU is the MPN |
| [Decide social proof before any reviews exist](https://github.com/Kintukka2/FOSLN-WATCH/issues/54#issuecomment-6135908726) | No review area at launch. Judge.me (free) sends one request 10 days after delivery. Verified buyers only, no rewards, every rating published. Reviews show from 3, and the theme adds the rating to the ProductGroup markup |
| [Set performance and accessibility targets](https://github.com/Kintukka2/FOSLN-WATCH/issues/55#issuecomment-6142775088) | "Good" Core Web Vitals, and Lighthouse mobile 90 or more before release. PDP budgets: 1 MB total, 60 KB theme JavaScript, 120 KB fonts. WCAG 2.2 AA. A **`preview` theme branch**: Theme Check, Lighthouse and axe must pass before merging to `main`. ADR 0004 amended |
| [Decide analytics, consent and email capture at launch](https://github.com/Kintukka2/FOSLN-WATCH/issues/53#issuecomment-6159105764) | Shopify Analytics only; no GA4, Meta pixel or Ads tags. Free listings with conversion tracking off. No banner. No email capture. Email footers read "Sent by [legal name], who sells as FOSLN" with the PO Box. ADR 0006 amended |
| [Decide page titles, search terms and the Merchant Center feed](https://github.com/Kintukka2/FOSLN-WATCH/issues/57#issuecomment-6161030473) | Titles follow "Model - shape, colour-tone, size \| FOSLN". Prices written "US$" in meta descriptions. Copy unchanged. SKUs `FOSLN-ACIER-CHR/ONX`, `FOSLN-CADRE-NOI/CHA` serve as MPNs. USD feed tested at setup. No blog or gender pages |

### Also done in this chat

- **New tickets:** the map's last four open areas became tickets #52 to #57 (two research, four grilling).
- **Merged PRs:**
  - [#51](https://github.com/Kintukka2/FOSLN-WATCH/pull/51): the home page prototype in `prototypes/home-page/`;
  - [#58](https://github.com/Kintukka2/FOSLN-WATCH/pull/58): ADR 0004, the preview theme;
  - [#59](https://github.com/Kintukka2/FOSLN-WATCH/pull/59): ADR 0006, email footers;
  - [#60](https://github.com/Kintukka2/FOSLN-WATCH/pull/60): the prototype opens on the chosen design.
- **Research branches:** `research/analytics-consent-email` and `research/search-and-feed`.

## Live pages (private to the owner)

- **Home page prototype:** https://claude.ai/artifact/S6TM9quEXrgSCy3zgkKVji. It opens on the chosen design. Source: `prototypes/home-page/` on `main`.
- **Sample checklist and shot list:** https://claude.ai/artifact/Jwg9pKE3AFziVqGE3EqCcH. One page with these sections:
  - the parcel's contents;
  - measurements for both Models;
  - checks on each of the four Dials;
  - the Supplier's written answers, plus a ready-to-send AliExpress message with a copy button;
  - the shoot: light-tent shots per Model, then the wrist shots, the wearer's consent and an "Add photo" upload on every shot.

  **Saving:** every answer saves automatically to the page's database, and photos go to its asset store.
  - **Reading the answers:** `ArtifactData` with `action: "list"`, `collection: "checks"`.
  - **Doc ids:** `parcel`, `acier`, `acier-chrome`, `acier-onyx`, `cadre`, `cadre-noir`, `cadre-champagne`, `supplier`, `shoot-acier`, `shoot-cadre`, `shoot-wrist`.
  - **Shot fields:** each is stored as `{done, photos: [assetId…]}`.
  - **Downloading a photo:** `Artifact` read with `path: <assetId>`.
  - **Source file:** the scratchpad of this session only, so a new session republishes by `url` after reading the page.

## What's next

### 1. Finish "Verify the specs against the samples" (#36)

The samples arrived on 16 October 2026. **As of 12 November the checklist page is still empty**: no measurements, answers or photos have been recorded.

**The owner:**
1. Fills in the checklist page: the parcel, measurements, the Dial checks and the plate text.
2. Sends the Supplier message (if not already sent) and pastes the replies in.
3. Does the shoot the same day, uploading each photo.

**Then Claude:**
1. Reads the page's data and photos.
2. Updates [`docs/specs/models.md`](../specs/models.md):
   - every averaged figure and every TBC becomes measured;
   - the status line changes from provisional to verified;
   - the wrist range is worked out from the circumference, the removable links and the pitch.
3. Checks any figure that moved against the spec table and fit guidance.
4. Settles the existing ACIER renders: they stay if the plate reads QUARTZ, and are retired if it reads QUARTA.
5. If the Supplier confirms stainless steel in writing, updates the claims: "stainless steel case" becomes allowed, and ACIER's title may become "Round stainless steel watch".
6. Posts the resolution, closes #36, and adds the line to the map.
7. Then declares the map complete: comment on #4 that the destination is reached.

Producing the product CSV and editing the photos (cut-outs, swatches, share images) are **build-effort work**, not this map's. Note them as the hand-off.

### 2. After the map closes: the build effort (a separate effort)

These checks are already logged in the resolutions, to run at store setup:

- **Shopify setup:**
  - set USD as the store currency before any order;
  - Australia as the only Market;
  - connect the domain behind the password page.
- **Release branches:** connect `main` to the live theme and a `preview` branch to an unpublished theme.
- **Swatches:** test that a Color-linked option still shows "Dial" to shoppers. If not, fall back to the `fosln.swatch` variant field.
- **Google & YouTube:** test whether the channel accepts a USD feed for Australia. If not, free listings wait.
- **Judge.me:** check whether it fills Shopify's standard rating fields.
- **Privacy:** keep Shopify Network Intelligence off.
- **Accounts to set up:** the Private PO Box, MyPost Business and a FOSLN-only phone number.
- **Fields:** the owner creates the metafield definitions from a checklist the build effort writes, imports the CSV, and fills the variant fields in the bulk editor.

## Start a new chat

Paste this into the new chat:

> Continue the FOSLN wayfinding map. Read `docs/handoff/wayfinder13.md` in Kintukka2/FOSLN-WATCH first, then run `/anthropic-skills:wayfinder https://github.com/Kintukka2/FOSLN-WATCH/issues/4`. The only open ticket is "Verify the specs against the samples" (#36). Read my answers from the checklist page at https://claude.ai/artifact/Jwg9pKE3AFziVqGE3EqCcH and resolve it.
