# 03. Daily commands

**Question:** Which command matches the intent I have right now?

Choose by the state you want next. Inspect before mutating. Commands marked destructive can remove tracked work or rewrite shared history.

## Inspect

| Command | What it does | Caveat |
| --- | --- | --- |
| `git status -sb` | Shows short status plus the current branch. | It does not show every ignored file. |
| `git diff` | Shows working-tree changes not staged. | It does not show staged changes. |
| `git diff --staged` | Shows the next commit's staged patch. | Review this before every commit. |
| `git log --oneline --graph --decorate` | Shows compact topology and refs. | Add `--all` when you need other local refs. |
| `git log -S'needle' --oneline` | Finds commits that add or remove a string. | It is textual, not semantic. |
| `git blame -L 1,20 path` | Attributes lines to commits. | Blame describes provenance, not fault. |
| `git show --stat --oneline HEAD` | Shows one object and its patch summary. | A tag may show tag metadata before the commit. |

The scratch run returned `d9cb235 needle` for `log -S`, line attribution from `blame`, and `d9cb235 needle` plus `file.txt | 1 +` for `show --stat`.

## Stage

Explain the boundary first: staging is selecting the next snapshot.

```bash
git add -p path/to/file
git add -A
git restore --staged path/to/file
```

`add -p` was exercised with a piped `y` response and staged only the displayed hunk. `add -A` then included the new file too.

**Caveat:** `git add -A` stages additions, modifications, and removals across the repository. Run it from the repository root when that is what you mean. It can stage a deletion you did not notice.

`restore --staged` removes a path from the index but keeps its working-tree edit. It is the readable replacement for the path form of `git reset`.

## Commit

```bash
git commit -m "describe the complete staged change"
git commit --amend -m "correct the last message or snapshot"
git commit --fixup=<commit>
GIT_SEQUENCE_EDITOR=true git rebase -i --autosquash <base>
```

`--amend` replaces the tip commit; it changes the commit ID. `--fixup` creates a commit whose message tells interactive rebase where to fold it. `--autosquash` moves fixup commits beside their target during an interactive rebase. The scratch run created `fixup! amended`, then autosquash reduced the visible history back to `amended`.

**WARNING:** Amend and autosquash rewrite commits. Do not rewrite commits other people already use unless the team explicitly coordinates it.

## Branch

```bash
git switch -c topic/name
git branch -d topic/name
git branch -D topic/name
```

`switch -c` creates and checks out a branch. `branch -d` refuses to delete a branch whose commits are not merged; the scratch run returned:

```text
error: the branch 'unmerged-delete' is not fully merged
hint: If you are sure you want to delete it, run 'git branch -D unmerged-delete'
```

`-D` overrides that protection.

**WARNING:** `branch -D` deletes the name without checking reachability. Recovery may still be possible through reflog, but do not treat it as a backup.

## Undo working-tree and index changes

```bash
git restore path/to/file
git restore --staged path/to/file
```

The first restores the working tree from the index. The second restores the index from `HEAD`. Both are path-scoped and leave commits alone.

**WARNING:** `git restore path/to/file` discards uncommitted tracked edits to that path. The discarded bytes are not guaranteed to be recoverable.

## Undo commits with reset

`reset` moves a ref and optionally changes the index and working tree. The target is commonly `HEAD^` or a named commit.

| Mode | Moves `HEAD`/branch | Index after reset | Tracked working tree after reset | What it destroys immediately |
| --- | --- | --- | --- | --- |
| `--soft` | Yes | Unchanged | Unchanged | Only the old branch position as the visible tip; changes remain staged. |
| `--mixed` | Yes | Matches target | Unchanged | Staging choices; working edits remain. This is the default. |
| `--hard` | Yes | Matches target | Matches target | Uncommitted tracked edits; files obstructing tracked paths can also be deleted. |

The scratch run showed `A  reset.txt` after soft, `?? reset.txt` after mixed, and the same untracked file after hard. That last detail matters: `--hard` does not generally remove unrelated untracked files, but it can remove untracked files or directories that obstruct writing tracked paths.

**WARNING:** `reset --hard` destroys uncommitted tracked content and moves branch history. Reflog can recover the old commit, not bytes that were never committed.

## Move work between histories

```bash
git stash push -m "short rescue label"
git stash pop
git cherry-pick <commit>
git revert <commit>
```

- `stash` stores local work outside the branch history so you can switch. Prefer a worktree for sustained parallel work; see [06: Worktrees](06-worktrees.md).
- `cherry-pick` copies a commit's change and creates a new commit. Resolve conflicts, then use `git cherry-pick --continue` or `--abort`.
- `revert` records the inverse on the current branch and is the safe public-history undo.

The scratch run printed `Saved working directory and index state On main: demo stash`, then restored it with `stash pop`; it also created `picked-change` and `Revert "picked-change"` as separate commits.

## History surgery

```bash
GIT_SEQUENCE_EDITOR=true git rebase -i <base>
GIT_SEQUENCE_EDITOR=true git rebase -i --autosquash <base>
git rebase --onto <new-base> <old-base> <branch>
```

Use interactive rebase to reorder, edit, squash, fixup, or drop commits before they are shared. Use `--onto` when the commits to replay are a slice after one base but should land on another. The scratch run moved `onto-topic` from `main` onto `onto-base` and printed `Successfully rebased and updated refs/heads/onto-topic.`

**WARNING:** Rebase creates new commit IDs. Do not rebase a branch other people have based work on unless they agree to the rewrite.

Do not rebase when the branch is already a shared integration history, when signed or audited commit identity must remain unchanged, or when a merge commit is part of the meaning you must preserve. Use merge or revert instead when preserving public ancestry is the priority.

## Sources

- [git-add](https://git-scm.com/docs/git-add), access date 2026-11-02
- [git-restore](https://git-scm.com/docs/git-restore), access date 2026-11-02
- [git-reset](https://git-scm.com/docs/git-reset), access date 2026-11-02
- [git-rebase](https://git-scm.com/docs/git-rebase), access date 2026-11-02
- [git-stash](https://git-scm.com/docs/git-stash), access date 2026-11-02
