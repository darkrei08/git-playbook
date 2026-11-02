# 01. Mental model

**Question:** What does Git actually point at when I edit, stage, commit, switch, or push?

Git is a content-addressed object database plus movable names. The safe mental model is to ask which of four places changed: the working tree, the index, a commit graph, or a ref.

## The object graph

```text
working tree --git add--> index --git commit--> commit object
     ^                       |                    |
     |                       v                    v
 git restore            HEAD -> branch ref -> parent commit
                                               ^
                                               |
                                  tag / origin/main / other refs
```

A clone has its own object database and refs. A fork is a GitHub-hosted repository relationship. Neither changes the meaning of a local commit.

## Concepts with exercised examples

The outputs below come from one scratch run. `$SCRATCH` replaces the temporary directory; hashes and messages are real from that run.

| Concept | Human meaning | Worked command and real output |
| --- | --- | --- |
| Working tree | Files currently checked out and editable. | `git -C $SCRATCH/supplement status -sb` after editing showed `## onto-topic` and `?? scratch.txt`. |
| Index | The proposed next commit. | `git -C $SCRATCH/supplement diff --staged` showed the staged replacement of `alpha` with `alpha changed`; plain `diff` was empty. |
| Commit | An immutable snapshot plus parent(s), author, message, and metadata. | `git -C $SCRATCH/repo show --oneline e30064b` printed `e30064b fsck-lost` and the new `fsck.txt`. |
| Branch | A movable local ref naming one commit. | `git -C $SCRATCH/repo switch -c feature` printed `Switched to a new branch 'feature'`; after merge, `git log` printed `c711f40 (HEAD -> main) Merge feature`. |
| Ref | A name resolving to an object, such as a branch, tag, or remote-tracking branch. | `git -C $SCRATCH/repo branch -r` printed `origin/main`; `git tag` created `v1.0.0`. |
| Tag | A ref intended to mark a release or other stable point; annotated tags are objects with a message. | `git -C $SCRATCH/repo show --stat --oneline v1.0.0` printed `tag v1.0.0`, `Version 1.0.0`, and `c711f40 Merge feature`. |
| Remote-tracking ref | Your local record of a remote ref, updated by fetch. | After `push -u origin main`, `git branch -r` printed `origin/main`; it did not create a local branch named `origin/main`. |
| `HEAD` attached | `HEAD` points to a branch name, so a new commit moves the branch. | `git status -sb` printed `## main`. |
| `HEAD` detached | `HEAD` points directly to a commit; commits need a new branch ref if they must survive. | `git switch --detach HEAD` printed `HEAD is now at c711f40 Merge feature`; a rescue branch was then created at `601467b`. |
| Repository | The `.git` database and refs plus a working tree, if checked out. | `git init -b main $SCRATCH/repo` printed `Initialized empty Git repository in .../repo/.git/`. |
| Clone | A local repository copied from another repository, normally with a remote named `origin`. | `git clone $SCRATCH/remote.git $SCRATCH/other` printed `Cloning into .../other... done.` |
| Fork | A GitHub server-side repository owned by another account, related to an upstream repository. | `gh repo fork OWNER/PROJECT --clone=false` is the real CLI form; hosting permissions and network state decide its result. |
| Merge | A new commit combining histories when fast-forwarding is not selected. | `git merge --no-ff feature -m 'Merge feature'` printed `Merge made by the 'ort' strategy.` and created `c711f40`. |
| Rebase | Replays commits on a new base, creating new commit IDs. | `git rebase rebase-base` printed `Successfully rebased and updated refs/heads/topic.` and produced `05bc36c topic-change`. |
| Cherry-pick | Copies one existing change into the current branch as a new commit. | `git cherry-pick 213b7b0...` created `[cherry-target 213b7b0] picked-change`. |
| Revert | Adds a new commit that inverses an earlier commit; it does not move the branch backward. | `git revert --no-edit HEAD` created `[cherry-target 0f42c1a] Revert "picked-change"`. |

The same commit can be reachable through several refs. Deleting a branch deletes only that name; the commit may remain reachable through reflog or another ref. See [07: Recovery](07-recovery.md).

## Attached versus detached `HEAD`

Attached mode is the normal mode:

```text
HEAD -> refs/heads/main -> commit A
```

Detached mode is useful for inspection, bisect, and temporary experiments:

```text
HEAD -> commit A
refs/heads/main -> commit A
```

If you commit in detached mode, immediately name the commit:

```console
$ git switch --detach <commit>
$ git switch -c rescue/<short-purpose>
```

The second command is the safety step. Without it, later switching can leave the commit reachable only through reflog.

## Sources

- [gitrevisions](https://git-scm.com/docs/gitrevisions), access date 2026-11-02
- [gitglossary](https://git-scm.com/docs/gitglossary), access date 2026-11-02
- [git-commit](https://git-scm.com/docs/git-commit), access date 2026-11-02
- [git-switch](https://git-scm.com/docs/git-switch), access date 2026-11-02
