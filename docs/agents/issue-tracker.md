# Issue tracker: GitHub

Issues for this repo live in GitHub Issues at [Kintukka2/FOSLN-WATCH](https://github.com/Kintukka2/FOSLN-WATCH/issues). Use the `gh` CLI for every operation. Inside the clone, `gh` infers the repo from the git remote; outside it, add `--repo Kintukka2/FOSLN-WATCH`.

Where `gh` isn't available, use the GitHub MCP tools for the same operations.

In Claude Code cloud sessions, GitHub's GraphQL API is blocked. The `gh issue …` and `gh label …` subcommands depend on it, so they fail there. Use the REST equivalents through `gh api`:

| Operation | REST call |
| --- | --- |
| Create | `gh api repos/{owner}/{repo}/issues --input body.json` (JSON with `title`, `body`, `labels`) |
| Read | `gh api repos/{owner}/{repo}/issues/<number>`, plus `…/<number>/comments` |
| Comment | `gh api repos/{owner}/{repo}/issues/<number>/comments -F body=@comment.md` |
| Labels | `gh api repos/{owner}/{repo}/issues/<number>/labels -f 'labels[]=<label>'`; create one with `gh api repos/{owner}/{repo}/labels -f name=<label>` |
| Assign | `gh api repos/{owner}/{repo}/issues/<number>/assignees -f 'assignees[]=<login>'` |
| Close | `gh api -X PATCH repos/{owner}/{repo}/issues/<number> -f state=closed -f state_reason=completed` (or `not_planned`) |

The wayfinding `gh api` calls below are already REST, so they work everywhere.

## Settings

- **PRs as a request surface**: off. Only issues count as requests; pull requests are not pulled into triage. Set to `on` to treat open PRs as requests too.

## Operations

- **Create an issue**: `gh issue create --title "<title>" --body-file -`, with the body in a heredoc:

  ```sh
  gh issue create --title "<title>" --body-file - <<'BODY'
  <body>
  BODY
  ```

- **Read an issue**: `gh issue view <number> --comments`
- **List issues**: `gh issue list --state open --json number,title,labels,assignees --limit 100`. Narrow with `--label`, `--assignee` or `--search`.
- **Comment**: `gh issue comment <number> --body-file -` (heredoc as above)
- **Add / remove labels**: `gh issue edit <number> --add-label "<label>"` / `--remove-label "<label>"`. A label must exist before it can be applied; create it the first time with `gh label create "<label>"`.
- **Assign**: `gh issue edit <number> --add-assignee @me`
- **Close**: `gh issue close <number> --comment "<why>"`. Add `--reason "not planned"` for won't-do.

## When a skill says…

- **"publish to the issue tracker"**: create a GitHub issue.
- **"fetch the ticket"**: `gh issue view <number> --comments`.

## Wayfinding operations

How the `wayfinder` skill's map, tickets, blocking and frontier live on GitHub.

GitHub's sub-issue and dependency endpoints take an issue's numeric **id**, not its `#number`. Look it up with:

```sh
gh api repos/{owner}/{repo}/issues/<number> --jq .id
```

`gh api` fills in `{owner}` and `{repo}` from the git remote.

- **Map**: one issue labelled `wayfinder:map`. Find maps with `gh issue list --label wayfinder:map --state all`.
- **Ticket**: a sub-issue of the map, labelled `wayfinder:<type>` (`research`, `prototype`, `grilling` or `task`). Create the issue, then attach it to the map:

  ```sh
  gh api repos/{owner}/{repo}/issues/<map-number>/sub_issues -F sub_issue_id=<ticket-id>
  ```

- **List a map's tickets**:

  ```sh
  gh api repos/{owner}/{repo}/issues/<map-number>/sub_issues --paginate \
    --jq '.[] | {number, title, state, assignees: [.assignees[].login]}'
  ```

- **Blocking**: GitHub's native "blocked by" relationship. Mark a ticket as blocked by another:

  ```sh
  gh api repos/{owner}/{repo}/issues/<blocked-number>/dependencies/blocked_by -F issue_id=<blocker-id>
  ```

  List a ticket's blockers:

  ```sh
  gh api repos/{owner}/{repo}/issues/<number>/dependencies/blocked_by --jq '.[] | {number, state}'
  ```

- **Claim**: `gh issue edit <number> --add-assignee @me`. An open ticket with no assignee is unclaimed.
- **Frontier**: the map's sub-issues that are open, unassigned, and have no open blocker. List the map's tickets, keep the open unassigned ones, then drop any whose `blocked_by` list holds an open issue.
- **Resolve**: post the answer with `gh issue comment <number>`, then `gh issue close <number>`.
- **Rule out of scope**: `gh issue close <number> --reason "not planned" --comment "<why it's out of scope>"`.
