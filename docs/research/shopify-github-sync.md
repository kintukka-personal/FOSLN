# How Shopify's GitHub sync works with this repo

Research for [#6](https://github.com/Kintukka2/FOSLN-WATCH/issues/6), on the map [#4](https://github.com/Kintukka2/FOSLN-WATCH/issues/4). Researched 2026-10-09 against Shopify's and GitHub's own docs.

## Short answer

- **No terminal for the owner.** Every owner step is a click in Shopify admin or on GitHub's website. Git work and hard merge conflicts fall to Claude Code sessions.
- **The theme must sit at the root of the branch.** Subfolders aren't supported. The theme folders would sit next to `CLAUDE.md`, `GLOSSARY.md` and `docs/`, which Shopify ignores.
- **One connected branch makes one theme.** Each save in the theme editor becomes a `shopify` bot commit on that branch. A merge into the branch behind the published theme goes live at once.
- **Conflicts are handled loosely.** The theme editor warns before overwriting, and the code editor doesn't. Git conflicts get fixed in GitHub. **Reset to last commit** in admin re-pulls the branch.

---

## 1. What the integration is

Shopify's GitHub app links a GitHub account to a Shopify store and syncs theme code both ways. "The GitHub theme integration updates your theme in the Shopify admin whenever the connected branch is updated. It also commits changes made through the Shopify admin to the branch to ensure that the branch and theme in the Shopify admin always match." ([shopify.dev: GitHub integration](https://shopify.dev/docs/storefronts/themes/tools/github))

The commit-back can't be turned off: "Files are updated in GitHub whenever changes are made to a connected theme. This can't be disabled." ([same page](https://shopify.dev/docs/storefronts/themes/tools/github#how-it-works))

**The GitHub app.** You install it from [github.com/apps/shopify](https://github.com/apps/shopify). It asks for broad rights: read and write on Administration, Contents, Pull requests, Secrets, Actions and Workflows, among others. Shopify recommends granting them "on a per-repository basis" ([shopify.dev: The Shopify GitHub App](https://shopify.dev/docs/api/github-app#required-github-permissions)). The same app also serves Hydrogen (headless) storefronts, which probably explains why it asks for so much. An older Shopify changelog entry said so, but it no longer loads, so I couldn't confirm it. GitHub's install screen lets you choose **Only select repositories** and pick `FOSLN-WATCH` ([GitHub docs: installing a third-party app](https://docs.github.com/en/apps/using-github-apps/installing-a-github-app-from-a-third-party)). You can change that later under the app's **Repository access** setting ([GitHub docs: modifying installed apps](https://docs.github.com/en/apps/using-github-apps/reviewing-and-modifying-installed-github-apps#modifying-repository-access)).

**What this means for this repo.** The GitHub API reports `Kintukka2/FOSLN-WATCH` as a **private** repo owned by a **personal** (User) account, not an organization. Shopify's docs say:

- "Your Shopify account can only be connected to one GitHub account" ([GitHub App page](https://shopify.dev/docs/api/github-app#connect-shopify-and-github)). That's fine for one owner. Shopify recommends organization accounts for teams.
- "Personal repositories where you're a GitHub collaborator, but not the owner, aren't visible in the list of available repositories" ([Limitations](https://shopify.dev/docs/storefronts/themes/tools/github#limitations)). So whoever connects in Shopify admin must authorize GitHub as **Kintukka2**, the owner.
- If the account is later converted to an organization, the Shopify app has to be uninstalled and reinstalled ([GitHub App page: known limitations](https://shopify.dev/docs/api/github-app#troubleshooting-and-known-limitations)).
- The docs never say whether private repos are allowed. They describe repos "associated with your GitHub account" and don't restrict visibility. I found nothing saying private repos fail.

## 2. Layout: root or subfolder?

**At the root.** Shopify: "You can connect only branches that match the default Shopify theme folder structure… Folders in the repository that don't match the default theme structure are ignored." ([Repository structure](https://shopify.dev/docs/storefronts/themes/tools/github#repository-structure))

The default structure is these top-level folders ([shopify.dev: theme architecture](https://shopify.dev/docs/storefronts/themes/architecture#directory-structure-and-component-types)):

```
assets/  blocks/  config/  layout/  locales/  sections/  snippets/  templates/ (with customers/ and metaobject/)
```

"Subdirectories, other than the ones listed, aren't supported", so you can't nest `sections/pdp/…`. "Only a `layout` directory containing a `theme.liquid` file is required for the theme to be uploaded" (same page).

**No subfolder option exists.** Nothing in Shopify's docs lets you point the integration at a subfolder.

- On Shopify's developer forum in March 2025, a poster signing as Liam-Shopify called subfolder selection "a recorded feature request that our team are exploring" ([community.shopify.dev](https://community.shopify.dev/t/github-theme-integration-subdirectory/11446)).
- In November 2024 a poster signing as Luke-Shopify said "There are no current plans to add support for custom repository structures/monorepo support". The same thread says a branch with the theme in a subfolder fails with "Branch isn't a valid theme" ([community.shopify.dev](https://community.shopify.dev/t/github-integration-root-property/1667)). A user was still asking for the feature in June 2026.
- I read Shopify's developer changelog feed from 9 Oct 2024 to today. It has one GitHub-integration entry, about commit messages (section 4), and nothing about subfolders ([changelog feed](https://shopify.dev/changelog/feed.xml)).

Forum replies are not official docs, but they agree with the docs.

**Files that aren't theme files, at the root.** The docs say non-theme folders are "ignored". They say nothing either way about loose root files such as `CLAUDE.md`. Shopify's own theme repos keep non-theme files at the root next to the theme folders:

- [Horizon](https://github.com/Shopify/horizon) has `LICENSE.md`, `README.md` and `release-notes.md`.
- [Dawn](https://github.com/Shopify/dawn) has `.github/`, `.theme-check.yml`, `README.md` and others.

That strongly suggests root files are fine, but it is an inference, not a documented rule.

**One wording conflict to know about.** The older [version-control best-practices page](https://shopify.dev/docs/storefronts/themes/best-practices/version-control#managing-source-and-compiled-code) says "a branch that's connected to Shopify can't contain `src` and `dist` folders." The current integration page says non-theme folders "are ignored". I read the newer page as current, so `docs/` should be ignored. Check it the first time the branch is connected (section 7).

**What the repo would look like** with the theme on `main`:

```
CLAUDE.md          ← ignored by Shopify
GLOSSARY.md        ← ignored
README.md          ← ignored
docs/              ← ignored (not a theme folder)
assets/ blocks/ config/ layout/ locales/ sections/ snippets/ templates/   ← the theme
```

**If you want the theme separate from the planning docs,** Shopify's options all have costs:

| Option | Source | Owner needs a terminal? | Trade-off |
|---|---|---|---|
| Theme at the root of `main` in this repo | [Integration page](https://shopify.dev/docs/storefronts/themes/tools/github) | No | Simplest. Claude sessions see `CLAUDE.md` and the glossary next to the code. The Shopify app gets read/write on the whole repo, docs included. |
| A second repo just for the theme | Shopify suggests "multiple repositories or subtrees" ([How it works](https://shopify.dev/docs/storefronts/themes/tools/github#how-it-works)) | No | Limits Shopify's access to theme code. Two repos to keep in step, and theme sessions don't see this repo's `CLAUDE.md` unless it's copied. |
| A theme-only branch, made with `git subtree` | [Version-control page](https://shopify.dev/docs/storefronts/themes/best-practices/version-control#separate-source-code-and-compiled-code-using-branches-recommended) | No (Claude sessions run the git commands) | Built for themes that need a build step. More branches and more git work for every change. |
| Skip the integration. A GitHub Action runs `shopify theme push --path <subfolder>` | [CLI in CI/CD](https://shopify.dev/docs/storefronts/themes/tools/cli/ci-cd), [`theme push` flags](https://shopify.dev/docs/api/shopify-cli/theme/theme-push) | No. The owner makes a Theme Access password in admin ([Theme Access](https://shopify.dev/docs/storefronts/themes/tools/theme-access)) and adds it as a repo secret on GitHub's website ([GitHub docs: secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)) | The theme can live in a subfolder. But the sync is **one-way**: theme-editor changes stay in Shopify and are never committed. A later push can overwrite them unless the workflow pulls first or skips settings files. Shopify says "this method isn't compatible with the Shopify GitHub integration" ([version-control page](https://shopify.dev/docs/storefronts/themes/best-practices/version-control#manage-only-your-source-code-in-version-control)). Pushing to the live theme needs `--allow-live`. |

**Which way this points:** the theme at the root of this repo is the only option that keeps two-way sync with nothing extra to run. Choosing it belongs to [#21](https://github.com/Kintukka2/FOSLN-WATCH/issues/21).

## 3. Branches and themes

- **One connection makes one theme.** You connect a branch with **Add theme › Connect from GitHub**, and it "appears in your theme library" as a new theme. Its card shows the repo, the branch and the last commit time ([Step 1](https://shopify.dev/docs/storefronts/themes/tools/github#step-1-connect-a-theme-repo)). The documented flow only adds a new theme from a branch. It describes no way to attach a theme that's already in the library.
- **Disconnecting is permanent.** "You can't reconnect a branch to a theme after it has been disconnected. If you reconnect a branch, then it's added as a new theme." ([Branch management](https://shopify.dev/docs/storefronts/themes/tools/github#branch-management-strategies))
- **Publishing keeps the link.** "If an unpublished theme is connected to a branch and then published, then it maintains its connection to the branch." (same section)
- **Shopify's suggested pattern** is to connect `main` and publish it, so that "the published theme is always up to date with features as they are merged". Use other branches for things like sales, publish them for a while, then go back to `main` ([version-control page: Branch organization](https://shopify.dev/docs/storefronts/themes/best-practices/version-control#branch-organization)).
- **Merging is deploying.** If `main` is connected to the published theme, every merge into `main` updates the live store. No release step sits in between. To preview a Claude session's work first, connect its working branch as a second, unpublished theme and preview that. Each preview theme takes one library slot: 20 on the Basic, Grow and Advanced plans, 100 on Plus ([Help Center: adding themes](https://help.shopify.com/en/manual/online-store/themes/adding-themes)).
- **Not documented:** whether one branch can be connected to two themes at once.

## 4. How theme-editor changes commit back

From the [Commits by Shopify](https://shopify.dev/docs/storefronts/themes/tools/github#commits-by-shopify) section:

- **What triggers a commit:** any save by the owner, staff or a collaborator, from three places:
  - the theme editor
  - the code editor
  - theme apps installed in the Online Store
- **When:** "Changes are added as a commit to the connected branch when they are saved." Saves "within about 10 seconds of each other are batched into one commit."
- **Who it's from:** the `shopify` bot. Since **26 August 2026** the commit body also names the shop and the last person to edit ([changelog](https://shopify.dev/changelog/github-commits-now-name-the-last-theme-editor)):

  ```
  Update from Shopify for theme Dawn

  Committed from shop: Snowdevil
  Theme last edited by: Bob Bobsen
  ```

  This is recent. Older guides and forum posts say commits don't name the editor.

**Which files the theme editor writes.** Theme-editor customizations "are stored in setting files, which are part of the theme code". In practice that means three kinds of file:

- `config/settings_data.json`: "Any time that the value of [a setting] is changed in the theme editor, `settings_data.json` is updated" ([settings_data.json](https://shopify.dev/docs/storefronts/themes/architecture/config/settings-data-json))
- JSON templates in `templates/*.json`, which store each page's sections and their settings ([JSON templates](https://shopify.dev/docs/storefronts/themes/architecture/templates/json-templates))
- section-group files in `sections/*.json`, such as the header and footer ([section groups](https://shopify.dev/docs/storefronts/themes/architecture/section-groups))

The code editor can change any file.

**Shopify can also change files itself.** Since 13 January 2026, Shopify has automatically rewritten theme files that fail its stricter Liquid parsing ([changelog](https://shopify.dev/changelog/liquid-is-getting-faster-and-ready-to-evolve)). I *expect* such rewrites to show up as Shopify commits on a connected branch. The docs don't say so.

## 5. Conflicts

These rules are from Shopify's [Conflicts and error handling](https://shopify.dev/docs/storefronts/themes/tools/github#conflicts-and-error-handling):

| Situation | What happens |
|---|---|
| Someone edits a file in the **theme editor** while the same file changes in GitHub or the code editor | The user "is warned that they're overriding the new changes when they save." |
| Someone saves in the **code editor** | "There are currently no conflict alerts in the code editor. The version of the file in the code editor overwrites the GitHub version." |
| A conflict between commits or pushes made outside Shopify | "The developer has a chance to resolve it in GitHub or force push the change to overwrite the file in Shopify." |
| Someone saves in Shopify at the same moment a commit is pushed to the branch | "The commit coming from Shopify might be viewed as outdated and rejected by GitHub." |
| You suspect a sync error | Click **View logs** beside **Last saved** on the theme card. |
| The theme has drifted from the branch | **Actions › Reset to last commit** on the theme card pulls the branch again. |

**What this likely means here** (my reading, not Shopify's words):

- **The common conflict.** A Claude session works on a branch while the owner customizes the live theme. Each save commits to `main`, often touching `settings_data.json` or a JSON template. If the session's branch touches the same file, its pull request will conflict.
- **What GitHub's website can fix.** GitHub says: "You can resolve simple competing line change conflicts on GitHub. For other conflicts, use the command line." ([GitHub docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-on-github)). Large JSON files tend to give messy line conflicts. The cleaner fix is to ask a Claude session to rebase or merge `main` and push again. Under the no-terminal rule, that is a Claude job.
- **Force pushing** can't be done from GitHub's website, so that remedy is also a Claude-session job.
- **A rejected Shopify commit** leaves the change in the theme but not on the branch. **Reset to last commit** reloads the branch, so I'd expect it to *discard* that theme-only change. That's unverified, so check **View logs** before resetting.

**Undoing a change, all without a terminal:**

- On GitHub's website, a merged pull request has a **Revert** button. It opens a new pull request that undoes the change ([GitHub docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/reverting-a-pull-request)). Merging that revert into `main` updates the theme.
- In Shopify's code editor, the **Timeline** panel lets you **Revert contents** of a single file ([code editor](https://shopify.dev/docs/storefronts/themes/tools/code-editor#track-file-changes)). That revert is then committed back to the branch like any other save.

## 6. Protecting `main` (a GitHub plan issue)

`main` is unprotected today. GitHub's API says rulesets on this repo need GitHub Pro: "Upgrade to GitHub Pro or make this repository public to enable this feature." GitHub's plans page also lists protected branches for private repos under **GitHub Pro**, not Free ([GitHub's plans](https://docs.github.com/en/get-started/learning-about-github/githubs-plans)). So on today's plan, nothing technical stops a Claude session pushing straight to `main`, and so to the live store. Merging through a pull request is a convention, not a guard.

If protection is added later, there's a catch:

- GitHub: "Actors may only be added to bypass lists when the repository belongs to an organization" ([about protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)). [Rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets) can list GitHub Apps as bypass actors, but again they need Pro on a private repo.
- Shopify commits straight to the connected branch. If `main` required pull requests, Shopify's theme-editor commits could be rejected. **Unverified:** neither Shopify nor GitHub documents this case. It may also depend on whether the app's Administration permission counts as admin.

**Which way this points:** keep the Shopify-connected branch free of "require a pull request" rules, or move the repo to an organization so the Shopify app can be added as a bypass actor.

## 7. Where each step happens

| # | Step | Where | Who | Terminal? |
|---|---|---|---|---|
| 1 | Install the Shopify GitHub app with **Only select repositories → FOSLN-WATCH** | GitHub website ([github.com/apps/shopify](https://github.com/apps/shopify)) | Owner | No |
| 2 | Put the theme at the branch root (`layout/theme.liquid` at minimum) and commit it | The repo | Claude session | Yes, inside Claude's own environment |
| 3 | **Online Store › Themes › Add theme › Connect from GitHub**, then pick the account, repo and branch | Shopify admin | Owner (or staff with the **Themes** permission) | No |
| 4 | Test the link: change a setting in the theme editor, save, then click the branch name on the theme card and check for a `shopify` bot commit ([Step 2](https://shopify.dev/docs/storefronts/themes/tools/github#step-2-test-the-connection)) | Shopify admin, then GitHub website | Owner | No |
| 5 | Check that `docs/` and the root `.md` files were ignored: **View logs** on the theme card | Shopify admin | Owner | No |
| 6 | Publish the theme connected to `main` ([Step 3](https://shopify.dev/docs/storefronts/themes/tools/github#step-3-publish-the-theme)) | Shopify admin | Owner | No |
| 7 | Theme-editor and code-editor edits commit back on their own | Shopify admin | Owner | No |
| 8 | Code changes: the session pushes a working branch and opens a pull request | GitHub | Claude session | Yes, Claude's environment |
| 9 | Optional preview: connect the working branch as an unpublished theme and preview it | Shopify admin | Owner | No |
| 10 | Review and merge the pull request, which updates the live theme | GitHub website | Owner | No |
| 11 | Simple conflict: **Resolve conflicts** in the pull request. Hard conflict or force push: ask a Claude session | GitHub website / Claude session | Owner / Claude | No / Claude only |
| 12 | Out of sync: **View logs**, then **Actions › Reset to last commit** | Shopify admin | Owner | No |
| 13 | Roll back: **Revert** on the merged pull request, or **Revert contents** for one file in the code editor | GitHub website / Shopify admin | Owner | No |
| 14 | Disconnect the accounts: **Revoke** at github.com/settings/apps/authorizations ([GitHub App page](https://shopify.dev/docs/api/github-app#disconnect-shopify-and-github)) | GitHub website | Owner | No |

One documented quirk: on a new development store or client-transfer store, "you can't set up the GitHub app on the initial login after the store is created. Log out of your store and log in again before proceeding." ([GitHub App page](https://shopify.dev/docs/api/github-app#troubleshooting-and-known-limitations))

## 8. Unverified, or may have changed

- **Root files next to the theme** (`CLAUDE.md`, `GLOSSARY.md`, `README.md`). These are inferred to be fine from Shopify's own repos. The docs only say non-theme *folders* are ignored. Check at step 5.
- **The `src`/`dist` wording.** The older page says a connected branch "can't contain" them. The current page says unknown folders are "ignored". I assume the current page wins.
- **Subfolder support.** Absent as of today (docs, forum, changelog to 9 Oct 2026). Shopify called it a feature request in 2025, so it could ship later.
- **A 50 MB branch limit.** A secondary search summary mentioned one. It is not in the current docs, and I couldn't check an archived copy. Treat it as unknown. A theme with a few product photos in `assets/` is unlikely to come near it, but large images or video would.
- **Protected branches against Shopify's commits** (section 6). Not documented.
- **Whether Reset to last commit discards theme-only changes** (section 5). This is my inference.
- **Shopify's January 2026 automatic Liquid rewrites arriving as Shopify commits.** Inferred, not documented.
- **Private repos.** Not stated either way. No sign they're excluded.
- **Plan names.** The Help Center's "Basic, Grow, and Advanced" plans and the 20-theme limit are as of today. Shopify renames plans from time to time.
- **Forum replies** on community.shopify.dev are from posters signing as Shopify staff, but they aren't official docs.

## Sources

Shopify:

- [Shopify GitHub integration for themes](https://shopify.dev/docs/storefronts/themes/tools/github): the main source
- [The Shopify GitHub App](https://shopify.dev/docs/api/github-app): permissions, account rules, disconnecting
- [Version control for Shopify themes](https://shopify.dev/docs/storefronts/themes/best-practices/version-control)
- [Theme architecture: directory structure](https://shopify.dev/docs/storefronts/themes/architecture#directory-structure-and-component-types)
- [settings_data.json](https://shopify.dev/docs/storefronts/themes/architecture/config/settings-data-json), [JSON templates](https://shopify.dev/docs/storefronts/themes/architecture/templates/json-templates), [Section groups](https://shopify.dev/docs/storefronts/themes/architecture/section-groups)
- [Code editor](https://shopify.dev/docs/storefronts/themes/tools/code-editor)
- [Shopify CLI in a CI/CD pipeline](https://shopify.dev/docs/storefronts/themes/tools/cli/ci-cd), [`shopify theme push`](https://shopify.dev/docs/api/shopify-cli/theme/theme-push), [Theme Access](https://shopify.dev/docs/storefronts/themes/tools/theme-access)
- Changelog: [GitHub commits now name the last theme editor](https://shopify.dev/changelog/github-commits-now-name-the-last-theme-editor) (26 Aug 2026), [Stricter Liquid parsing](https://shopify.dev/changelog/liquid-is-getting-faster-and-ready-to-evolve) (13 Jan 2026), [feed](https://shopify.dev/changelog/feed.xml)
- [Help Center: Adding themes](https://help.shopify.com/en/manual/online-store/themes/adding-themes)
- Developer forum: [subdirectory request](https://community.shopify.dev/t/github-theme-integration-subdirectory/11446), [custom root / monorepo](https://community.shopify.dev/t/github-integration-root-property/1667)
- Example repos: [Shopify/horizon](https://github.com/Shopify/horizon), [Shopify/dawn](https://github.com/Shopify/dawn)

GitHub:

- [Installing a GitHub App from a third party](https://docs.github.com/en/apps/using-github-apps/installing-a-github-app-from-a-third-party), [Modifying installed apps](https://docs.github.com/en/apps/using-github-apps/reviewing-and-modifying-installed-github-apps)
- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches), [About rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets), [GitHub's plans](https://docs.github.com/en/get-started/learning-about-github/githubs-plans)
- [Resolving a merge conflict on GitHub](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-on-github), [Reverting a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/reverting-a-pull-request)
- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
- This repo's own settings, read through the GitHub API on 2026-10-09: private, owned by a personal account, `main` unprotected, rulesets unavailable without Pro.
