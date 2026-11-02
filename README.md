# Git Playbook

This is a field guide and reference directory for humans who need to make safe Git decisions, not memorize isolated commands. Read it in order once; return to the command chapter, cheat sheet, and references during daily work.

All command examples were exercised in a fresh temporary repository with Git 2.47.3 and a local bare remote. Paths and identities in output are sanitized. Convention claims are linked to primary documentation; access date: 2026-11-02.

## Reading order

1. Build the object model before changing history.
2. Understand remotes before pushing or synchronizing a fork.
3. Learn the daily commands by intent.
4. Use the contribution loop for forks and pull requests.
5. Write executable issues.
6. Use worktrees for parallel branches.
7. Recover with reflog before attempting salvage.
8. Put repositories and worktrees in a layout that prevents duplication.
9. Keep the one-page tables nearby.
10. Use the curated references for deeper study and authoritative follow-up.

## Chapters

| Chapter | Question it answers |
| --- | --- |
| [01: Mental model](01-mental-model.md) | What does Git actually point at when I edit, stage, commit, switch, or push? |
| [02: Remotes](02-remotes.md) | Which repository and ref am I fetching from or pushing to? |
| [03: Daily commands](03-daily-commands.md) | Which command matches the intent I have right now? |
| [04: Forks and pull requests](04-forks-and-pull-requests.md) | How do I contribute without confusing my fork, the upstream project, and my local branch? |
| [05: Issues](05-issues.md) | What makes a ticket executable by a stranger or an agent? |
| [06: Worktrees](06-worktrees.md) | How do I work on several branches without stashing or cloning repeatedly? |
| [07: Recovery](07-recovery.md) | How do I get back after a bad reset, deleted branch, detached commit, or interrupted rebase? |
| [08: Layout and hygiene](08-layout-and-hygiene.md) | How should repositories, forks, reference clones, and worktrees live on disk? |
| [09: Cheatsheet](09-cheatsheet.md) | What is the shortest safe reference for common intent? |
| [10: References](10-references.md) | Where should I go next for deeper explanations, practice, recovery, and first-party documentation? |

## Verification boundary

The guide proves Git command groups against a scratch repository and a second local bare remote. GitHub CLI syntax was checked with the installed CLI's help output. **UNVERIFIED locally:** GitHub-hosted review, merge, and issue state depend on permissions, branch protection, repository settings, and network state; those behaviours are not claimed as locally reproduced.

## Sources

- [Git command documentation](https://git-scm.com/docs)
- [GitHub CLI manual](https://cli.github.com/manual/)
- [GitHub Docs: About permissions and visibility of forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/about-permissions-and-visibility-of-forks)
- [GitHub Docs: Syncing a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)
- [GitHub Docs: About pull requests](https://docs.github.com/en/pull-requests/reference/pull-requests)
