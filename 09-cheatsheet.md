# 09. Cheatsheet

| Intent | Command | What it changes |
| --- | --- | --- |
| Inspect status | `git status -sb` | Reads working tree, index, and current branch. |
| Inspect unstaged work | `git diff` | Reads working-tree changes not in the index. |
| Inspect staged work | `git diff --staged` | Reads the proposed next commit. |
| Inspect graph | `git log --oneline --graph --decorate --all` | Reads reachable commits and refs. |
| Find a string change | `git log -S'needle' --oneline` | Searches commit snapshots for a changed string. |
| Explain a line | `git blame path` | Reads line provenance. |
| Inspect one object | `git show HEAD` | Reads an object and its patch. |
| Stage selected hunks | `git add -p` | Moves chosen working-tree hunks into the index. |
| Stage all changes | `git add -A` | Stages additions, edits, and removals. |
| Unstage a path | `git restore --staged path` | Copies `HEAD`'s path into the index. |
| Commit | `git commit -m "message"` | Creates a commit from the index. |
| Amend last commit | `git commit --amend` | Replaces the tip commit. |
| Create fixup | `git commit --fixup=<commit>` | Creates an autosquash-marked commit. |
| Autosquash | `git rebase -i --autosquash <base>` | Replays and folds fixups. |
| Create and switch | `git switch -c branch` | Creates a branch and attaches `HEAD`. |
| Delete merged branch | `git branch -d branch` | Deletes a safely merged branch ref. |
| Force-delete branch | `git branch -D branch` | Deletes the branch ref without merge protection. |
| Add remote | `git remote add name URL` | Adds a named remote. |
| Rename remote | `git remote rename old new` | Renames a remote and its config. |
| Fetch | `git fetch remote` | Downloads objects and updates remote-tracking refs. |
| Pull fast-forward only | `git pull --ff-only remote branch` | Fetches and integrates only by fast-forward. |
| Push a branch | `git push -u origin branch` | Updates the remote branch and records tracking. |
| Safe history rewrite | `git push --force-with-lease origin branch` | Replaces a remote ref only if its expected value still matches. |
| Discard tracked edit | `git restore path` | Replaces the working path from the index. |
| Move branch tip, keep staged | `git reset --soft target` | Moves `HEAD`; leaves index and worktree. |
| Move branch tip, unstage | `git reset --mixed target` | Moves `HEAD` and resets the index. |
| Move branch tip and files | `git reset --hard target` | Moves `HEAD`, index, and tracked worktree. |
| Save a short interruption | `git stash push -m "label"` | Stores local edits outside the branch. |
| Copy one commit | `git cherry-pick commit` | Creates a new commit with one change. |
| Public undo | `git revert commit` | Creates an inverse commit. |
| Read recovery map | `git reflog --all --oneline` | Reads recent local ref positions. |
| Find dangling objects | `git fsck --lost-found` | Reports and records unreachable objects. |
| Add worktree | `git worktree add -b branch path base` | Creates another checkout sharing the repository. |
| List worktrees | `git worktree list` | Reads registered checkouts and branches. |
| Remove worktree | `git worktree remove path` | Removes a linked checkout and registration. |
| Repair worktrees | `git worktree repair path` | Repairs moved administrative links. |
| Prune stale worktrees | `git worktree prune -v` | Deletes registrations for missing directories. |
| Create issue | `gh issue create --title "..." --body-file issue.md` | Opens an issue with executable evidence. |
| List issues | `gh issue list --state open` | Reads open issues. |
| View issue | `gh issue view 123` | Reads one issue. |
| Comment on issue | `gh issue comment 123 --body "..."` | Adds evidence to an issue. |
| Close issue | `gh issue close 123` | Closes an issue. |
| Open PR | `gh pr create --fill` | Opens a pull request from the pushed branch. |
| Check PR | `gh pr checks 456` | Reads required and reported checks. |
| Merge PR | `gh pr merge --squash` | Integrates the PR as one commit, subject to policy. |
