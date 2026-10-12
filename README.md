# Sprint 2 — GitHub Issue Triage

Current public repository: https://github.com/JaiMayrie/-n8n-sprint2-issue-triage

Course: CS 590 Low Code No Code for Rapid Prototyping  
Assignment: Sprint 2, Week 6  
Build session documented: October 10, 2026

## Repository preparation status

The current Sprint 2 documentation in this repository is `README.md`, `runbook.md`, and `test_evidence.md`.

The actual supplied n8n workflow export is now included as [sprint2_github_issue_triage.json](sprint2_github_issue_triage.json). Its JSON syntax was checked, and no exposed password, token, configured credential, or pinned payload was found in the review. All exported configuration values and connections were preserved; only JSON whitespace was compacted. This is a configuration artifact, not proof of successful execution.

All 16 configuration screenshots are uploaded in `evidence/`, with filenames matching the evidence links below. See the [screenshot inventory](evidence/README.md). These images document the configured build, not successful runtime tests.

Remaining repository-name cleanup: correct `-n8n-sprint2-issue-triage` to the course-required `n8n-sprint2-issue-triage` by removing the leading dash. The current repository is public, but the name does not yet match the handout.

## Current status: configuration built; execution blocked

I assembled the workflow and configured its route-setting, ledger, route-log, and failure-handling nodes. During my execution attempts, n8n does not progress beyond the first node. I have already communicated with my instructor about this execution issue.

This submission documents the configured build and the current limitation. I cannot verify end-to-end event delivery, successful table writes, duplicate suppression, the opened-action gate, or the intentional-failure result. Those runtime checks are marked **Blocked / not verified**, not passed. The cause of the execution problem has not been established. GitHub CLI authentication is confirmed, but that does not establish that webhook forwarding or n8n processing succeeded.

See [test_evidence.md](test_evidence.md) for the evidence inventory, static export observations, and test status. Configuration screenshots and the uploaded JSON show settings and connections, not successful executions.

## Purpose

The intended workflow receives newly opened GitHub issues, classifies each title without an LLM, saves a durable issue ledger record, and writes a category-specific internal route-log record. Repeated delivery of an already recorded issue should update only its last-seen timestamp. A route-log error should preserve the ledger row and mark its route-log status as failed. This is the behavior required by the course tutorial, not a claim that execution has been demonstrated. [Course tutorial, pp. 1–2, 12–16.]

## Services and components

The course design uses a public GitHub test repository, GitHub CLI and its `cli/gh-webhook` extension, locally hosted n8n, and two n8n Data Tables. The local editor is at `http://localhost:5678`. Docker is the local hosting environment used for this build. No external notification destination or AI classification service is part of the assignment. [Course tutorial, pp. 3–5, 9, 12.]

## Node sequence and branches

The uploaded export and latest canvas show the following structure. It is a branching workflow, not a list in which every node runs for every issue. The classification settings have the discrepancies noted below.

1. `GitHub issue webhook` receives the event.
2. `Opened action?` should admit only `body.action = opened`; the false branch stops.
3. `Normalize issue` prepares the shared issue fields.
4. `Classify title` is intended to select a category.
5. One category-setting node supplies its fixed values: `Set Bug Route`, `Set feature route`, `Set Help Route`, or `Set other route`.
6. Every category-setting node connects to the same two checks:
   - `If ledger row exists` → `Update existing last seen` → stop.
   - `If ledger row does not exist` → `Insert pending ledger row` → `Intentional failure test?`.
7. On the false output of `Intentional failure test?`, `Record internal route` writes to the route-log table.
8. `Record internal route` Success → `Update route-log recorded`.
9. `Record internal route` Error → `Update route-log failed`.
10. On the true output of `Intentional failure test?`, `Simulate route-log failure` throws the deliberate course-test error. Its Error output also connects to `Update route-log failed`.

Evidence: [workflow export](sprint2_github_issue_triage.json) and [latest routing canvas](evidence/01_workflow_canvas.png). The beginning of the workflow is outside the left edge of this canvas capture. Earlier field-configuration screenshots should be read together with the exported connection layout.

## Classification rules

The course rules use `title_lower` and the following order. These are the required rule definitions, not a claim that the saved Switch fully implements them or that matching has been verified. [Course tutorial, pp. 11–12.]

| Order | Match intent | Category | Route name |
| --- | --- | --- | --- |
| 1 | Whole word `bug` or `error` | `bug` | `bug-triage` |
| 2 | Whole word `feature` or `request` | `feature` | `feature-requests` |
| 3 | Whole word `help` or `question` | `help` | `support` |
| 4 | Fallback when the earlier rules do not match | `other` | `general-triage` |

The tutorial prints the three regex match values as follows; this transcription preserves its backslashes.

```text
\\b(bug|error)\\b
\\b(feature|request)\\b
\\b(help|question)\\b
```

**Saved Switch differences identified by static export review:** Rule 1 uses `regex`; Rules 2 and 3 use `equals`, rather than the required regex operation. Rule 4 uses `notRegex` with an empty match value, and the Switch options do not configure a fallback output. The first three saved match strings contain two literal backslashes before each `b` after JSON decoding; their regex escaping also needs review. These settings were preserved in the uploaded artifact, not silently corrected. They are configuration findings, not a confirmed explanation of the reported first-node blockage.

All four category-setting nodes in the supplied export use separate String fields named `category` and `route_name`, and all four have `includeOtherFields: true`. This confirms the Feature field-retention setting that was not visible in the earlier final screenshots. It does not establish a successful classification result.

## Field mappings

### Normalization: course-specified mappings

