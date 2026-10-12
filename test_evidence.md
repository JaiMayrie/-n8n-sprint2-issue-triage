# Sprint 2 test evidence

## Repository upload note

The 16 PNG files described below are prepared in the submission package but have not yet been uploaded to this repository. Their links identify the intended paths, not completed uploads. The actual n8n workflow JSON is also still awaiting upload. See [evidence/README.md](evidence/README.md) for the image inventory.

## Submission status

**Configuration documented; runtime verification blocked.** During my execution attempts, n8n does not move beyond the first node. I have already communicated with my instructor about this issue. The root cause has not been established.

The prepared screenshots show configured nodes, field mappings, connection layouts, and GitHub CLI login. They do not show successful execution or populated table results. No runtime test below is marked passed. A screenshot of a node configured to write `failed` is not evidence that the deliberate-failure test ran.

## Configuration evidence prepared; PNG upload pending

The original screenshots were captured during the October 10 build session. The prepared copies are cropped to remove unrelated browser and desktop areas. No execution output or table data has been added to them. Some field screenshots precede later node renaming or connection corrections; the latest canvas is the reference for the final visible branch layout.

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
| [12_recorded_status_update.png](evidence/12_recorded_status_update.png) | Recorded status and blank error-note mapping | Successful status transition |
| [13_intentional_failure_code.png](evidence/13_intentional_failure_code.png) and [14_code_error_setting.png](evidence/14_code_error_setting.png) | Deliberate error code and error-output configuration | Deliberate failure executed |
| [15_failed_status_update.png](evidence/15_failed_status_update.png) | Failed status and safe message mapping | Retention of a failed ledger row |
| [16_github_login_status.png](evidence/16_github_login_status.png) | GitHub CLI logged in with an active account | Live webhook forwarding or n8n event receipt |

The Feature field-retention toggle, initial payload mappings, and actual Switch regex configuration still require final inspection in the real workflow export/editor. They are not established by the screenshots listed here.

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

**Known preparation:** The editor is accessible; the node configuration and connection work is documented; GitHub CLI installation and active login were checked. The supplied screenshots do not establish that a forwarding command successfully delivered an event.

**Configuration work completed while preparing the build:** Removed duplicate route/check nodes; separated field names from values; corrected the route-log table selection; limited update mappings to intended columns; corrected the success/error connections; and configured the intentional-failure branch. These changes are build work, not proof that they resolved the runtime problem.

**Diagnosis status:** Unconfirmed. No exact runtime error message or successful received payload is available here. I am not attributing the blocker to a specific component or cause without evidence.

**Instructor communication:** I have already communicated about the execution issue. Private correspondence is not included in this public repository.

**Next evidence to add, when available:** A genuine screenshot of the current blocked state or relevant execution message, cropped to remove private information. It may be named `evidence/execution_blocked.png`. That image is not included among the current configuration captures. After resolution, replace blocked statuses only with actual execution and table-result evidence.

## Export and publication checks

The workflow artifact should be a second, submission-safe copy exported from the actual n8n editor and named `sprint2_github_issue_triage.json`. It should be reviewed for credentials, tokens, secrets, sensitive notes, and pinned payload data before publication. The original private export stays outside the repository. No substitute JSON or manually populated result tables are part of this evidence package. [Course tutorial, pp. 9, 21–22, 25.]
