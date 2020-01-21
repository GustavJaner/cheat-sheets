# Getting Started

## Misc.
- Master branch should be stable. Production code  
      - Use dev branches to develop and then `merge`/`rebase` to master

- HEAD points to the tip (most recent Commit) of current branch  
			- Or to a specific older Commit (detached HEAD)

- Remote Repo = Origin

- Remote Tracking branch = Upstream branch

- File life cycle:  
      - Untracked -> Unmodified -> Modified/Unstaged -> Staged -> Committed -> Pushed

## Standard Push Procedure
1. **Stage(add)** the local files that you have changed and that you want to Commit
2. **Commit** the Staged  files
3. **Push** the Committed files to the Remote Repository

### Push New Branch
```
$ git checkout -b <newBranch>      # Create a new local branch and redirect HEAD to it

# do changes...

$ git status                       # Check which files have been modified
$ git add .                        # Stage all modified files
$ git commit -m “commit message“   # Commit the Staged files (m = message)
$ git push -u origin <newBranch>   # Push the new local branch to Remote Repo (u = set the Upstream branch)
```
