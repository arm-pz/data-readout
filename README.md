# data-readout

An agent skill that turns the numbers you already own into a readout you can act on. It opens the real file first, so row counts, columns, types and missingness come from the data rather than from a guess about it, then returns an executive summary, findings ranked by business impact, statistics with their test, interval and effect size, chart specifications written for someone else to build, forecasts as ranges with the assumptions a stakeholder can challenge, A/B calls, segment reads, dashboard metric selection and an anomaly explanation built as ranked hypotheses. Every figure you did not supply stays a visible gap.

Install: `npx skills add arm-pz/data-readout --skill data-readout`

Ask it things like:

- "Here's a CSV of my orders — what is it actually saying?"
- "Did this A/B test win, or is it too small to tell?"
- "Which few metrics should my function track weekly, and what breaks them?"
- "Churn jumped 40% last month. What could explain it and how do I check?"

## What you get

| Mode | Output |
|---|---|
| Dataset interpretation | Findings in plain English, ranked by impact, each with n and an observed-or-inferred label |
| Statistical analysis | Test, p-value or interval, effect size, and the business implication in one line |
| Chart specification | A build spec per chart: axes, encoding, scale and sort order with reasons, marks, delivery notes |
| Pattern detection | Patterns that survived a check, and the ones that look real and did not survive it |
| Forecast | A range with the backtest error behind it and an attackable assumption list |
| Segmentation | Segments with size, spread, the metric that differs, and the implication of each |
| A/B interpretation | Design audit, significance, effect size, guardrails, and one call: ship, keep running, or stop |
| Dashboard selection | Metric list with formula, owner, cadence, and a target slot you set |
| Report template | A repeatable skeleton with standing checks, plus one run filled with your numbers |
| Anomaly explanation | Ranked hypotheses, each with the check that confirms or kills it |

## Not for

Writing the pandas, R or SQL that crunches the file — that is `production-code`, and data-readout hands it a calculation spec. Market figures from outside your own data belong to `research-brief`; the roadmap that follows a finding belongs to `strategy-memo`; pressure-testing a decision you already made belongs to `crash-test`; polishing the prose of the finished readout is `wordsmith`. It also does not draw the charts it specifies, and does not report a statistic it has not run.

## Install

```
npx skills add arm-pz/data-readout --skill data-readout
```

Or copy `skills/data-readout/` into your agent's skills directory (`.claude/skills/`, `.agents/skills/`, `.cursor/skills/` or `.qoder/skills/`). With Claude Code plugins, `.claude-plugin/plugin.json` makes the repo installable as a plugin; add it to your marketplace repo as an entry pointing at `arm-pz/data-readout`.

## Structure

```
data-readout/
├── README.md
├── LICENSE
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── data-readout/
│       ├── SKILL.md                      # intake, modes, output shape, quality bar, handoffs
│       └── references/
│           ├── analysis-playbook.md      # what to do per mode
│           ├── statistical-checks.md     # significance, n, multiple comparisons, traps
│           └── report-format.md          # exec summary, ranked findings, chart-spec table
└── tests/
    └── test-prompts.md
```

## Part of a suite

One of eight skills built from the same set of prompts. Its neighbours in that set are `platform-content`, `sales-copy`, `strategy-memo`, `production-code`, `research-brief`, `study-system` and `career-track`.
