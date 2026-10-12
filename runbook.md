# Sprint 2 runbook

**Current operating status: execution blocked.** n8n has not progressed beyond the first node during my attempts, and I have communicated with my instructor. The procedures below describe intended operation and proposed recovery; they do not report successful tests or completed shutdown actions. Required section names follow the course tutorial, p. 23.

**Evidence upload note:** The PNG paths below identify prepared screenshot files. Their repository upload is pending; see `evidence/README.md`. The actual n8n JSON export is also pending.

## Purpose

**Action:** Use only the public course repository and synthetic test issues to evaluate the configured workflow. Keep the two Data Tables separate: one tracks each issue and its status, and one records the internal routing outcome.

**Expected result:** An auditable distinction between an issue record, its routing outcome, duplicate delivery, and failure. This is the course requirement, not an observed end-to-end result.

**Evidence location:** `README.md`, `test_evidence.md`, and `evidence/01_workflow_canvas.png`. [Course tutorial, pp. 1–2, 7–8.]

## Normal operation

**Action:** Once the blocker is resolved, confirm local n8n is available, the workflow is active/published, the two tables have the required columns, and GitHub CLI forwarding is running for the course repository. Create one course test issue at a time. Inspect the received payload, executed nodes, ledger, and route log before creating another issue. Keep the forwarding terminal open during the tests.

**Expected result:** An `opened` event is normalized and classified. The new key reaches `Insert pending ledger row` before any route-log insert. A normal event produces one ledger row with `route_log_status = recorded` and one route-log row with the matching key and required route. Non-opened events stop at `Opened action?` without new records.

**Evidence location:** Current configuration: `evidence/07_ledger_insert_upper.png`, `evidence/08_ledger_insert_lower.png`, and `evidence/10_route_log_mapping.png`. Actual execution and table-result evidence is unavailable; its status is in `test_evidence.md`. [Course tutorial, pp. 9–19.]

## Duplicate delivery

**Action:** Only after the original Bug issue has successfully produced one ledger row and one route-log row, open the repository's Settings → Webhooks → relevant webhook → Recent deliveries. Select the original Bug delivery and choose Redeliver. Compare records for that same `issue_key` before and after. Do not create a second Bug issue as a substitute.

**Expected result:** The existing-delivery path updates only `last_seen_at`. There remains exactly one ledger row and one `bug-triage` route-log row for that key. A replay of a failed/pending issue is not a tested recovery mechanism: the existing-row path ends after its timestamp update.

**Evidence location:** Configuration: `evidence/05_existing_timestamp_update.png` and `evidence/06_new_issue_check.png`. Before/after runtime results are not available. [Course tutorial, pp. 12–14, 19–20.]

## Route-log failure

**Action:** After event delivery is working, create the fifth course issue with the title `Documentation: intentional route-log failure test`. The configured true branch should throw the intentional error. Also inspect the normal route-log insert's error-output connection.

**Expected result:** The event first retains a pending ledger row, then reaches `Update route-log failed`. Its ledger status becomes `failed`, its `error_note` contains the safe fixed message, and no route-log record exists for that issue key. Do not copy a raw stack trace or sensitive details into the table.

**Evidence location:** Configuration: `evidence/09_intentional_failure_condition.png`, `evidence/13_intentional_failure_code.png`, `evidence/14_code_error_setting.png`, and `evidence/15_failed_status_update.png`. An executed failure result is not available. [Course tutorial, pp. 15–16, 20–21.]

## Recovery action

**Action for the present blocker:** Preserve the current workflow export and available screenshots, keep the runtime tests marked blocked, and continue the existing instructor discussion. At the next diagnostic session, check receipt of a real course webhook and inspect the first node's input before changing downstream logic. The root cause is unconfirmed; no successful forwarding or processing is claimed.

**Proposed action after a real route-log failure:** Pause new intake, preserve the affected ledger row, and identify it by `issue_key`. Inspect the failing node's table selection, column mapping, and error output. Check the route-log table for that key before retrying any insert. Because the duplicate guard stops existing keys, simply redelivering the same event will not automatically retry its route write. An instructor-reviewed controlled recovery path would need to reuse the existing ledger key, insert the route only if absent, and mark the ledger recorded only after a confirmed successful write. Do not delete the ledger or manually create rows to make evidence appear successful. This recovery extension is proposed, not implemented or tested.

**Expected result:** The original issue remains auditable. A verified controlled recovery would leave one route-log entry, not multiple entries. For the deliberate test, keep the simulation in place as the handout requires; it is not an accidental code defect to remove.

**Evidence location:** `test_evidence.md` records the current blocker and missing runtime evidence. Add real recovery evidence there only after carrying it out. The recovery design is my proposed procedure based on the course's duplicate-guard and failure-retention requirements. [Course tutorial, pp. 13–16, 21, 23.]

## Safe shutdown

**Action:** Save the workflow and documentation and keep a private original export. Deactivate/unpublish this workflow. In GitHub, remove or disable the course forwarding webhook if one was created. Stop the forwarding command with Control+C if it is running. Preserve the local persistent data; do not delete Docker volumes to stop the workflow. Record the actual shutdown time and actions once completed.

**Expected result:** No further test events are processed, and the local build and evidence remain available for review. I have not documented these shutdown actions as completed.

**Evidence location:** This section is the shutdown record location. **Current record: shutdown completion not confirmed in the supplied evidence.** [Course tutorial, pp. 24–25.]

## What this prototype does not test

**Action:** Restrict conclusions to the supplied configuration evidence and the reported blocker. Do not treat configured nodes as proof of output.

**Expected result:** The reviewer can distinguish the implemented configuration from the unverified runtime behaviors. Current evidence does not establish delivery, regex matching, table writes, duplicate suppression, the opened-action gate, recovery, or intentional-failure persistence. The course's local forwarding exercise also does not establish production availability, concurrent-delivery safety, real-user privacy handling, or performance.

**Evidence location:** The blocked-test matrix in `test_evidence.md`. Production and concurrency limitations are explicitly untested questions, not claims about observed failures. [Course tutorial, pp. 4, 23.]
