# Sprint 2 test evidence

## Repository upload note

The actual n8n workflow export is uploaded as [sprint2_github_issue_triage.json](sprint2_github_issue_triage.json). All 16 PNG files described below are uploaded in the repository's `evidence` folder, with filenames matching the links in this document. See [evidence/README.md](evidence/README.md) for the image inventory. Upload completion does not change the blocked/not-verified status of the runtime tests.

## Submission status

**Configuration documented; runtime verification blocked.** During my execution attempts, n8n does not move beyond the first node. I have already communicated with my instructor about this issue. The root cause has not been established.

The uploaded screenshots and supplied workflow JSON show configured nodes, field mappings, connection layouts, and GitHub CLI preparation. They do not show successful execution or populated table results. No runtime test below is marked passed. A node configured to write `failed` is not evidence that the deliberate-failure test ran.

## Static review of the supplied export

This review inspected the JSON file; it did not execute n8n, receive a GitHub event, or write a table row. All configuration values were preserved in the uploaded copy. Only JSON whitespace was compacted.

Confirmed in the file:
- Valid JSON with 17 nodes and the saved connections.
- All four category-setting nodes have `includeOtherFields: true`, including Feature.
- The Webhook uses POST with path `sprint2-issue-triage`; the opened-action gate compares `body.action` with `opened`.
- All four category-setting nodes connect to the same two ledger checks. The recorded and failed update paths are present.
- No configured credentials, exposed passwords/tokens, or pinned payload data were found in the review.

Configuration items that remain unresolved:
- In `Classify title`, Feature and Help use `equals`, not the regex operation required in the tutorial.
- The fourth Switch rule is `notRegex` with an empty match value; the Switch options do not define the required fallback output.
- After JSON decoding, the first three match strings contain two literal backslashes before each `b`; regex escaping needs review. The earlier documentation transcribed the handout's printed backslashes, not a successfully tested pattern.
- The pending insert and recorded-status update include `error_note` in their schemas but omit it from `columns.value`. The export alone does not establish that the required empty value is written or a prior error is cleared.
- `If ledger row exists` does not explicitly serialize `matchType`, although the tutorial specifies All Conditions. It contains only one condition. The other ledger check and Update nodes explicitly store All Conditions.

These observations identify saved settings that differ from or need clarification against the tutorial. They do not establish the cause of the separately reported first-node blockage. No fixes were silently applied to the submitted workflow. Reference: uploaded JSON, particularly `Classify title`, the ledger nodes, and `connections`; course tutorial, pp. 11–16.

## Uploaded configuration evidence

The original screenshots were captured during the October 10 build session. The uploaded copies are cropped to remove unrelated browser and desktop areas. No execution output or table data has been added to them. Some field screenshots precede later node renaming or connection corrections; the uploaded JSON is the reference for the supplied final configuration.

| File | What it supports | What it does not establish |
| --- | --- | --- |
| [01_workflow_canvas.png](evidence/01_workflow_canvas.png) | Shared ledger checks and visible routing/error branch layout | Successful execution; leftmost start of workflow is outside this capture |
| [02_bug_fields.png](evidence/02_bug_fields.png) | Separate category/route fields; other input fields retained | Actual Bug classification |
| [03_help_fields.png](evidence/03_help_fields.png) | Help route field settings | Actual Help classification |
| [04_other_fields.png](evidence/04_other_fields.png) | Other route field settings | Actual fallback classification |
| [05_existing_timestamp_update.png](evidence/05_existing_timestamp_update.png) | Only the timestamp is mapped for the existing-row update | Duplicate suppression |
| [06_new_issue_check.png](evidence/06_new_issue_check.png) | New-row check uses the ledger and issue key | A received issue passing the check |
| [07_ledger_insert_upper.png](evidence/07_ledger_insert_upper.png) and [08_ledger_insert_lower.png](evidence/08_ledger_insert_lower.png) | Pending-ledger insert mappings | Creation of a ledger record |
| [09_intentional_failure_condition.png](evidence/09_intentional_failure_condition.png) | Configured intentional-test phrase check | Execution of that branch |
| [10_route_log_mapping.png](evidence/10_route_log_mapping.png) | Four mappings in the route-log table | A saved route-log record |
| [11_route_log_error_setting.png](evidence/11_route_log_error_setting.png) | Continue using error output is selected | An error actually handled |
| [12_recorded_status_update.png](evidence/12_recorded_status_update.png) | Recorded status and an empty error-note editor box | Successful status transition or actual error-note clearing |
| [13_intentional_failure_code.png](evidence/13_intentional_failure_code.png) and [14_code_error_setting.png](evidence/14_code_error_setting.png) | Deliberate error code and error-output configuration | Deliberate failure executed |
| [15_failed_status_update.png](evidence/15_failed_status_update.png) | Failed status and safe message mapping | Retention of a failed ledger row |
| [16_github_login_status.png](evidence/16_github_login_status.png) | GitHub CLI logged in with an active account | Live webhook forwarding or n8n event receipt |

