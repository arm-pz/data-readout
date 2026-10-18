# Test prompts

Cases for `data-readout`: three clear triggers, two implicit triggers, three near-misses that belong to a sibling, plus edge and spanning cases. Each row states a pass condition you can check by reading the output.

## Cases

| # | Message | Expected skill | Expected mode or command | Pass condition |
| --- | --- | --- | --- | --- |
| 1 | "I've got `orders.csv` in this folder. Tell me what's happening with refunds." | data-readout | Dataset interpretation | Opens the file first; row count, column names, types and missingness quoted from what it read; n stated in every finding; findings ranked by impact; nothing described that it did not compute |
| 2 | "Two arms, no sales: A had 1,203 buyers out of 24,110 sessions, B had 1,289 out of 24,005. Did we win?" | data-readout | A/B test interpretation | Names the test, gives the p-value or interval, the absolute and relative difference, n per arm; one call (ship, keep running, stop); asks or flags that assignment unit and planned end date are unknown |
| 3 | "What should I chart to show the board why revenue slipped last quarter?" | data-readout | Chart specification | Written specs (title, question, type, x, y, encoding, scale with reason, sort order, marks, delivery); no generated image, no narration of a chart it has not been told about; one chart per finding |
| 4 | "Churn jumped 40% last month and I have no idea why." | data-readout | Anomaly explanation | Quantifies how unusual against an expected band; rules out data artefacts (pipeline, backfill, timezone, bot filter) before business causes; ranked hypotheses each with the confirming check; never one confident cause |
| 5 | "I report to leadership every week and nothing changes. What should we even be watching?" | data-readout | Dashboard metric selection | Metric list with formula, source, grain, owner, cadence, and a counter-metric against gaming; targets left as `[target — yours to set]`; no invented industry benchmark |
| 6 | "Write the pandas script that loads this CSV and builds the cohort retention matrix." | production-code | Handoff, not analysis | Says the code is `production-code`'s, sends a calculation spec (input, columns, filters, formula per metric, expected output shape) in the handoff block; does not write the script |
| 7 | "What's the average churn rate for streaming services in 2025?" | research-brief | Handoff | Refuses to produce an outside-market figure from its own knowledge or the user's file; hands the question over with granularity, period and use stated |
| 8 | "Given all this, should we shut the EU rollout, and what is the ninety-day plan?" | strategy-memo | Handoff after a short readout | Delivers the findings and the constraints the data exposed, then hands the decision and roadmap to `strategy-memo`; does not produce a roadmap itself |
| 9 | "Here's three months of signups in this sheet. Project next quarter." | data-readout | Forecast | A range, not a point; method named; assumption list a stakeholder can attack; the single assumption that moves the range most identified; states the basis for the band or marks it unverified |
| 10 | "My p-value is 0.04. Is that good news?" (n = 40) | data-readout | Statistical analysis | Explains significance against effect size; wide interval stated; does not call it a win and does not call it a null; says what n would settle it |
| 11 | "Is our email open rate good?" (no data supplied) | data-readout | Intake | One batched ask capped at three questions, or a framework with `[metric — supply this]` slots; no invented benchmark or industry average; if it proceeds, it prints the assumptions used |
| 12 | "Read this analytics export and then give me the code so I can rerun it monthly." | data-readout then production-code | Readout first, then handoff | Does the readout on the real file first, then emits the `HANDOFF — data-readout -> production-code` block with the calculation spec and the gaps; numbers are not rewritten in the handoff |
| 13 | "Just polish the wording of this readout, the numbers are final." | wordsmith | Out of scope | Hands over rather than re-cutting prose; does not touch, round or "improve" any figure |

## Setups

- Cases 1, 3, 9, 12: a small real CSV in the working directory (a few hundred rows, one date column, one category column, deliberate missing values) so the file can actually be opened. If it cannot be read, the correct behaviour is to say so once and ask for the rows pasted, then scope the readout to what it was shown.
- Case 2: no file, only the counts in the message. The readout is limited to a two-proportion comparison, and the output must say which design facts it could not check.
- Case 7: no data at all; the point is that external figures are not this skill's to state.
- Case 11: no toolchain needed; it tests the intake rule.

## Boundary pairs

| Message | Should trigger | Why |
| --- | --- | --- |
| "What do these refund numbers say?" | data-readout | Interprets the user's own figures |
| "Write me the script that computes the refund rate by cohort each Monday" | production-code | Wants reusable code, not a readout |
| "Our conversion is 1.8% — is that in line with the market?" | data-readout first, then research-brief | The user's number is in scope; the market comparator is not, and must be sourced and dated by `research-brief` |
| "Revenue is down 12%, what do we do about it?" | strategy-memo | Wants a decision and a sequence; data-readout supplies the evidence block it starts from |
| "Did the test win?" | data-readout | Statistical read of a result the user supplies |
| "We already decided to ship it, try to break that call" | crash-test | Pressure-testing a made decision, not interpreting a result |

## Running them

Not yet run. This table is the written contract; nothing here claims a passing result.

How to run them:

- **By hand:** read only the frontmatter descriptions of `data-readout`, `production-code`, `research-brief`, `strategy-memo`, `crash-test` and `wordsmith`, decide which skill an agent would pick for each message, and mark pass or fail. Quick, and it catches most overlaps.
- **With tooling:** where Claude Code is available, skill-creator's description-improvement loop can run these against real triggering. It does not run in a plain chat, so do not report that it did.
- Cases 1, 3, 9 and 12 need a real file present before their pass conditions can be judged; the rest are checkable from the description and the output text alone.

Failures worth re-checking after any edit to the description:

- Cases 6, 7, 8 and 13 are the boundary. If a description change pulls any of them into `data-readout`, revert it.
- Case 12 spans two skills. If it triggers only `production-code`, the readout is lost; if it triggers only `data-readout`, the code request is dropped.
- The sibling descriptions were written by other builders and were not visible when this file was authored, so every "Expected skill" row for a near-miss is a claim to verify against their final text.

