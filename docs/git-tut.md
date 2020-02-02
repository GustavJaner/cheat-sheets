# Misc.
- Master branch should be stable. Production code  
      - Use dev branches to develop and then `merge`/`rebase` to master

- HEAD points to the tip (most recent Commit) of current branch  
			- Or to a specific older commit (detached HEAD)

- Remote Repo = Origin

- Remote Tracking branch = Upstream branch

- File life cycle:  
      - Untracked -> Unmodified -> Modified/Unstaged -> Staged -> Committed -> Pushed

## Standard Push Procedure
1. **Stage(add)** the local files that you have changed and that you want to commit
2. **Commit** the staged  files
3. **Push** the commit to the remote repository upstream branch

# Getting Started
## Setup a New Repo from the CLI
```
$ mkdir <folderName> && cd "$_"

$ git init                                  # Initialise a Local git repository in current directory
$ echo "# Title of repo" >> README.md       # Create the README.md file
$ echo "# Files to ignore:" >> .gitignore   # Create the .gitignore file
$ git add .                                 # Stage all modified files
$ git commit -m "Init commit"               # Commit the Staged files
$ git remote add origin <repoUrl>           # Add the Remote Repo (use an empty repo!)
$ git push -u origin master                 # Push local Repo to the Remote origin
```

### Create a New Repo from an Existing Repository
```
$ git pull origin master --allow-unrelated-histories
```

### Change the Remote Repository of a Local Repository
```
git remote -v                                 # List the current remote repository
git remote set-url <remoteName> <remoteUrl>   # Let remoteName=origin
```
