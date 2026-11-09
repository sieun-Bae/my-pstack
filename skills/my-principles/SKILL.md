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
2. Finish one logical unit, run its checks (see Verification, and the Python gate for Python projects), then commit it. Never batch several units into one commit and never leave a green unit uncommitted.
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

## Python projects

A project is a Python project when it has `pyproject.toml`, `setup.py`, `setup.cfg`, `requirements*.txt`, or `*.py` files at its core. The rules below apply to every Python file you write or change. Existing project configuration (`pyproject.toml`, `setup.cfg`, `.flake8`, `ruff.toml`) wins over these defaults.

1. **Style (PEP 8).** Indentation, whitespace, blank lines, line breaks, and line length follow PEP 8. Format with `black` and check with `black --check`. If the project configures `ruff format` instead, use that.
2. **Naming and imports.** Variables and functions use `snake_case`, classes use `PascalCase`, constants use `UPPER_SNAKE_CASE`, and private names start with one underscore. Names say what the thing is, with no cryptic abbreviations. Group imports as standard library, third party, then local, with a blank line between groups, sorted, with no unused imports and no wildcard imports. Fix with `isort` (use the `black` profile) or `ruff check --select I --fix`.
3. **Docstrings and comments.** Every module, public class, and major function has a docstring that states its role, its inputs (parameters and types), and its outputs (return value and raised errors). Keep the project's existing docstring style (Google, NumPy, or reST) and language. If the project has none, use Google style and ask the user once which language to write it in. Add a comment only where the reason is non-obvious. Do not narrate what the code already says.

**Python gate.** Run it before every commit that touches a `.py` file, together with the tests that cover the change, so each commit is formatted, named, documented, and verified at once.

- Format and fix first, then check: `black`, `isort`, then the checks below.
- Lint, naming, and docstrings: `ruff check --select E,W,F,I,N,D` (or `flake8` with `pep8-naming` and `flake8-docstrings` when the project uses flake8). Pick the style convention for `D` from the project, for example `--ignore D203,D213` for Google style.
- Tests for the change, then the real run from Verification.
- Use the tools the project already declares. If a tool is missing, install it into the project's virtual environment as a dev dependency only after telling the user. Never install into the global interpreter. If you cannot run a tool, report it as a gap.
- Fix every finding in the same unit before committing. Never silence a finding with `noqa` or `# fmt: off` unless it is a genuine false positive, and say why in the code.
- Keep formatting-only changes out of logic commits. When a first run reformats untouched files, commit that as its own `style:` commit before the unit.

## Environment

The default environment is Windows 11 with PowerShell and Git Bash.

- Use forward slashes in Git Bash and quoted backslash paths in PowerShell. Quote paths with spaces.
- Do not assume `/tmp`, `python3`, `node`, `make`, or Unix-only tools exist. Check with `command -v` or `Get-Command` first.
- Prefer a command that works in both shells. When a script is Unix-only, run it under Git Bash and say so.
- Line endings. Respect the repository's `.gitattributes`. Do not rewrite line endings of untouched files.
- If a step needs a Unix-only feature and no Windows route exists, report it as skipped with the reason.

## Evaluation criteria document

Every task starts with an evaluation criteria document. It is the definition of done and the basis for tests.

1. **At the start of a task**, before planning or writing code, locate the document with the **feature-gate** skill's Locate step.
2. **If none exists**, stop and ask the user to write one first. Ask in Korean with the question tool. Offer three options: the user gives a path to an existing document, the user writes it now, or the two of you draft it together from the task description. Do not start implementation until a document exists or the user explicitly waives it in plain words. Record a waiver in the progress file and run feature-gate without a document in lightweight mode.
3. **At the end of every feature**, run the **feature-gate** skill.
