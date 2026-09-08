# Issue tracker: Linear

Issues and specs for this repo live as Linear issues on the **Victor's Personal** team (`VL`), in the **Omapad** project.

Use the Linear MCP for all operations.
Do not use `gh issue` for this repo's work items.

| Field | Value |
| --- | --- |
| Team name | Victor's Personal |
| Team id | `66d9264a-ce2c-4dea-860e-772c9ebe40ad` |
| Team key / issue prefix | `VL` (identifiers look like `VL-123`) |
| Project name | Omapad |
| Project id | `d4e9d4ca-d18b-4d9d-838f-5510057c4cfb` |
| Project URL | https://linear.app/quietengines/project/omapad-42c75976ccf9 |

## Scope

`project`. Issues and `/initiative` updates live on **Omapad**.

Always scope create / list / filter to this team + project.
Prefer project **id** over name.
Do not re-resolve these by name search.

## Conventions

Always set `team` to `Victor's Personal` (or `VL`) and `project` to `d4e9d4ca-d18b-4d9d-838f-5510057c4cfb` when creating an issue.

- **Create an issue**: `linear__save_issue` with `title`, `team: "Victor's Personal"`, `project: "d4e9d4ca-d18b-4d9d-838f-5510057c4cfb"`, and `description`. Add labels via `labels` (e.g. `["needs-triage"]`).
- **Read an issue**: `linear__get_issue` with the identifier (`VL-123`) and `includeRelations: true`. Then `linear__list_comments` with `issueId: "VL-123"`.
- **List issues**: `linear__list_issues` with `project: "d4e9d4ca-d18b-4d9d-838f-5510057c4cfb"` (and `team: "Victor's Personal"` if you need to disambiguate). Filter with `label`, `state`, `assignee`, or `parentId`.
- **Comment on an issue**: `linear__save_comment` with `issueId` and `body`.
- **Apply / remove labels**: `linear__save_issue` with `id` and `labels`. `labels` **replaces** the full set. Read the issue first and send the complete list, or existing labels are dropped.
- **Close**: `linear__save_issue` with `id` and `state: "Done"`. Use `Canceled` for wontfix / out-of-scope; `Duplicate` when it's a dupe.

Workflow states on this team: `Backlog`, `Todo`, `In Progress`, `In Review`, `Done`, `Canceled`, `Duplicate`.

Issue ids are always `VL-{number}`.
Bare `#42` in chat for this repo means Linear `VL-42`, not a GitHub issue.

## Close-out git

**Default branch, no PR.** Smash commits on the current branch. Do not create a feature branch or pull request.

## Pull requests as a triage surface

GitHub PRs are not a triage surface for this repo.

## When a skill says "publish to the issue tracker"

Create a Linear issue on team Victor's Personal in project Omapad (d4e9d4ca-d18b-4d9d-838f-5510057c4cfb).

## When a skill says "fetch the relevant ticket"

Run `linear__get_issue` on the `VL-N` identifier (with `includeRelations: true`) and `linear__list_comments` for the thread.

## Wayfinding operations

Used by `/wayfinder`.
The **map** is a single Linear issue with **child** issues as tickets.

- **Map**: a single issue labelled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body. Create with `linear__save_issue` (`team: "Victor's Personal"`, `project: "d4e9d4ca-d18b-4d9d-838f-5510057c4cfb"`, `labels: ["wayfinder:map"]`).
- **Child ticket**: `linear__save_issue` with `parentId` set to the map's identifier. Labels: `wayfinder:<type>` (`research` / `prototype` / `grilling` / `task`). Once claimed, the ticket is assigned to the driving dev.
- **Blocking**: Linear's native **blocked by** relation. Add an edge with `linear__save_issue` `id: <child>` and `blockedBy: ["VL-N"]` (append-only). A ticket is unblocked when every blocker is in a completed or canceled state. Remove a stale edge with `removeBlockedBy`.
- **Frontier query**: `linear__list_issues` with `parentId` = the map, open states only (`Backlog` / `Todo` / `In Progress` / `In Review`). Drop any with an open blocker (`linear__get_issue` + `includeRelations: true`) or an assignee; first in map/child order wins.
- **Claim**: `linear__save_issue` with `id` and `assignee: "me"`. The session's first write.
- **Resolve**: `linear__save_comment` with the answer, then `linear__save_issue` with `state: "Done"`, then append a context pointer (gist + link) to the map's Decisions-so-far (`patch` on the map issue).
