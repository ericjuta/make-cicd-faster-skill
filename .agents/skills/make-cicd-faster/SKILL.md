---
name: make-cicd-faster
description: Audit slow or expensive CI pipelines, identify unnecessary jobs and repeated setup, and compare changes in feedback time and compute cost without weakening merge or release checks.
---

# Make CI faster

Start in audit mode unless implementation was requested. Read the actual workflows, scripts they invoke, and required-check configuration. If branch rules or billing data are unavailable, mark them unknown rather than inferring them from YAML.

## Trace one change through CI

Pick representative recent PRs, including a retry or superseded revision. Record run links, commit, event, changed scope, runner class and outcome. Separate queue, setup and useful execution time. Sum runner-minutes across jobs **including cancellations** for compute; measure elapsed time to the last required result for feedback. Parallel job durations add to the former, not the latter. Estimate cost per class using its rate and billing granularity; distinguish estimates from invoices. Report sample size and workload shifts.

## Choose the first experiment

| If the evidence shows… | Try… | Guardrail |
|---|---|---|
| Superseded revisions or irrelevant jobs paying startup cost | Cancel obsolete PR runs; decide eligibility before allocating expensive runners. | Keep required status reporting intact; never cancel a deployment mid-flight merely because a newer push arrived. |
| The same setup across many short shards | Combine small jobs or share immutable build outputs. Measure transfer time against rebuilding. | Key reuse on source, toolchain, lockfile, platform and relevant config. Never promote untrusted PR artifacts into privileged jobs. |
| Most tests unrelated to a typical edit | Select tests using dependency or observed execution data; compare selections against full runs in shadow mode. | Missing base, stale map, unknown dependency or global config change: run the full suite. Keep periodic full validation and investigate misses. |
| A few slow shards determine feedback time | Balance by observed durations, then vary worker count or runner size on the same revision. | Watch memory pressure, retries and setup duplication. A cheaper job can still delay everyone. |
| Tests repeatedly exercise the same contract | Identify the realistic regression and strongest surviving assertion before consolidating. | Demonstrate the surviving test fails on a deliberately broken scratch change; never remove the sole regression guard. |

Map each check to PR, main, schedule or release. Move broader checks only with a named failure owner and a policy that ties release evidence to the exact revision shipped. Required gates are constraints, not savings opportunities.

## Deliver and verify

Rank candidates by measured waste, effect on the critical path, confidence and risk. For the top candidate give the workflow location, proposed change, baseline, comparison and rollback. If authorized, change one variable in a reviewable patch. Replay the same revision/input on both configurations when possible, including a known failing case. Compare outcomes, feedback time and runner cost together. Report regressions, uncertainty and whether to keep or revert; do not attribute a fleet-wide trend to one patch.

Source: [Vincent Koc’s OpenClaw CI case study](https://vincent.build/openclaw-ci-cost-reduction/) and its [versioned workflow example](https://github.com/openclaw/openclaw/blob/a7d05c7af04aa17ac57c994da238d0c60f058ca6/.github/workflows/ci.yml). Adapt the method, not OpenClaw’s thresholds or reported savings.
