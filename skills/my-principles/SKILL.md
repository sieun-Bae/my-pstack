---
name: my-principles
description: The owner's standing working rules for every my-pstack task, kept in one place. Korean replies, small automatic commits on a work branch, lint plus a real run before done, Windows as the default environment. Read it at the start of any code task.
disable-model-invocation: true
icon: scroll
color: green
---

# My principles

These rules are the single source for the owner's preferences. Other skills point here and never copy them. Change a rule once, in this file.

## Language

Reply to the user in Korean. Keep code, identifiers, commands, file paths, and commit message subjects in their original language. Write each subagent brief in English unless the user asks otherwise, and tell the subagent to report back in Korean.

## Commits

Commit small and often on a work branch.

1. If the current branch is the default branch (`main`, `master`, or `develop`), create a work branch first. Name it `feat/<slug>` or `fix/<slug>`.
2. Finish one logical unit, run its checks (see Verification), then commit it. Never batch several units into one commit and never leave a green unit uncommitted.
3. Commit without asking. This standing grant covers local commits on a work branch only.
4. Never push, force-push, merge, or open a PR unless the user asks in that message. A push is the user's call.
5. Never commit a failing check, a secret, or a file the unit did not touch. Stage paths by name.
6. End each commit message with the attribution trailer the session specifies.

## Verification

Done means checked and run, not "it compiles".

1. **Static checks.** After code changes, run the project's lint, formatter check, and type check, whichever the project defines. Find them in the project manifest or CI config. If none exist, say so in the report and do not invent one.
2. **Tests.** Run the tests that cover the change. Run the whole suite before the final report when it takes under a few minutes.
3. **Real run.** Start the program and exercise the changed behavior through its real surface (CLI, HTTP call, UI). A passing test is not a run. If the app cannot be started, say exactly why and what you ran instead. Never report "verified" on a proxy.
4. Fix what the checks find before the next unit. Report a check you could not run as a gap, never as a pass.

## Environment

The default environment is Windows 11 with PowerShell and Git Bash.

- Use forward slashes in Git Bash and quoted backslash paths in PowerShell. Quote paths with spaces.
- Do not assume `/tmp`, `python3`, `node`, `make`, or Unix-only tools exist. Check with `command -v` or `Get-Command` first.
- Prefer a command that works in both shells. When a script is Unix-only, run it under Git Bash and say so.
- Line endings. Respect the repository's `.gitattributes`. Do not rewrite line endings of untouched files.
- If a step needs a Unix-only feature and no Windows route exists, report it as skipped with the reason.

## Evaluation criteria document

When the user supplies an evaluation criteria document for the task, run the **feature-gate** skill at the end of every feature. The criteria document is the definition of done for the work.
