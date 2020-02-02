# Git Commands

## CLONE
```
$ git clone <repoUrl>   # Download remote repository to local environment
```

## BRANCH
```
$ git branch                    # List all local branches
$ git branch -v                 # List all local branches with the commit hash+message of the respective branch tip
$ git branch -a                 # List all local and Remote branches of repo
$ git branch <newBranch>        # Creates a new branch (without 'checking it out'/redirecting HEAD)

$ git checkout <branch>         # Redirect HEAD to an existing branch
$ git checkout -b <newBranch>   # Create a new local branch and redirect HEAD to it (short command)

$ git branch -d <branch>        # Remove a Local  branch
$ git push origin :<branch>     # Remove a Remote branch
```

### Set a Remote Tracking/Upstream Branch of a Local Branch
#### Use Flag -u when Pushing
```
$ git push -u origin <branch>   # If Remote branch does not already exist in repo: Remote branch is created and set as Upstream
```

#### Set the Upstream Branch to an Existing Remote Branch
```
$ git branch --set-upstream-to=origin/dev   # Set the Upstream branch of current local branch
$ git branch -u origin/dev                  # Short command
```

## CHECKOUT
```
$ git checkout                  # Less detailed than $ git status
$ git checkout -- <file>        # Discard(--hard) changes of an Unstaged file in working directory

$ git checkout -b <newBranch>   # Create a new local branch and check out/redirect HEAD to it
$ git checkout <branch>         # Redirect HEAD to an existing branch
$ git checkout <commitHash>     # Redirect HEAD to a specific commit - Detached HEAD state
```

## ADD
Stage files for commit
```
$ git add .               # Stage all modified files
$ git add <file>          # Stage a single file

$ git reset HEAD          # Unstage all files (--soft)
$ git reset HEAD <file>   # Unstage a single file
```

## COMMIT
Commit staged files to be pushed
```
$ git commit -m “commit message“   # Commit all Staged files with the given message
$ git commit                       # Enter text editor to write commit message. Save to commit
```

## PUSH
Push local commits to Origin
```
$ git push   # Push all Commits of current Local branch to its Upstream branch
```

### If Current Local branch has no Upstream Branch
```
$ git push -u origin <branch>   # If Remote branch does not already exist in repo: Remote branch is created and set as Upstream
```

## FETCH
Update all local origin branches of the Local repo
```
$ git fetch   # Update all the origin branches of the local repo
```

**Note:**  
`fetch` only updates the local copies of the Origin branches in the local repo. It does **not** `merge` the Origin updates with the actual local branches. Even if the local branches are tracking the Origin branches that are updated.

## PULL
`pull` is short command for: `fetch`+`merge`
```
$ git fetch
$ git merge origin <branch>

# Above commands are equal to:
$ git pull origin <branch>
```

### Update a Local Branch with a Remote branch
```
$ git pull                 # Update the current Local branch with its Upstream branch
$ git pull origin master   # Update the current Local branch with the Remote master branch
```

## MERGE

### Safe Workflow
Never let `merge` conflicts reach the origin master branch.  
Always update a dev branch with origin master first, before merging the dev branch to origin master - to resolve any merge conflicts in the dev branch instead of origin master. Also to make sure that the dev feature still works with any new updates from origin master.  
**Keep master stable**

### Update dev Branch With New Commits to origin master
```
# * currently on branch dev *

# * if branch dev is private:
$ git pull origin master     # Update local dev branch with origin master

# * if branch dev is public:
$ git pull                   # Update current branch dev with upstream Commits + Fetch(update) the local origin/master branch
$ git merge origin master    # Update local dev branch with origin master

# resolve potential conflicts...

$ git push
```

### Merge dev Branch to origin master
Always create a **Pull Request(PR)** on the web interface of your Git provider to merge new commits to master. Request other collaborators to review the PR before merging.  
_Otherwise, also possible to use the Git CLI:_
```
# * currently on branch dev *

$ git checkout master && git pull   # Update local master branch with origin master
$ git merge dev
$ git push
```

