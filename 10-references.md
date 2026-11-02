# 10. References

**Question:** Where should I go next when I need a deeper explanation, a compact reference, a recovery path, or an authoritative source?

This chapter is a curated directory for the subjects covered in this playbook. Each section starts with the question it answers, then lists resources with the verified metadata that explains why they are here.

## 1. Mental Model and Internals

**Question:** Which resources explain Git's objects, references, index, and graph before the command syntax?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [Git from the Inside Out](https://codewords.recurse.com/issues/two/git-from-the-inside-out) | https://codewords.recurse.com/issues/two/git-from-the-inside-out | Web essay / Interactive guide | Blobs, trees, commits, refs, index, merge graph reconstruction | Builds a mental model by walking through Git's implementation step by step in JavaScript. | 2.2k (repo: maryrosecook/gitlet) | 2026 (site live) | MIT (code) / All rights reserved (text) | Yes |
| [Think Like (a) Git](https://think-like-a-git.net/) | https://think-like-a-git.net/ | Web guide / book | Graph theory, reachability, detached HEAD, rebasing as graph manipulation | Teaches the underlying graph concepts before command syntax. Timeless foundational reading. | N/A (independent) | 2026 (site live) | CC BY-NC-SA 3.0 | Yes |
| [Git for Ages 4 and Up](https://www.youtube.com/watch?v=1ffBJ4sVUb4) | https://www.youtube.com/watch?v=1ffBJ4sVUb4 | Video lecture (Michael Schwern) | Object store, DAG, pointers, branching, merging, rebasing | Uses physical Tinkertoys to explain Git's data structure without command jargon. Unmatched physical intuition. | N/A (Linux.conf.au) | 2013 (frozen classic) | CC-BY | Yes |
| [Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/) | https://jwiegley.github.io/git-from-the-bottom-up/ | Web book / PDF (John Wiegley) | Directed acyclic graphs, repository anatomy, .git layout, index vs working tree | A rigorous Unix-philosophy exploration of Git's data structures for systems developers. | 892 (repo: jwiegley/git-from-the-bottom-up) | 2025-08 | CC BY 4.0 | Yes |

## 2. Cheat Sheets

**Question:** Which compact references help me choose or recall a common Git command quickly?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [GitHub Git Cheat Sheet](https://training.github.com/downloads/github-git-cheat-sheet.pdf) | https://training.github.com/downloads/github-git-cheat-sheet.pdf | 2-page printable PDF | Setup, setup repo, staging, committing, branching, merging, history rewrite | The authoritative, official 2-page GitHub reference sheet translated into over 20 languages. | 5.1k (repo: github/training-kit) | 2026-09 | CC-BY-4.0 | Yes |
| [Atlassian Git Cheat Sheet](https://www.atlassian.com/dam/jcr:e7e22f25-bba2-4ef1-a197-53f46b6df4a5/SWTM-2088_Atlassian-Git-Cheatsheet.pdf) | https://www.atlassian.com/dam/jcr:e7e22f25-bba2-4ef1-a197-53f46b6df4a5/SWTM-2088_Atlassian-Git-Cheatsheet.pdf | 2-page printable PDF | Everyday workflow, tracking changes, comparing, branching, undoing mistakes | Clear typography and logical progression from basic setup to undo commands. | N/A | Current | Free download | Yes |
| [Tower Git Cheat Sheet](https://www.git-tower.com/learn/cheat-sheets/git) | https://www.git-tower.com/learn/cheat-sheets/git | Printable PDF & Web guide | Basic commands, branch handling, remote repos, diff inspection, reflog | Clean design with clear syntax notes and quick visual indicators. | N/A | Current | Free download | Yes |

## 3. Recovery and Mistakes ("I broke my repo")

**Question:** Which resources help me recover committed work and choose a safe response after a Git mistake?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [git-flight-rules](https://github.com/k88hudson/git-flight-rules) | https://github.com/k88hudson/git-flight-rules | Markdown reference repository | Exhaustive symptom-based decision trees for broken merges, accidental hard resets, lost stashes, rebase panics | The undisputed encyclopedia of Git troubleshooting. Organized by "What did I do wrong?". | 42.6k | 2026-10 | CC BY-SA 4.0 | Yes |
| [Oh Shit, Git!?! / Dangit, Git!](https://ohshitgit.com) | https://ohshitgit.com | Web guide | Top 10 emergency mistakes: wrong commit message, committed to wrong branch, accidentally reset | Conversational, panic-reducing triage for everyday slip-ups with clean command recipes. | 1.5k+ (repo: ksylor/ohshitgit) | 2026-01 | All rights reserved (free web) | Yes |
| [How to undo (almost) anything with Git](https://github.blog/open-source/git/how-to-undo-almost-anything-with-git/) | https://github.blog/open-source/git/how-to-undo-almost-anything-with-git/ | Technical article (GitHub Blog) | Mechanics of soft/mixed/hard `git reset`, `git revert`, and recovering unreferenced commits with `git reflog` | Focuses on explaining the mechanical difference between reset and revert so developers choose safely. | N/A | Current | Open web | Yes |

## 4. Remotes, Forks, Triangular Workflows, Pull Requests

**Question:** Which resources explain fork-based collaboration, triangular remotes, and the pull-request workflow?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [first-contributions](https://github.com/firstcontributions/first-contributions) | https://github.com/firstcontributions/first-contributions | Interactive repository | Hands-on walkthrough: fork, clone, branch, edit, push to origin, submit PR upstream | Zero-friction real-world drill for the fork-and-pull model used across open source. | 56.7k | 2026-11 (active) | MIT | Yes |
| [How the GitHub CLI can now enable triangular workflows](https://github.blog/open-source/git/how-the-github-cli-can-now-enable-triangular-workflows/) | https://github.blog/open-source/git/how-the-github-cli-can-now-enable-triangular-workflows/ | Technical article (GitHub Blog) | Triangular workflow mechanics, `remote.pushDefault`, `origin` (fork) vs `upstream` (canonical) | Authoritative explanation of how Git models triangular remotes without fragile `pushurl` hacks. | N/A | Current | Open web | Yes |
| [Atlassian: Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow) | https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow | Tutorial & architectural diagrams | Centralized vs Feature Branch vs Gitflow vs Forking workflows | Best visual side-by-side comparison of team branching and fork governance strategies. | N/A | Current | Free web | Yes |

## 5. Worktrees and Parallel Work

**Question:** Which resources cover linked worktree lifecycle, layout, locking, and shared repository safety?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [GitWorktree.org](https://www.gitworktree.org/) | https://www.gitworktree.org/ | Dedicated documentation portal | `git worktree add`, `list`, `remove`, `prune`, directory layout conventions, AI coding agents | The only modern, dedicated reference portal focused exclusively on worktree lifecycle, locking, and multi-agent development. | N/A | Current | Free web | Yes |
| [Git Worktree Reference (Official)](https://git-scm.com/docs/git-worktree) | https://git-scm.com/docs/git-worktree | First-party reference manual | Internal `.git/worktrees/` structure, administrative lock files, branch mutual exclusion rules | Essential primary source for the safety guarantees and edge cases of shared object stores. | N/A | 2026 (Git 2.45+) | GPLv2 | Yes |

## 6. History Rewriting: Rebase, Interactive Rebase, Fixup, Force-With-Lease

**Question:** Which resources explain how to rewrite history deliberately and synchronize the rewritten result safely?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [git rebase: what can go wrong?](https://jvns.ca/blog/2023/11/06/rebasing-what-can-go-wrong-/) | https://jvns.ca/blog/2023/11/06/rebasing-what-can-go-wrong-/ | Illustrated technical essay (Julia Evans) | Rebase mechanics, conflict loops during multi-commit rebases, SHA churn, recovery strategies | Clarifies the exact failure points of rebase with visual clarity, making mid-rebase panics solvable. | N/A | 2023-11 | Free web | Yes |
| [Thoughtbot: Git Interactive Rebase, Squash, Amend](https://thoughtbot.com/blog/git-interactive-rebase-squash-amend-rewriting-history) | https://thoughtbot.com/blog/git-interactive-rebase-squash-amend-rewriting-history | Practical workflow guide | `git rebase -i`, `pick`, `squash`, `fixup`, `edit`, `exec`, commit splitting | A production-grade standard for curating atomic commits prior to code review. | N/A | Current | Free web | Yes |
| [So You Think You Know Git](https://www.youtube.com/watch?v=aolI_Rz0ZqY) | https://www.youtube.com/watch?v=aolI_Rz0ZqY | Video lecture (Scott Chacon, FOSDEM 2024) | Modern rebase (`--update-refs`), `range-diff`, `--force-with-lease`, `includeIf`, `git maintenance` | Essential lecture by the Pro Git author updating developers on modern Git capabilities (2.23-2.44+). | N/A | 2024-02 | CC-BY | Yes |

## 7. Interactive Practice and Games

**Question:** Where can I practice Git graph operations and collaboration in a safe environment?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [Learn Git Branching](https://learngitbranching.js.org) | https://learngitbranching.js.org | Browser interactive sandbox | Commits, branching, merges, interactive rebase, cherry-pick, remote tracking | The gold standard in visual Git education. Real-time SVG graph updates on every command. | 34.2k (repo: pcottle/learnGitBranching) | 2026-10 | MIT | Yes |
| [Git Katas](https://github.com/eficode-academy/git-katas) | https://github.com/eficode-academy/git-katas | CLI exercises (local repo) | Terminal exercises: submodules, bisect, reflog, rerere, merge conflict drills | Runs in your *real* local terminal using automated setup scripts, building real muscle memory. | 1.68k | 2026-01 | MIT | Yes |
| [GitHub Skills](https://skills.github.com/) | https://skills.github.com/ | Interactive GitHub courses | PR reviews, conflict resolution, GitHub Actions CI, branch protections | Replaces the deprecated Learning Lab; runs inside real GitHub repositories with automated bot feedback. | Multi-repo (github/skills) | Current | CC-BY-4.0 / MIT | Yes |

## 8. Books and Long-Form References

**Question:** Which books and long-form references are worth keeping for a complete or visual treatment of Git?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [Pro Git (2nd Edition)](https://git-scm.com/book/en/v2) | https://git-scm.com/book/en/v2 | Free online / Paid print (Scott Chacon, Ben Straub) | Complete Git coverage: plumbing vs porcelain, branching, distributed workflows, internals | The definitive standard manual for Git worldwide. Hosted on git-scm.com. | 6.6k (repo: progit/progit2) | 2026-05 | CC BY-NC-SA 3.0 | Yes |
| [Version Control with Git (3rd Edition, 2022)](https://www.oreilly.com/library/view/version-control-with/9781492091189/) | https://www.oreilly.com/library/view/version-control-with/9781492091189/ | Paid book (O'Reilly, Jon Loeliger & Prem Kumar Ponuthorai) | In-depth architecture, advanced branching/merging, hooks, submodules, server config | The most comprehensive commercial reference book, fully updated for modern workflows. | N/A | 2022-10 | Commercial | Yes |
| [How Git Works](https://wizardzines.com/zines/git/) | https://wizardzines.com/zines/git/ | Paid visual zine ($12, Julia Evans) | Commits as snapshots, branches as pointers, HEAD, reflog, staging area | Unmatched visual distillation; perfect for engineers who learn best from hand-drawn schemas. | N/A | Current | Commercial | Yes |

## 9. GitHub-Specific (Workflow Touchpoints)

**Question:** Which official and practical resources explain GitHub CLI commands, pull requests, and workflow events?

| Resource | URL | Format | What it covers | Why this one | Stars | Last push | License | Verified |
|---|---|---|---|---|---|---|---|:---:|
| [GitHub CLI Manual](https://cli.github.com/manual) | https://cli.github.com/manual | Official documentation | `gh pr create/checkout/diff`, `gh repo fork --remote`, `gh issue develop` | The fastest bridge between local Git work and GitHub primitives without context-switching to a browser. | 39.5k (repo: cli/cli) | Current | MIT | Yes |
| [GitHub Docs: About Pull Requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) | https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests | Official documentation | Branch comparisons, draft PRs, review workflows, merge methods (merge, squash, rebase) | Definitive specification of how GitHub evaluates Git DAG relationships during PR merges. | N/A | Current | CC-BY-4.0 | Yes |
| [GitHub Docs: Workflow Events (Git triggers)](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows) | https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows | Official documentation | `push`, `pull_request`, `pull_request_target`, `release` triggers | Crucial for understanding how Git push events invoke automated CI/CD workflows and security contexts. | N/A | Current | CC-BY-4.0 | Yes |

## 10. Dead or Avoid

**Question:** Which older, broken, or counter-productive resources should I avoid despite appearing in old lists?

Resources that are broken, unmaintained, or counter-productive.

| Resource | Canonical URL / Repo | Status | Reason to Avoid / Evidence |
|---|---|:---:|---|
| **git-it** | `jlord/git-it-electron` | Dead | Built on obsolete Electron 1.x; fails to run on modern operating systems due to deprecated native Node bindings. Superseded by Learn Git Branching and GitHub Skills. |
| **git-game** | `hgarc014/git-game` | Abandoned | Inactive since 2015. Contains broken challenge steps and lacks modern command syntax (`switch`/`restore`). Use *Git Katas*. |
| **Explain Git with D3** | `onlywei/explain-git-with-d3` | Frozen (2015) | Static visual playground without interactive levels or remote support. Entirely superseded by *Learn Git Branching*. |
| **git-tips** | `git-tips/tips` | Avoid | Unstructured link farm of 170+ one-liners. Promotes dangerous force commands (`git push -f`) without safety checks or recovery instructions. |
| **GitHub Learning Lab** | `lab.github.com` | Retired | Officially deprecated and shut down on September 1, 2022. All interactive courses migrated to *GitHub Skills*. |
| **PeepCode Git Internals PDF** | Defunct | Obsolete | 2008 publication by Scott Chacon. Domain is defunct; fully superseded by Chapter 10 of *Pro Git v2*. |

## 11. First-Party Sources

**Question:** Which official manuals should be the baseline when a third-party guide and local practice disagree?

Baseline official documentation to keep bookmarked.

1. **[Git Documentation (git-scm.com/docs)](https://git-scm.com/docs):** The authoritative manual pages for all commands, release notes, and configuration options.
2. **[Git Glossary](https://git-scm.com/docs/gitglossary):** The canonical definitions of Git concepts (tree-ish, commit-ish, refspec, fast-forward).
3. **[GitHub CLI (gh) Manual](https://cli.github.com/manual):** Complete reference for command-line GitHub operations.
4. **[GitHub Docs](https://docs.github.com):** The primary documentation for GitHub-hosted collaboration, permissions, and CI/CD triggers.
