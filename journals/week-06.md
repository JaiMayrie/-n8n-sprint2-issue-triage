# Weekly Build Activity Journal

Course: CS 590 Low Code No Code for Rapid Prototyping  
Student: Jaida Calvin  
Week: 6 (Sprint 2 assignment label)  
Date range: October 5-11, 2026  
Build screenshots: October 10, 2026  
Course work/project: Sprint 2 - GitHub Issue Triage Automation

**Repository copy note:** The configuration screenshots referenced below are prepared in the submission package, but their PNG upload to this repository is pending. The separate Word journal already contains selected screenshots. The actual n8n JSON export is also awaiting upload.

## 1. Weekly Focus and Goals

This week I worked on Sprint 2 - GitHub Issue Triage Automation in self-hosted n8n. I assembled the workflow and corrected its fields and connections. Execution remains blocked, so the configuration work is not a claim that the workflow passed its tests.

1. Build the four title-based routes: Bug, Feature, Help, and Other.
2. Configure the ledger checks, route-log write, and success/failure updates.
3. Verify the required tests and prepare the submission evidence. Testing remains blocked.

## 2. Completed Work and Evidence

The evidence below shows configuration and setup, not successful runtime results.

| Completed work | Evidence | Why it matters to this week's objective |
| --- | --- | --- |
| Built and corrected the four category routes, removed extra nodes, and connected every route to the same two ledger checks. | [Figure 1: workflow canvas](../evidence/01_workflow_canvas.png) | Shows a shared duplicate-check structure rather than four repeated workflows. |
| Configured the existing-row update to change only last_seen_at and added the pending-ledger insert for new issues. | [Figure 1](../evidence/01_workflow_canvas.png); [Figure 2: timestamp update](../evidence/05_existing_timestamp_update.png) | Separates existing issues from new records and limits the fields being updated. |
| Configured the failure update to write failed and a readable error_note. Added the intentional-failure branch during the build. | [Figure 3: failure update](../evidence/15_failed_status_update.png); [failure code](../evidence/13_intentional_failure_code.png); [error setting](../evidence/14_code_error_setting.png) | Documents the planned failure response without claiming that the failure test ran. |
| Confirmed that GitHub CLI was installed and that my GitHub account had an active login. | [Figure 4: login](../evidence/16_github_login_status.png) | Confirms authentication preparation, not successful webhook forwarding. |

## 3. Work in Progress and Next Step

| Work in progress | Current state | Next concrete action |
| --- | --- | --- |
| Workflow execution | My attempts do not progress beyond the first node. | Continue the existing instructor discussion and check whether a course event reaches the webhook. |
| Required runtime tests | The four categories, redelivery, opened-action edit, and intentional failure are blocked/not verified. | Once execution is available, run the tests one at a time and capture actual execution and table results. |
| Submission checks | Written documentation and configuration screenshots are prepared. | Confirm that the actual workflow export is included, sensitive values are removed, and the required files are uploaded. |

## 4. What You Learned

I learned that a field name and its value belong in separate boxes. For example, category is the field name and bug is its value. I also learned that selecting the correct Data Table matters before mapping columns. I corrected the route-log table selection and removed unused fields from Update nodes so only the intended values would be changed.

The prototype dimension I examined most directly was implementation through reviewing the nodes and their connections. I also checked data shape through the field mappings. This was a configuration review, not an end-to-end execution test. The main unanswered question is whether one actual issue can complete the workflow and create the required records.

Extensibility observation: All four categories use the same ledger checks and downstream routing logic. A future category could be added with a new Switch rule and a route-setting node connected to those existing checks, rather than another copy of the entire workflow. I would also update the rule order, route name, documentation, and tests. I have not implemented or tested that extension.

## 5. Challenge and Diagnosis

**What happened:** When I tried to execute the workflow, n8n did not move beyond the first node. I have already communicated with my professor about this problem. I do not have a confirmed cause or a specific runtime error message to report.

**What I tried or corrected:** I reviewed the connections, removed duplicate nodes, separated field names from values, corrected the table selection, limited update mappings, and checked GitHub CLI installation and login. These corrections improved the configuration, but did not establish that the execution problem was resolved.

**What I will try next:** At the next troubleshooting review, I will check whether the webhook receives an event and record any exact message before changing later nodes. I will keep the current screenshots and workflow for comparison.

## 6. Response Plan

My next priority is to continue the instructor-supported review of the execution problem. My immediate submission will identify the completed build work and the blocked tests separately. After event processing is working, I will complete the required tests and replace blocked statuses only with results I actually observe. I will keep tokens, passwords, and private correspondence out of the public submission.

## Course Evidence Check

- [ ] The prepared configuration screenshots are uploaded to this repository. Upload pending; selected screenshots are included in the separate Word journal.
- [x] Evidence uses course configuration and excludes secrets and private correspondence.
- [x] A specific build outcome and an unresolved execution challenge are described.
- [x] Extensibility and safe handling of public evidence are addressed.

**Runtime status: All required live tests remain blocked/not verified.**

Template: build_journal_template_v2.docx, sections 1-6 and Course Evidence Check. Assignment: Sprint 2 Tutorial: GitHub Issue Triage Automation, pp. 12-16 and 22.
