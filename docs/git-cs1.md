## Repository Information
```
git status   # Info about current branch
git log      # List all commits of current branch
git show     # List the changes of the last commit
git diff     # List the changes of unstaged files
git branch   # List all local branches
```

## Stash
```
git stash         # Save changes to the stash stack
git stash list    # List the stash stack
git stash apply   # Resume the top/latest stash
```

## Reset
```
git reset HEAD            # Unstage all files
git reset --hard HEAD     # Discard all changes

git reset HEAD~n          # Reset   the n latest commits
git reset --hard HEAD~n   # Discard the n latest commits
```

## Misc.
```
git clone <repoUrl>     # Download remote repository

git checkout <branch>   # Redirect HEAD to a branch
git checkout <commit>   # Redirect HEAD to a commit
```
