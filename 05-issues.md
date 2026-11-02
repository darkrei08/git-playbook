# 05. Issues

**Question:** What makes a ticket executable by a stranger or an agent?

An executable issue removes interpretation work. A reader should be able to reproduce the symptom, distinguish expected from actual behaviour, make the change, and prove the acceptance criteria without asking for hidden context.

## The issue contract

Use a declarative, symptom-specific title:

```text
Status command omits the active worktree when the branch is detached
```

Avoid titles such as `Fix Git stuff` or `Improve docs`. The title should state the observable outcome, not the proposed implementation.

Use this body shape:

```markdown
## Reproduction
1. Run the exact command.
2. Run the next exact command.

## Expected
The command prints the active worktree and branch state.

## Actual
The active worktree is missing from the output.

## Environment
- Git 2.47.3
- Linux
- repository commit: <full SHA>

## Acceptance criteria
- [ ] The reproduction no longer fails.
- [ ] `npm test` passes.
- [ ] `npm run lint` passes.

## Evidence
Paste the relevant command output, link the review, and state what was not run.
```

Exact commands matter. `status: approved` or a similar planning state can say that the scope is approved; it does not replace reproduction or acceptance criteria.

## Labels as contracts

A `documentation`, `bug`, or `enhancement` label classifies the work. A `ready-for-agent` style label is stronger: it promises that the issue is self-contained, locations are known, the environment is pinned, acceptance commands are exact, and the fix direction is bounded. Do not apply it to a vague request.

## GitHub CLI loop

The installed CLI help verified these command forms:

```bash
gh issue create --title "Status command omits the active worktree" --body-file issue.md
gh issue list --state open
gh issue view 123
gh issue comment 123 --body "Reproduced on Git 2.47.3."
gh issue close 123
gh pr checks 456
```

Use `--repo OWNER/REPO` when the current directory is not a clone of the target repository. Prefer `--body-file` for a multi-section issue so shell quoting does not change the evidence.

## Link history to work

Put `Fixes #123` in a pull request body when merging the PR should close the issue. Use `Refs #123` when the PR is related but should not close it. Keep the link in the body or commit message where the hosting service can parse it; do not rely on a comment that may be missed.

## Review evidence

Before asking for merge, run the exact checks in the issue and record both results and omissions:

```bash
gh pr checks 456
git status -sb
```

A green check list proves only the checks that ran. It does not prove a missing platform, a manual UI path, or a command that was never executed.

## Sources

- [GitHub CLI: gh issue create](https://cli.github.com/manual/gh_issue_create), access date 2026-11-02
- [GitHub CLI: gh issue list](https://cli.github.com/manual/gh_issue_list), access date 2026-11-02
- [GitHub CLI: gh issue view](https://cli.github.com/manual/gh_issue_view), access date 2026-11-02
- [GitHub CLI: gh issue comment](https://cli.github.com/manual/gh_issue_comment), access date 2026-11-02
- [GitHub CLI: gh issue close](https://cli.github.com/manual/gh_issue_close), access date 2026-11-02
- [GitHub CLI: gh pr checks](https://cli.github.com/manual/gh_pr_checks), access date 2026-11-02
- [GitHub Docs: Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue), access date 2026-11-02
