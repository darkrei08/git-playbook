# 04. Forks and pull requests

**Question:** How do I contribute without confusing my fork, the upstream project, and my local branch?

The contribution loop has three identities: the canonical project (`upstream`), your GitHub fork (`origin`), and a short-lived local topic branch. Keep the local branch focused and the fork's `main` disposable and synchronized.

## Start the loop

```bash
gh repo fork OWNER/PROJECT --clone=false
gh repo clone YOUR-OWNER/PROJECT
cd PROJECT
git remote add upstream OWNER/PROJECT.git
git remote -v
git switch -c fix/short-symptom
```

The CLI help for the installed `gh` verified `repo fork`, `repo clone`, and the `--clone` flag. Replace placeholders; do not paste private URLs into a public guide.

Use branch names that identify the change, not the person: `fix/short-symptom`, `docs/command-model`, or `feat/small-capability`.

## Commit and open the PR

```bash
git status -sb
git add -p
git commit -m "Fix the symptom"
git push -u origin fix/short-symptom
gh pr create --fill
```

`gh pr create --fill` uses the pushed branch and commit metadata to populate the pull request. Review the title and body before submitting; generated text is a starting point, not evidence.

## Respond to review

Make the requested change locally, run the project's checks, and push another commit:

```bash
git add -p
git commit -m "Address review feedback"
git push
gh pr checks
```

Use `gh pr view` and `gh pr diff` to inspect the exact review state. If the branch is private to you and the project permits it, clean up its commit series with interactive rebase before pushing again:

```bash
GIT_SEQUENCE_EDITOR=true git rebase -i --autosquash upstream/main
git push --force-with-lease origin fix/short-symptom
```

**WARNING:** The force-with-lease is safe only relative to the remote state you fetched. It still rewrites the PR branch. Never force-push a branch that other contributors are using without coordination.

If review asked for a change that is easier to express as a new commit, leave the history alone. Reviewability matters more than a perfectly linear graph.

## Update without polluting the branch

For a private topic branch, first update your local view, then rebase the topic onto the current upstream base:

```bash
git fetch upstream
git switch main
git merge --ff-only upstream/main
git switch fix/short-symptom
git rebase main
git push --force-with-lease origin fix/short-symptom
```

This keeps upstream integration out of the topic branch's visible history. If the branch is shared, use a merge or coordinate the rewrite instead. See [02: Remotes](02-remotes.md) and [03: Daily commands](03-daily-commands.md).

## Keep the fork in sync

Sync the fork's default branch separately from working branches:

```bash
git fetch upstream
git switch main
git merge --ff-only upstream/main
git push origin main
```

If your fork's `main` has a local-only divergence and the project policy allows replacing it:

```bash
git push --force-with-lease origin main
```

Do not merge `upstream/main` into every working branch just to synchronize the fork. Update `main`; rebase an active private branch only when it needs the new base.

## Merge the pull request

The merge method is a project policy decision:

```bash
gh pr merge --squash
# or
gh pr merge --rebase
# or
gh pr merge --merge
```

- `--squash` makes one integration commit; use it when the PR's individual commits are implementation notes and the project wants a compact mainline.
- `--rebase` preserves each commit without a merge commit; use it when commits are already coherent and the project values linear ancestry.
- `--merge` creates a merge commit; use it when preserving the branch topology or the fact of a grouped integration matters.

The repository's branch protection, required checks, review rules, and permissions can override what the command is allowed to do. Inspect `gh pr checks` before merging.

## When not to fork

If you own the project and are the maintainer, create an ordinary repository. A GitHub fork is a contribution relationship, not a replacement for normal ownership. Use a second remote or a second clone when you need a reference copy; do not create a fork of your own project just to get an `origin` name.

## Further reading

- [GitHub CLI Manual](10-references.md#9-github-specific-workflow-touchpoints)
- [GitHub Docs: About Pull Requests](10-references.md#9-github-specific-workflow-touchpoints)
- [GitHub Docs: Workflow Events (Git triggers)](10-references.md#9-github-specific-workflow-touchpoints)

## Sources

- [GitHub CLI: gh repo fork](https://cli.github.com/manual/gh_repo_fork), access date 2026-11-02
- [GitHub CLI: gh repo clone](https://cli.github.com/manual/gh_repo_clone), access date 2026-11-02
- [GitHub CLI: gh pr create](https://cli.github.com/manual/gh_pr_create), access date 2026-11-02
- [GitHub CLI: gh pr checks](https://cli.github.com/manual/gh_pr_checks), access date 2026-11-02
- [GitHub CLI: gh pr merge](https://cli.github.com/manual/gh_pr_merge), access date 2026-11-02
- [GitHub Docs: About pull requests](https://docs.github.com/en/pull-requests/reference/pull-requests), access date 2026-11-02