### Merging vs. Rebasing
`merge` is safer than `rebase`. Merging creates an extra merge-commit if needed and **cannot** rewrite the commit history

## REBASE
**Commit history can diverge from origin**  
**Be careful. Especially when rebasing public branches**

If you mess up, rewind history to just before the current `rebase` operation:
```
$ git rebase –-abort
```

### Rebasing Instead of Merging
- Rewinding HEAD to replay the commits of one branch on top of another branch
- Apply commits from one branch onto another - without creating a merge-commit
- Result: Linear commit history, free of merge-commits

#### Rebase master to dev
To be able to perform the next step of Rebasing dev to master:  
First, the dev branch has to be Directly ahead of the Remote HEAD of the master branch - so that Fast-Forwaring(Rebasing) is possible
```
# * currently on branch dev *


$ git pull
$ git rebase origin/master   # Rebase the new commits of master to dev - dev is now directly ahead of master

$ git status
On branch dev
Your branch and 'origin/dev' have DIVERGED,
and have 3 and 2 different commits each, respectively.
(use "git pull" to merge the remote branch into yours)

# If we pull now, we merge the rebasing with Upstream branch... Not what we want.
# When rebasing, you’re changing the commit history --> need to Force Push to Upstream branch

$ git push -f
```

#### Rebase dev to master
Create a **Pull Request(PR)** on the web interface of your Git provider, or use the Git CLI:
```
$ git checkout master && git pull
$ git rebase dev                    # Rebase to Fast-Forward master to the tip of dev
$ git push
```
**Note:**  
In this case, since dev has been already been rebased on top of master:  
--> a `merge` would also be performed by the **Fast-Forwarding** strategy automatically - thus same result as `rebase`.

### Rebasing Commits on Single Branch
To clean up the commit history of a private branch before merging back to master

```
$ git rebase -i HEAD~n   # Interactively rebase the last n commits from HEAD

....
  # Commands:
  # p, pick   <commit> = use commit
  # r, reword <commit> = use commit, but edit the commit message
  # e, edit   <commit> = use commit, but stop for amending
  # s, squash <commit> = use commit, but meld into previous commit
  # f, fixup  <commit> = like "squash", but discard this commit's log message
  # d, drop   <commit> = remove commit
  # x, exec  <command> = run command (the rest of the line) using shell
  # b, break = stop here (continue rebase later with 'git rebase --continue')
  # l, label <label> = label current HEAD with a name
  # t, reset <label> = reset HEAD to a label
  # m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
....

$ git push -f
```

_To rebase multiple commits into one: use **squash** on all commits except the oldest commit_

## RESET
### Soft vs. Hard Reset
- soft reset will unstage/uncommit files/commits, but still keep the changes
    - `reset --soft` is the default if not specified
- hard reset will unstage/uncommit files/commits, **discarding** all changes

**Careful:** `reset --hard`  
All local changes currently made will be **lost**. To undo a `reset`:
```
$ git reset HEAD@{1}   # CANNOT though recover changes that were never previously Staged
```

### Unstage Staged Files
```
$ git reset HEAD <file>     # Unstage single file
$ git reset HEAD            # Unstage all files

$ git reset --hard HEAD     # Discard all uncommitted changes on current branch
```

### Uncommit Unpushed Commits
```
$ git reset HEAD~1          # Reset the most recent commit    - Keeping the changes of the commit
$ git reset HEAD~n          # Reset the n most recent commits - Keeping the changes of the commit

$ git reset --hard HEAD~n   # Reset the n most recent commits - Discarding the changes of the commit
```

### Uncommit Pushed Commits
Use any of the above `reset` commands and then force `push` to remove the commit from remote branch
```
$ git push -f
```

## REVERT
Reverts a commit that is already pushed to Remote repo
```
$ git revert <commitHash>   # Create a new commit which reverts the changes of the commit you specified
```

## DIFF/SHOW
```
$ git diff                 # List the changes of Unstaged files compared to last commit
$ git diff <commitHash>    # List the changes compared to a specific commit

$ git show                 # List the changes of the last commit compared to the 2nd last commit
$ git show <commitHash>    # List the changes of a specific commit
```

