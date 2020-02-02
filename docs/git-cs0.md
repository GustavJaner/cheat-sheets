## Push to a New Branch
```
git checkout -b <newBranch>      # Create a new local branch
# do changes to your code..
git status                       # List changed files
git add .                        # Add all changed files
git commit -m “commitMsg“        # Create a commit
git push -u origin <newBranch>   # Push new branch with the commit
```

## Push to Current Existing Branch
```
git status                   # List changed files
git add .                    # Add all changed files
git commit -m “commit msg“   # Create a commit
git push                     # Push the commit to upstream branch
```

## Merge and Rebase
```
git merge <branch>         # Merge  <branch> to current branch
git pull origin <branch>   # Fetch+Merge origin <branch>

git rebase <branch>        # Rebase <branch> to current branch
git rebase -i HEAD~n       # Rebase the last n commits of HEAD
```

## Fetch and Pull
```
git fetch   # Update the origin branches of local repo
git pull    # Update current branch with upstream branch
```
