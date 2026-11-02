# 02. Remotes

**Question:** Which repository and ref am I fetching from or pushing to?

A remote is a named URL plus refspecs. `origin` is only a convention: it usually names the repository you cloned. `upstream` is another convention for the project you contribute to. Any name works if it is clear.

## A fork clone needs two remotes

For a contribution to a project you do not own:

- `origin` points to your fork, where you can push branches.
- `upstream` points to the canonical project, where you fetch changes.
- `main` is your local branch.
- `origin/main` and `upstream/main` are local records of the two servers' branches.

A personal repository that you own should not be a GitHub fork just to support this workflow. Use an ordinary repository and add whatever reference remote you need.

## Inspect and edit remote names

```bash
git remote -v
git remote add upstream OWNER/PROJECT.git
git remote set-url upstream OWNER/PROJECT.git
git remote rename upstream canonical
```

These commands were exercised against the local bare remote. The verified output was:

```text
mirror  $SCRATCH/supplement-remote.git (fetch)
mirror  $SCRATCH/supplement-remote.git (push)
origin  $SCRATCH/supplement-remote.git (fetch)
origin  $SCRATCH/supplement-remote.git (push)
```

`set-url` changes a name's URL. `rename` changes the name and updates related configuration. Use `git remote get-url --all <name>` when you need the exact configured URL without printing fetch/push labels.

## Fetch, pull, and push

Why first:

- `fetch` downloads objects and moves remote-tracking refs. It does not change your current branch or working tree.
- `pull` runs fetch and then integrates the fetched branch according to configuration or explicit options. It can merge or rebase; inspect before using it on a dirty or shared branch.
- `push` asks the remote to update its refs from your local refs.

```bash
git fetch origin
git pull --ff-only origin main
git push -u origin topic/my-change
```

The scratch run proved both the local transport and the safety option:

```text
$ git -C $SCRATCH/supplement pull --ff-only
Already up to date.
```

Prefer an explicit integration strategy for important branches, such as `git pull --ff-only`, rather than accepting an accidental merge.

## `origin/main` is not `main`

`main` is a local branch ref. `origin/main` is a remote-tracking ref stored locally after fetch. It is a remembered remote state, not a second checkout and not a branch you should commit on.

```bash
git fetch origin
git log --oneline main..origin/main
git switch main
git merge --ff-only origin/main
```

If `origin/main` is ahead, the last command updates your local `main`. If it is not an ancestor, `--ff-only` stops instead of inventing a merge.

## Triangular workflows

A triangular workflow fetches from one place and pushes to another. Configuration makes the intent visible:

```bash
git config remote.pushDefault origin
git config branch.main.pushRemote origin
git config branch.topic.pushRemote origin
```

The scratch run printed:

```text
origin
origin
```

Those are the values returned by `git config --get remote.pushDefault` and `git config --get branch.main.pushRemote`. The branch-specific setting wins for that branch; the default applies when no branch-specific push remote exists. Keep the fetch remote and push remote conceptually separate even when both URLs happen to be the same.

## Force updates

A normal push refuses to move a remote branch backward. A force push replaces the remote ref and can discard commits other people have fetched.

**WARNING:** `git push --force` can erase other people's remote commits with no check. Treat it as unacceptable for collaborative branches.

Use the lease form after a history rewrite:

```bash
git fetch origin topic/my-change
git push --force-with-lease origin topic/my-change
```

`--force-with-lease` checks that the remote ref still has the value you believe it has. In the scratch run, another clone advanced `rewrite`, so the stale local push was rejected:

```text
! [rejected]        rewrite -> rewrite (stale info)
error: failed to push some refs to '$SCRATCH/remote.git'
```

After fetching the remote change and deliberately rebuilding the local rewrite, the same lease form succeeded:

```text
c8b5735..257b2c8  rewrite -> rewrite
```

Never replace the lease with `--force` merely to make a rejection disappear. Fetch, inspect, rebase or coordinate, then retry with the lease.

## Further reading

- [first-contributions](10-references.md#4-remotes-forks-triangular-workflows-pull-requests)
- [How the GitHub CLI can now enable triangular workflows](10-references.md#4-remotes-forks-triangular-workflows-pull-requests)
- [Git Documentation (git-scm.com/docs)](10-references.md#11-first-party-sources)

## Sources

- [git-remote](https://git-scm.com/docs/git-remote), access date 2026-11-02
- [git-fetch](https://git-scm.com/docs/git-fetch), access date 2026-11-02
- [git-pull](https://git-scm.com/docs/git-pull), access date 2026-11-02
- [git-push](https://git-scm.com/docs/git-push), access date 2026-11-02
- [GitHub Docs: Syncing a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork), access date 2026-11-02
