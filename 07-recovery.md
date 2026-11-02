# 07. Recovery

**Question:** How do I get back after a bad reset, deleted branch, detached commit, or interrupted rebase?

**Rule:** run `git reflog` first, always. Reflog records where local refs and `HEAD` used to point. It is the fastest map back to committed work that no branch currently names.

## Recovery protocol

1. Stop mutating the repository.
2. Copy or archive the working tree if uncommitted bytes may matter.
3. Run `git status -sb` and `git reflog --all --oneline`.
4. Identify the last good commit by message, time, and graph.
5. Create a rescue ref before experimenting.
6. Only then reset, rebase, or clean up.

## Lost commit or deleted branch

The scratch run made `recoverable` at `5629f61`, deleted its branch, and recreated it:

```bash
git reflog --all --oneline
git branch rescue/recoverable 5629f61
git switch rescue/recoverable
git show --stat --oneline HEAD
```

The important action is creating a new ref. Do not use the recovered SHA as an unlabelled temporary value and then switch away.

## Bad `reset --hard`

The scratch run committed `hard-reset-target` at `324b46a`, ran `reset --hard HEAD^`, then recovered it:

```bash
git reflog --oneline -5
git reset --hard 324b46a
git status -sb
```

The reflog showed:

```text
c711f40 HEAD@{0}: reset: moving to HEAD^
324b46a HEAD@{1}: commit: hard-reset-target
```

**WARNING:** This restores committed content. It cannot restore an uncommitted tracked edit that was discarded by `reset --hard`.

## Interrupted rebase

If a rebase stopped on a conflict, inspect the state before choosing a path:

```bash
git status -sb
git rebase --show-current-patch
git rebase --abort
```

The scratch run produced `## HEAD (no branch)` and `AA conflict.txt`, then `git rebase --abort` returned the branch to `rebase-conflict-topic`.

If the intended resolution is known, use the other valid path:

```bash
# edit the conflicted files
git add path/to/resolved-file
git rebase --continue
```

Use `--skip` only when the entire replayed commit is no longer needed. Use `--quit` only when you intentionally want to leave the current files and stop rebase bookkeeping.

## Detached `HEAD` with useful work

The scratch run committed `detached-work` at `601467b` while detached, then named it:

```bash
git switch --detach <commit>
# edit files
git add -A
git commit -m "detached work"
git switch -c rescue/detached-work 601467b
git switch main
```

If you have already switched away, find the commit with `git reflog --all --oneline`, then create the branch at that SHA.

## `fsck --lost-found`

Use this after reflog when an object is no longer named by a ref:

```bash
git fsck --lost-found
git show <dangling-commit>
git branch rescue/fsck <dangling-commit>
```

The scratch run printed:

```text
dangling commit e30064b432affa6a37aeb93fc958126aad7cea39
```

`git show --oneline e30064b...` proved it was `fsck-lost`. `fsck --lost-found` also writes salvageable objects under `.git/lost-found/`; inspect before naming anything.

## Stash rescue

A stash is a ref until it is dropped or expires. Inspect it before applying:

```bash
git stash list
git stash show -p stash@{0}
git stash branch rescue/stash stash@{0}
```

The scratch run proved the first two steps with `stash@{0}: On main: demo stash`, then restored the work with `git stash pop`. Prefer `stash branch` when the work is substantial or may conflict: it creates a branch from the stash's original base and applies the changes there.

## What is unrecoverable

- Bytes never committed, stashed, or saved elsewhere after `restore` or a destructive reset.
- Ignored files that were never tracked or copied.
- Objects removed by garbage collection after reflog and other reachability records expired.
- A remote force-push that other clones did not retain, once every local copy and reflog has expired.

Recovery is not a substitute for a backup. Reflog is local, expires, and is not transferred by push.

## Sources

- [git-reflog](https://git-scm.com/docs/git-reflog), access date 2026-11-02
- [git-fsck](https://git-scm.com/docs/git-fsck), access date 2026-11-02
- [git-stash](https://git-scm.com/docs/git-stash), access date 2026-11-02
- [git-rebase](https://git-scm.com/docs/git-rebase), access date 2026-11-02
