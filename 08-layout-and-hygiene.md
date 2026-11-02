# 08. Layout and hygiene

**Question:** How should repositories, forks, reference clones, and worktrees live on disk?

A predictable tree prevents accidental nested repositories, duplicate upstream clones, and worktree metadata that nobody knows how to repair.

## Two useful layouts

The identity layout used by `ghq` is:

```text
<root>/<host>/<owner>/<repo>
```

For example, a sanitized shape is:

```text
src/github.com/example/project
```

It makes host and owner identity obvious and scales across many repositories.

A role-based layout groups by what the checkout is for:

```text
projects/owned/project
contrib/upstream/project
references/upstream/project
worktrees/project/fix-short-symptom
```

Use one system consistently. A mixed tree is fine only when the role is explicit and searchable.

## Forks, reference clones, and own projects

| Kind | Purpose | Remote pattern |
| --- | --- | --- |
| Own project | The repository you maintain. | `origin` is the canonical project. |
| Fork clone | Contribution from a separate owner. | `origin` is your fork; `upstream` is the canonical project. |
| Reference clone | Read-only inspection, bisect, or comparison. | One remote; do not create a second copy merely to get another branch. |
| Worktree | Parallel checkout of one local repository. | Shared object database; one branch per worktree. |

## The triangular single-clone rule

For one upstream project, keep one writable clone per owner identity and use `origin` plus `upstream` for the fork workflow. Add worktrees for simultaneous branches. Clone a second copy only when isolation is intentional, such as a separate credential boundary, a disposable benchmark, or a truly independent object store.

Do not make four copies of the same upstream because four tasks exist. Four worktrees make the branch ownership visible; four clones make remotes, fetch state, and cleanup ambiguous.

## Never nest repositories

A repository inside another repository's working tree is easy to misread and easy to omit from the parent commit. The parent records only the nested directory as untracked or ignored content; it does not automatically include the child's history.

**WARNING:** Do not place `<child>/.git` below `<parent>` unless you deliberately manage a submodule or another supported boundary. Put independent repositories beside each other under a shared root.

Place linked worktrees either under `<repo>/.worktrees/<branch>` with an explicit ignore rule, or under a managed root outside the repository. See [06: Worktrees](06-worktrees.md).

## Sanitized case study

A real working tree was reviewed after drift accumulated. Personal names and paths are omitted. The inventory found:

- 101 repositories and linked worktrees in one broad tree.
- Four upstream projects cloned twice, despite each task being branch-isolated.
- One orphan worktree registration whose directory no longer existed.
- One independent clone nested inside another project.

The cleanup plan was deliberately boring: inventory remotes, choose the canonical clone for each upstream, move parallel tasks into named worktrees, run `git worktree prune` only after confirming the directories were gone, and move the nested clone beside the parent. The lesson is not that 101 is a magic limit; it is that duplication hides ownership and makes recovery less reliable.

## Further reading

- [Git from the Bottom Up](10-references.md#1-mental-model-and-internals)
- [GitWorktree.org](10-references.md#5-worktrees-and-parallel-work)
- [Git Worktree Reference (Official)](10-references.md#5-worktrees-and-parallel-work)

## Sources

- [ghq](https://github.com/x-motemen/ghq), access date 2026-11-02
- [git-worktree](https://git-scm.com/docs/git-worktree), access date 2026-11-02
- [Git submodule documentation](https://git-scm.com/book/en/v2/Git-Tools-Submodules), access date 2026-11-02
