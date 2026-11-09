---
name: feature-gate
description: Checks finished work against the user's evaluation criteria document at the end of each feature. Maps the feature to criteria, runs tests and a real run, reports met, unmet, and unknown per criterion, and decides whether to continue. Use at every feature completion when the user supplied a criteria document, or when asked to check progress against the criteria.
disable-model-invocation: true
icon: check-circle
color: blue
---

# Feature gate

The user writes one evaluation criteria document per task. It says how the finished work will be judged. You and the user work together to meet it. This skill is the checkpoint that keeps the work on that path.

Read [my-principles](../my-principles/SKILL.md) first. Its Verification and Commits rules apply here.

## Locate the document

1. Use the path the user gave. If none, look at the project root for `CRITERIA.md`, `GOAL.md`, `EVAL.md`, or `docs/criteria.md`, and for a path named in `CLAUDE.md`.
2. If you find several candidates or none, ask the user once and record the answer in the progress file below so you never ask again.
3. Never copy the document into the plugin or paraphrase it into new requirements. Read it from where it lives each time, because the user may have edited it.

## Progress file

Keep `.my-pstack/progress.md` at the project root. Add `.my-pstack/` to `.git/info/exclude` so it never reaches a commit. It holds the criteria document path, one row per feature, and one row per criterion with its last status. Create it on the first run. Update it on every run.

## Steps

1. **Read.** Read the whole criteria document. List every criterion with a stable id (use the document's own numbering, or number them in order).
2. **Map.** Name the feature that just finished, in one line. Tie it to the criteria it can affect. A feature may touch many criteria. A criterion may need many features.
3. **Run.** Run the static checks, the tests that cover the feature, and a real run of the changed behavior, per my-principles Verification. Capture the command and the key output of each run. Delegate long runs to a subagent when output is large.
4. **Judge.** For each mapped criterion write one status.
   - `met`. You ran something that shows it, and you can cite it.
   - `unmet`. You ran something that shows it fails, or the behavior is absent.
   - `unknown`. You could not check it. Say what blocked the check.
   Never mark `met` from reading code alone. Never mark a criterion `met` because a test you wrote passes when the test does not exercise what the criterion states.
5. **Drift.** Compare the feature list with the criteria. Name work done that no criterion asks for, and criteria that no planned feature will reach. Say plainly when the work is heading the wrong way.
6. **Decide.** Recommend one of: continue with the next feature, fix an unmet criterion first, or stop and ask the user because a criterion is ambiguous or conflicts with another. A genuine ambiguity in the document is the user's call. Everything you can settle by running something is not.
7. **Record.** Update the progress file. Commit any fix per my-principles. Do not commit the progress file.

## Report

Reply in Korean. Open with the feature and the one-line verdict. Then a table with one row per mapped criterion and the columns id, status, and evidence (command and result). Then drift, then the decision, then open questions. Keep the whole report short.

## Cadence

Run this at every feature completion. Also run it when the user asks "are we on track" or "check against the criteria". When a feature takes a long time, run a lightweight pass (steps 1, 2, 5) at natural pauses, without the full run.
