# Merging upstream pstack into my-pstack

my-pstack is a fork of pstack from `cursor/plugins`, subdirectory `pstack`. The fork keeps its own changes small so upstream merges stay cheap.

## Branch model

- `upstream`. A pristine copy of each upstream release, one commit per version. Never edit it by hand.
- `main`. The released my-pstack. Upstream changes arrive here by merge.
- Tag `upstream-<version>` marks each imported release. The baseline is `upstream-0.15.36`.

## What my-pstack changes

Keep this list current. A merge conflict can only happen in these places.

- `plugin.json`, `.claude-plugin/plugin.json`. The name is `my-pstack`, the version is the fork's own.
- `README.md`. A fork notice at the top.
- `docs/harness/claude-code.md`, `docs/guide/01-setup.md`, `skills/poteto-help/SKILL.md`. The `pstack:` prefix becomes `my-pstack:`.
- `skills/poteto-mode/SKILL.md`, `agents/poteto-agent.md`, `skills/poteto-mode/playbooks/{feature,bug-fix,refactoring,opening-a-pr}.md`, `skills/principle-prove-it-works/SKILL.md`, `skills/principle-sequence-verifiable-units/SKILL.md`. One pointer line to `my-principles` each.
- New files, which never conflict: `skills/my-principles/`, `skills/feature-gate/`, `docs/upstream-merge.md`.

## Steps

Run these in Git Bash or PowerShell from the repository root.

1. Get the new upstream release. Upstream ships a generated Claude Code copy under `harness/pstack/` in `cursor/plugins`. Clone it to a scratch directory outside this repository.
   ```
   git clone --depth 1 https://github.com/cursor/plugins.git ../plugins-upstream
   ```
2. Import it onto `upstream` as one commit.
   ```
   git checkout upstream
   git rm -r -q --cached . && git clean -fdxq
   cp -r ../plugins-upstream/harness/pstack/. .
   git add -A
   git commit -m "import: pstack <version> (upstream <sha>)"
   git tag upstream-<version>
   ```
   Read the version from the copied `plugin.json`. Use `Copy-Item -Recurse` in PowerShell if `cp` is missing.
3. Review what upstream changed before merging.
   ```
   git diff upstream-<old-version> upstream-<version> --stat
   ```
4. Merge into a work branch, never straight into `main`.
   ```
   git checkout main
   git checkout -b chore/merge-upstream-<version>
   git merge upstream
   ```
5. Resolve conflicts in the files listed above. Keep upstream's text and re-apply the fork's one-line change. If upstream renamed or deleted one of those files, move the pointer to the new place and update the list in this document.
6. Re-check the fork's changes survived.
   ```
   git grep -n "my-principles" -- skills agents
   git grep -n "pstack:" -- docs README.md skills
   ```
   The first command must list every wired file above. The second must show only `my-pstack:` prefixes. The plugin name in both manifests must still be `my-pstack`.
7. Verify the plugin loads. Install the work branch locally, start a session, and confirm that `/my-pstack:poteto-mode`, `/my-pstack:feature-gate`, and `/my-pstack:my-principles` resolve. Run one small task and confirm Korean replies, a small commit on a work branch, and a real run before done.
8. Merge the work branch into `main`, bump the version, and tag it.

## Rules

- Prefer a pointer line to copying text. A copied rule drifts and conflicts. A pointer merges cleanly.
- Add a fork change only when the owner's rules need it. Put new behavior in new files first.
- Do not rename the `pstack-models.md` rule file. `setup-pstack` still writes it and the adapter still reads it.