These are the expected payload paths from the tutorial. The export contains these expressions; its `repository_full_name` value additionally has a trailing space and `issue_number` is configured as a String. A received execution payload is still needed to verify behavior against this installation. [Course tutorial, pp. 10–11.]

| Field | Expression |
| --- | --- |
| `repository_full_name` | `{{ $json.body.repository.full_name }}` |
| `issue_number` | `{{ $json.body.issue.number }}` |
| `issue_key` | `{{ $json.body.repository.full_name + "#" + $json.body.issue.number }}` |
| `issue_url` | `{{ $json.body.issue.html_url }}` |
| `title` | `{{ $json.body.issue.title }}` |
| `title_lower` | `{{ $json.body.issue.title.toLowerCase() }}` |
| `reporter` | `{{ $json.body.issue.user.login }}` |
| `opened_at` | `{{ $json.body.issue.created_at }}` |
| `last_seen_at` | `{{ $now.toISO() }}` |

### `github_issue_ledger`: configured insert mapping

| Column | Value in the export |
| --- | --- |
| `issue_key` | `{{ $json.issue_key }}` |
| `issue_url` | `{{ $json.issue_url }}` |
| `title` | `{{ $json.title }}` |
| `category` | `{{ $json.category }}` |
| `reporter` | `{{ $json.reporter }}` |
| `opened_at` | `{{ $json.opened_at }}` |
| `last_seen_at` | `{{ $json.last_seen_at }}` |
| `route_log_status` | Fixed text: `pending` |
| `route_name` | `{{ $json.route_name }}` |
| `error_note` | Present in the mapping schema, but no explicit value is serialized in `columns.value` |

The tutorial requires a fixed blank `error_note`. A blank-looking editor box does not establish from this export alone whether an empty string or another blank/null state is written. That remains unverified.

Evidence: [workflow export](sprint2_github_issue_triage.json), [upper insert fields](evidence/07_ledger_insert_upper.png), and [lower insert fields](evidence/08_ledger_insert_lower.png). Course reference: pp. 13–14.

### `github_issue_route_log`: configured insert mapping

| Column | Value |
| --- | --- |
| `issue_key` | `{{ $json.issue_key }}` |
| `category` | `{{ $json.category }}` |
| `route_name` | `{{ $json.route_name }}` |
| `recorded_at` | `{{ $json.last_seen_at }}` |

Evidence: [route-log mapping](evidence/10_route_log_mapping.png) and the uploaded export. This screenshot was taken before the node was renamed from `Insert row` to `Record internal route`. Course reference: p. 15.

### Update operations

The existing-delivery update matches `issue_key` to `{{ $json.issue_key }}` and changes only `last_seen_at` to `{{ $json.last_seen_at }}`. The two route-result updates match `issue_key` to `{{ $('Insert pending ledger row').item.json.issue_key }}`. The success update explicitly sets `route_log_status` to `recorded`. Its `error_note` remains in the schema but is absent from `columns.value`, so clearing a prior note is not established. The tutorial requires clearing that note on success. The failure update explicitly sets the status to `failed` and uses this fixed safe message:

```text
Route-log write failed; review the route-log node configuration and execution details.
```

Evidence: [timestamp update](evidence/05_existing_timestamp_update.png), [recorded fields](evidence/12_recorded_status_update.png), [failed fields](evidence/15_failed_status_update.png), and the uploaded export. Course reference: pp. 13, 15–16.

## Intentional failure configuration

The configured IF expression is `{{ $json.title.toLowerCase() }}` with `contains` and the fixed phrase `intentional route-log failure test`. This differs from the tutorial's reference to `title_lower`: the build lowercases the ledger's saved `title` directly. The implementation choice is documented here and has not been runtime-verified.

The Code node contains:

```javascript
throw new Error('Intentional course test: route-log failure');
```

Both the Code node and the route-log insert use **Continue (using error output)**. Merely configuring this branch is not evidence that a failed ledger record was created.

## Setup and operating notes

The intended setup is the one in the course tutorial: create the public test repository; create the two named Data Tables with their required text columns; configure the POST Webhook path `sprint2-issue-triage`; activate/publish the workflow; and start GitHub CLI forwarding. [Course tutorial, pp. 5–16.]

The prescribed forwarding command, adapted to the submission repository, is:

```bash
gh webhook forward --repo=JaiMayrie/-n8n-sprint2-issue-triage --events=issues --url=http://localhost:5678/webhook/sprint2-issue-triage
```

This command uses the current repository name. After removing its leading dash, change the `--repo` value to `JaiMayrie/n8n-sprint2-issue-triage`. It is documented as a setup step, not as a successfully completed action. The confirmed preparation result is an active GitHub CLI login: [cropped login status](evidence/16_github_login_status.png). No live forwarding result is included.

The export records `active: true`; this is a saved setting, not proof that the local webhook is currently reachable. Data Table selections reference this installation's table identifiers. A reviewer opening the export in another installation may need to select their own corresponding tables; the export does not contain table rows or runtime evidence.

See [runbook.md](runbook.md) for intended operation, recovery, and shutdown. Use only the synthetic course test issues. Do not manually populate tables to imitate successful output. Keep the original export private, redact credentials and secrets from future submission copies, and do not include `.env` files. [Course tutorial, pp. 9, 17–22, 25.]

## Sources

Course source: *Sprint 2 Tutorial: GitHub Issue Triage Automation*, supplied course document, due October 11, 2026. Page references above refer to that 25-page handout. Evidence consists of the student's supplied October 10 configuration screenshots and the actual uploaded [workflow export](sprint2_github_issue_triage.json). Static inspection is not a runtime test.
