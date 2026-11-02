# 06. Worktrees

**Question:** How do I work on several branches without stashing or cloning repeatedly?

A linked worktree is another checked-out directory backed by the same repository object database. Each worktree has its own `HEAD`, index, and working files; refs and objects are shared. That makes parallel work explicit instead of hiding it in a stash.

## Add and inspect

Keep worktrees in a predictable location:

```text
<repo>/.worktrees/<branch>
```

or use a managed sibling root such as `<root>/worktrees/<owner>/<repo>/<branch>`. Do not put an ordinary clone inside the working tree of another repository; see [08: Layout and hygiene](08-layout-and-hygiene.md).

```bash
mkdir -p .worktrees
git worktree add -b docs/remote-model .worktrees/docs-remote-model main
git worktree list
git worktree remove .worktrees/docs-remote-model
```

The scratch run printed:

```text
$SCRATCH/repo        c711f40 [main]
$SCRATCH/worktree-a  c711f40 [worktree-branch]
```

Use `git worktree list --porcelain` when another tool needs stable field names.

## Why worktrees beat stashing

A stash is a temporary object with a recovery policy you must remember. A worktree keeps the branch checked out, visible, and independently testable. Use a worktree when two tasks must exist at the same time, when a long-running check needs a clean tree, or when switching branches would risk carrying edits across tasks. Use a stash for a short interruption, not as a parallel workspace.

## `.git` file versus directory

The main checkout normally has a `.git/` directory. A linked worktree has a small `.git` file whose contents point to administrative data under the main repository's `.git/worktrees/` directory. The worktree's Git commands follow that indirection; do not replace the file with a copied directory.

## Repair and prune

```bash
git worktree repair .worktrees/docs-remote-model
git worktree prune -v
git worktree list
```

`repair` updates administrative paths after a worktree or the main repository moved. The scratch run moved a worktree directory and `repair` printed:

```text
repair: gitdir incorrect: $SCRATCH/repo/.git/worktrees/moved-worktree/gitdir
```

After repair, `worktree list` showed the renamed path with `[moved-branch]`. The command can repair the association even when it reports the stale path it corrected.

`prune` removes stale registrations when a linked directory is gone. The scratch run printed:

```text
Removing worktrees/stale-worktree: gitdir file points to non-existent location
```

Run `git worktree prune --dry-run` first when the registration might still be needed.

## Failure modes

| Failure | Why it happens | First response |
| --- | --- | --- |
| Main repository moved | Administrative links still contain the old path. | Run `git worktree repair` from the moved main repository. |
| Worktree directory deleted | The registration remains under `.git/worktrees`. | Confirm it is really gone, then run `git worktree prune -v`. |
| Stale registration | A failed cleanup or manual deletion left metadata. | Inspect `git worktree list`, then prune. |
| Same branch twice | Git protects a branch from two writable checkouts. | Use another branch or a detached worktree; do not bypass the guard casually. |
| Worktree removed while dirty | `remove` protects local edits. | Inspect and commit, stash, or explicitly remove only when the data is disposable. |

**WARNING:** `git worktree remove -f` and deleting a worktree directory can destroy uncommitted tracked work. Use `list`, inspect status, and preserve work before cleanup.

## Sources

- [git-worktree](https://git-scm.com/docs/git-worktree), access date 2026-11-02
- [gitrepository-layout](https://git-scm.com/docs/gitrepository-layout), access date 2026-11-02
