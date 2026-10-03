# Issue tracker: GitHub

Issues and specs for this repo live as GitHub issues on `artsaraiva/chasepull`.

## Account and tooling

Use the **GitHub MCP server** (`mcp__plugin_github_github__*` tools). It is authenticated as the personal `artsaraiva` account. Always pass `owner: "artsaraiva"`, `repo: "chasepull"`.

This machine's active `gh` CLI account is a work account. Use `gh` only for operations the MCP server lacks (native issue dependencies), and always run it as the personal account by prefixing the call:

    GH_TOKEN=$(gh auth token --user artsaraiva) gh api ...

Never create issues, labels, comments, or PRs from any other account.

## Conventions

- **Create an issue**: `issue_write` with `method: "create"`, `title`, `body`, `labels`. Labels that don't exist yet are created by GitHub on first use.
- **Read an issue**: `issue_read` with `method: "get"`, then `method: "get_comments"` for the conversation.
- **List issues**: `list_issues` with `state` and `labels` filters; pass `fields` to trim the response.
- **Comment on an issue**: `add_issue_comment`.
- **Apply / remove labels**: `issue_write` with `method: "update"` and the full desired `labels` list (it replaces the set, so read the current labels first).
- **Close**: `add_issue_comment` with the reason, then `issue_write` with `method: "update"`, `state: "closed"`, `state_reason`.

## Pull requests as a triage surface

**PRs as a request surface: no.** _(Set to `yes` if this repo treats external PRs as feature requests; `/triage` reads this flag.)_

When set to `yes`, PRs run through the same labels and states as issues:

- **Read a PR**: `pull_request_read` (`get`, `get_diff`, `get_comments`).
- **List external PRs for triage**: `list_pull_requests` with `state: "open"`, then keep only authors whose association is `CONTRIBUTOR`, `FIRST_TIME_CONTRIBUTOR`, or `NONE` (drop `OWNER`/`MEMBER`/`COLLABORATOR`).
- **Comment / label / close**: `add_issue_comment` and `issue_write` with the PR number (GitHub treats PRs as issues for these).

GitHub shares one number space across issues and PRs, so a bare `#42` may be either: try `pull_request_read` and fall back to `issue_read`.

## When a skill says "publish to the issue tracker"

Create a GitHub issue with `issue_write` (`method: "create"`).

## When a skill says "fetch the relevant ticket"

`issue_read` with `method: "get"` and `method: "get_comments"`.

## Blocking edges

Use GitHub's **native issue dependencies** (UI-visible). The MCP server can't write them, so use `gh` as the personal account:

    GH_TOKEN=$(gh auth token --user artsaraiva) gh api --method POST \
      repos/artsaraiva/chasepull/issues/<blocked>/dependencies/blocked_by -F issue_id=<blocker-db-id>

`<blocker-db-id>` is the blocker's numeric **database id** (the `id` field from `issue_read`, _not_ the `#number` or `node_id`). Read open blockers with `GH_TOKEN=... gh api repos/artsaraiva/chasepull/issues/<n> --jq .issue_dependencies_summary.blocked_by`. If dependencies are unavailable, fall back to a `Blocked by: #<n>, #<n>` line at the top of the issue body. A ticket is unblocked when every blocker is closed.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a single issue with **child** issues as tickets.

- **Map**: a single issue labelled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body. `issue_write` create with `labels: ["wayfinder:map"]`.
- **Child ticket**: `issue_write` create with `parent_issue_number: <map>` (creates it as a sub-issue in one call). Labels: `wayfinder:<type>` (`research`/`prototype`/`grilling`/`task`). Once claimed, the ticket is assigned to the driving dev.
- **Blocking**: see "Blocking edges" above.
- **Frontier query**: `issue_read` with `method: "get_sub_issues"` on the map, keep open children, drop any with an open blocker or an assignee; first in map order wins.
- **Claim**: `issue_write` update with `assignees: ["artsaraiva"]`, the session's first write.
- **Resolve**: `add_issue_comment` with the answer, then `issue_write` update `state: "closed"`, then append a context pointer (gist + link) to the map's Decisions-so-far via `issue_write` update of the map body.