## LOG
```
$ git log             # List all commits to current branch and show HEAD pointers
$ git log --oneline   # Cleaner log
```

### HEAD Pointers
The HEAD points by default to the tip (most recent Commit) of current branch, or to a specific older Commit (detached HEAD)
```
$ git log --oneline

d1a3319 (HEAD -> master)             [Git] Rewrite merge vs. rebase section
bf30977 (origin/master, origin/HEAD) [Git] Rearange sections
```
- **HEAD -> master** - the Local HEAD is currently pointing to the tip of the Local master branch
- **origin/master** - the Remote master branch is currently one commit behind the Local master branch
- **origin/HEAD** - this branch/commit is the default initial checkout when the repo is cloned

## STASH
Save the current state of your working directory on the Stash Stack, that can at a later point be reapplied
```
$ git stash           # Save changes (both staged and unstaged) of current branch and Push to the Stash Stack

$ git stash list      # List all stashed stashes on the Stack

$ git stash drop      # Drop the top Stash on the Stack
$ git stash drop i    # Drop the Stash at index i of the Stack

$ git stash apply     # Resume the latest Stash on Stack
$ git stash apply i   # Resume the Stash at index i of the Stack

$ git stash pop       # Resume the latest Stash and drop it from the Stack

$ git stash show      # Summary of the top Stash
$ git stash show i    # Summary of Stash at index i of the Stack
```

## CLEAN
Remove untracked files from current git branch
```
$ git clean -n   # Display which files will be deleted
$ git clean -f   # Delete the untracked files. Actually deletes the files so be careful
$ git clean -i   # Display the Clean interface
```

# Misc.
## MERGE vs. REBASE
- If you prefer a clean, Linear history, free of any unnecessary merge-commits:  
use `rebase` when integrating changes from other branches.  
Merging creates an extra merge-commit - Rebasing does not.

- If you prefer preserving the complete history of your project and avoiding the risk of rewriting Public commits:  
use `merge` when integrating changes from other branches.  
Merging do not risk rewriting commit history - Reabasing does.

### Fast-Forward
```
$ git checkout master && git pull
$ git merge dev
Updating f65asdf..234gar
Fast-forward                  # Fast-forward
 index.html | 3 ++
 2 file changed, 5 insertions(+)
```

When the commit pointed to by the dev branch is Directly ahead of the tip of master, Git simply moves the master HEAD pointer forward(Fast-Forwarding).  
When merging a branch with another branch that can be reached by following the first branch's commit history - Git performs a Fast-Forward of the commits.

**--> This is the strategy of Rebasing**  
Notice below the pointing of the local HEAD and and the origin of master:  
only when the HEAD pointer of dev is Directly ahead of **origin/master** - then Fast-Forward of dev to origin master is possible
```
$ git log --oneline                            # Fast-Forward possible

c708f92 (HEAD -> dev)                          [Register] Update client for the new API
8hfeaj3                                        [Config] Minor update config file
3edfd07 (origin/master, origin/HEAD, master)   [VideoPreview] Create the feed of video previews
```

### Recursive Three-Way-Merge
```
$ git checkout master && git pull
Switched to branch 'master'

$ git merge dev
Merge made by the 'recursive' strategy.
index.html |    2 +
2 file changed, 5 insertion(+)
```
When the commit pointed to by the dev branch is **Not** directly ahead of the tip of master, Git has to make a three-way-merge (Recursive Strategy).  
This is the case when the development history has diverged at some previous commit.  
When the branch merged into master is not a direct ancestor of master, Git performs a three-way merge - using the two commits pointed to by the branch tips and the common ancestor commit.


## Checkout a Branch
Before checking out branches/redirecting the HEAD pointer to another branch/commit:  
If your working directory/staging area has uncommitted changes that conflict with the branch you’re checking out, then Git won’t let you switch branches.
Either way, it’s always best practice to have a clean working state before you switch branches - always `commit` or `stash` before checking out
