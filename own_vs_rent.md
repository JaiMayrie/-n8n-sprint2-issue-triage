# Own versus rent: self-hosted n8n and Zapier

## Scope and evidence

This is a documentation-based comparison, not a completed hands-on Zapier experiment. The n8n build is configured, but runtime verification is blocked at the first node. No relative speed, uptime, successful execution count, or measured monthly bill is claimed.

The Sprint 2 tutorial requires an `own_vs_rent.md` comparison and cost analysis (p. 22), but the supplied tutorial does not specify a traffic volume, labor rate, semester cost scenario, word count, or detailed Zapier test procedure. The planning assumptions below are my hypothetical analysis, not additional instructor requirements. External product facts are cited separately from the course source. Sources consulted October 10, 2026.

## Functional comparison

| Consideration | Self-hosted n8n in this course build | Zapier alternative: proposed, not tested |
| --- | --- | --- |
| Hosting | The course keeps n8n and the two Data Tables local; CLI forwarding must remain running during tests. [Course, pp. 3–4, 16–17.] | A managed automation alternative. The proposal would move this workflow's processing/state into Zapier rather than reproduce the required local course deployment. [1] |
| Logic and routing | The configured design uses an opened-action gate, normalization, a Switch, four route-setting nodes, and shared ledger checks. [Course, pp. 9–16.] | A proposed equivalent would need event intake, action filtering, deterministic matching, branching, a durable ledger lookup, and separate success/error handling. Feature equivalence and payload behavior would need a real test. |
| Persistent state | `github_issue_ledger` and `github_issue_route_log` have separate purposes in the assignment. [Course, pp. 7–8.] | Zapier Tables is a possible state store; its availability does not by itself prove identical duplicate or recovery behavior. [1][2] |
| Plan constraints | n8n documents a free self-hosted Community edition, but hosting and maintenance are still my responsibility. This comparison assumes Community edition rather than a paid self-hosted license. [3] | The pricing page lists Free at $0 with 100 tasks/month and two-step Zaps. Professional starts at $19.99/month and includes multi-step Zaps and Webhooks. A direct multi-step implementation of this design would not fit a single Free two-step Zap. [1] |
| Portability and operation | The submitted artifact is an actual JSON export; I must maintain my local installation and preserve its data. [Course, pp. 21–25.] | I would still need to maintain field mappings, task usage, authentication, and test evidence. Choosing a managed service would not remove those responsibilities. This is my implementation assessment, not a measured result. |

## Actual experience from this build

I was able to assemble the workflow and inspect its fields and connections. My execution attempts did not progress beyond the first node, and I have communicated with my instructor. That makes troubleshooting time a real consideration in an own-versus-rent decision. It does not establish that self-hosting caused the problem, and it does not show that Zapier would have avoided it. No numerical time log, electricity measurement, or hosting invoice is available.

## Cost model

The model separates the software price from the time and infrastructure needed to operate it:

```text
Monthly self-hosted cost = license + incremental infrastructure + maintenance hours × hourly value
Monthly managed cost    = subscription + extra usage charges + maintenance hours × hourly value
```

For a no-license Community installation, the n8n license term is $0. [3] Zapier's displayed Professional starting price is $19.99/month; the page offers monthly and yearly billing selections. That is a reference price, not a verified month-to-month quote for a particular task tier. The actual checkout amount, billing commitment, capacity, and taxes require confirmation. [1]

### Illustrative monthly budget, not actual spending

Assume an existing computer, no new hardware purchase, no extra usage charges, and a hypothetical time value of $20/hour. Assume $2/month in incremental local running cost, three local maintenance hours, and one managed-service maintenance hour. None of these time or infrastructure estimates was measured in my build.

| Item | Self-hosted illustration | Managed-service illustration |
| --- | --- | --- |
| Software/subscription reference | $0 Community license | $19.99 displayed starting reference |
| Incremental local running cost | $2 assumed | $0 assumed for a separate local server |
| Maintenance time | 3 × $20 = $60 | 1 × $20 = $20 |
| Illustrative total | **$62/month** | **$39.99/month before any additional charges** |

Under those assumptions, the managed illustration is $22.01 lower per month. This is a sensitivity example, not a conclusion that Zapier is actually cheaper. With labor valued at $0 and the other assumptions unchanged, the comparison is $2 versus $19.99. Different maintenance needs, hosting expenses, billing terms, task tiers, and usage can reverse the result. Shared internet and ordinary computer costs are excluded from both columns; the comparison considers only assumed incremental costs.

## Usage and scale

Do not estimate Zapier tasks by counting all n8n boxes. Zapier's task-rates page states that built-in tools including Tables, Filters, Paths, and Formatter do not use tasks, while other successful steps can incur usage; Code has its own listed rate. Its general pricing FAQ also says checking/polling for data does not consume tasks. The actual intake choice and final implementation need verification before assigning a measured task count. [1][2]

A planning formula is:

```text
Expected billable tasks = new issues × billable units per new issue
                       + redeliveries × billable units per redelivery
                       + other billable recovery/actions
```

For example, **assuming** one billable unit per new issue, 100 new issues would use 100 units, and 1,000 would use 1,000 units before redeliveries or recovery. This is only arithmetic under an assumed implementation, not an observed bill or a claim that each table write is billable. A compatible plan still needs the required multi-step functionality even when some actions have no task charge. Storage limits and retention must also be checked before extrapolating a semester's use.

## Conclusion

The local n8n build is the required course deliverable and makes the data model, state transitions, and failure paths explicit. For this submission, the defensible result is a configured prototype with blocked runtime tests. For a future service decision, I would compare validated event delivery, duplicate behavior, recovery, and measured operating effort before deciding whether the control of self-hosting outweighs managed-service charges. I would not purchase a plan based on the hypothetical totals alone.

## Sources

Course source: *Sprint 2 Tutorial: GitHub Issue Triage Automation*, supplied course handout, pp. 3–4, 7–16, 21–25.

[1] Zapier. “Plans & Pricing.” https://zapier.com/pricing — public plan features, displayed prices, billing choices, and polling/task FAQ. Consulted October 10, 2026.

[2] Zapier. “Task usage rates.” https://zapier.com/pricing/rates — current categories of counted and uncounted usage. Consulted October 10, 2026.

[3] n8n. “Host n8n.” https://docs.n8n.io/hosting/ — self-hosting options and free Community edition without a paid license key. Consulted October 10, 2026.