## Required runtime tests

Expected results below come from the tutorial, pp. 18–21. They are acceptance targets, not observed output.

| Test | Course input/action | Expected result | Actual result/status |
| --- | --- | --- | --- |
| 1: Bug | `Bug: blank screen after logout` | `bug`; one ledger row marked `recorded`; one `bug-triage` route entry | **Blocked / not verified** |
| 2: Feature | `Feature request: export task list to CSV` | `feature`; one ledger row marked `recorded`; one `feature-requests` entry | **Blocked / not verified** |
| 3: Help | `Help: change the public webhook address` | `help`; one ledger row marked `recorded`; one `support` entry | **Blocked / not verified** |
| 4: Other | `Documentation: clarify local setup` | `other`; one ledger row marked `recorded`; one `general-triage` entry | **Blocked / not verified** |
| 5: Duplicate delivery | Redeliver the original successful Test 1 delivery, without creating another issue | Same issue key retains one ledger row and one route-log row; last-seen time may change | **Blocked / not verified**; original successful run unavailable |
| 6: Opened-action gate | Edit Test 4's body by appending `Edited only to test that non-opened issue actions do not enter triage.` | No new ledger or route-log record | **Blocked / not verified** |
| 7: Deliberate route-log failure | `Documentation: intentional route-log failure test` | One ledger row marked `failed`, readable error note, and no route-log row for that key | **Blocked / not verified** |

No successful execution IDs, verified test issue URLs, before/after table counts, or populated-result screenshots are available in this submission record. Their absence is disclosed rather than replaced with example results. A public test issue by itself would not establish that n8n processed it.

## Blocker record

**Observed symptom reported:** The workflow does not progress beyond its first node during execution attempts.

**Known preparation:** The editor is accessible; the node configuration and connection work is documented; GitHub CLI installation and active login were checked. The supplied screenshots and JSON do not establish that a forwarding command successfully delivered an event.

**Configuration work completed while preparing the build:** Removed duplicate route/check nodes; separated field names from values; corrected the route-log table selection; limited update mappings to intended columns; corrected the success/error connections; and configured the intentional-failure branch. These changes are build work, not proof that they resolved the runtime problem. The static export findings above record settings that still need review.

**Diagnosis status:** Unconfirmed. No exact runtime error message or successful received payload is available here. I am not attributing the first-node blocker to a specific component or cause without evidence.

**Instructor communication:** I have already communicated about the execution issue. Private correspondence is not included in this public repository.

**Next evidence to add, when available:** A genuine screenshot of the current blocked state or relevant execution message, cropped to remove private information. It may be named `evidence/execution_blocked.png`. That image is not included among the current configuration captures. After resolution, replace blocked statuses only with actual execution and table-result evidence.

## Export and publication checks

The included [sprint2_github_issue_triage.json](sprint2_github_issue_triage.json) was prepared from the supplied file `Sprint 2 — GitHub Issue Triage.json`, not reconstructed from screenshots. It was checked for credentials, tokens, sensitive values, and pinned payload data before upload. None was found. All configuration values and connections are unchanged; only formatting whitespace differs. The original supplied export remains separate.

Parsing and inspection are not execution tests. Local table identifiers and the saved `active: true` flag are retained; they are not evidence of current local availability or table contents. No substitute runtime results or manually populated result tables are part of this evidence package. [Course tutorial, pp. 9, 21–22, 25.]
