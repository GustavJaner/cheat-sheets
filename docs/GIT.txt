GIT cheat sheet
Gustav Janér

=======================================================
                        MISC
=======================================================

- HEAD points to the tip (most recent Commit) of current branch
			or to a specific older Commit (detached HEAD)

- Master branch should be stable. Production code
      Use branches to develop and then merge/rebase to Master

- Remote Repo = Origin

- Remote tracking branch = Upstream branch

- Always: pull before push

- File life cycle:
      Untracked -> Unmodified -> Modified -> Staged -> Committed -> Pushed

=======================================================
                  Standard procedure
=======================================================

1. Stage(add) the files that you have changed and that you want to Commit
2. Commit the Staged  files
3. Push the Committed files to the Remote Repository


--- New branch ---

CHECKOUT a New local branch -> STAGE changes -> COMMIT changes -> PUSH new Branch to Remote Repo:

$ git checkout -b <newBranch>     (Flag -b creates a new branch)
... do changes...
$ git status         		          (Check which files should to be Staged)
$ git add .	      		            (Stage all modified files)
$ git commit -m “commit message“  (Commit the Staged files. -m = message)
$ git push origin <newBranch>     (Push the new local branch to Remote Repo)

HINT! last step:
If you are planning to push out a new Local branch that
should track its Remote counterpart, you may want to use
$ git push -u origin <newBranch>
to set the Upstream config as you Push.

-------------------------------------------------------
                        CLONE
-------------------------------------------------------

$ git clone <repo url>  (Copy remote repo to local environment)

-------------------------------------------------------
                       BRANCHES
-------------------------------------------------------

$ git branch                     (Display all local branches)
$ git branch -a                  (Display all local and Remote branches of repo)
$ git branch <branch>            (Creates a new branch without 'checking it out'/redirecting HEAD)

$ git checkout <anyBranch>       (Change to an existing branch/redirect HEAD to point that branch)
$ git checkout -b <newBranch>    (Create a new local branch and redirect HEAD to point to it)
			         (checkout -b is short for the two above commands)

$ git branch -d <anyBranch>      (Remove a local branch)
$ git push origin :<anyBranch>   (Remove a Remote branch on Remote repository)


--- Set a Remote Tracking Upstream Branch of a local branch ---
To avoid having to write : $ git pull origin <branch>

$ git status
On branch dev
Your branch is up to date with 'origin/dev'.  <-- IF NOT shown: the local branch has NO upstream branch
nothing to commit, working tree clean

OR IF:
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.

- Set the Upstream branch -
$ git branch --set-upstream-to=origin/dev  (Set upstream branch of current branch)
$ git branch -u origin/dev                 (Shorter command)

--> Local branch dev set up to track remote branch dev from origin.

$ git pull   (then, no need to specify origin <branch>)
$ git push

-------------------------------------------------------
                      CHECKOUT
-------------------------------------------------------

$ git checkout                 (Check which Files have been modified)
$ git checkout -- <file>       (Discard changes of an Unstaged file in working directory)

$ git checkout -b <newBranch>  (Create a new local branch and redirect HEAD to it)
$ git checkout <anyBranch>     (Check out/Redirect HEAD to an existing branch)
$ git checkout <anyCommit>     (Check out/Redirect HEAD to a specific commit - Detached HEAD state)

-------------------------------------------------------
                         ADD
-------------------------------------------------------

Add files to HEAD to Stage them for Commit

$ git add .               (Stage all modified files)
$ git add *               (Stage all modified files)

$ git add <file>          (Stage a single file)


$ git reset HEAD          (Unstage all files. a reset --soft)
$ git reset HEAD <file>   (Unstage a single file)

-------------------------------------------------------
                        COMMIT
-------------------------------------------------------

$ git commit -m “commit message“  (Commit all Staged files with the given message)
$ git commit                      (Enter text editor to write commit message. Save to commit)

-------------------------------------------------------
                         PUSH
-------------------------------------------------------
$ git push   (Push all Commits of current Local branch to its Remote Upstream branch)


--- If current Local branch is new and has No Upstream branch ---

$ git push -u origin <branch>  (If Remote branch does not exist in repo: Remote branch is created and set as Upstream)

- Or, set the Upstream branch to an existing Remote branch and then push:

$ git branch -u origin/<anyBranch>  (Set the upstream branch of current Local branch)
$ git push

-------------------------------------------------------
                        RESET
-------------------------------------------------------

$ git reset HEAD <file>       (Unstage single file)
$ git reset HEAD              (Unstage all files, --soft)

$ git reset --hard            (Discard all uncommitted changes on current branch)


--- Uncommit UN-pushed Commits ---

$ git reset --soft HEAD~1     (Reset the most recent commit, Keeping the changes of the commit)
$ git reset --soft HEAD~n     (Reset the n number of recent commits)

$ git reset --hard HEAD~1     (Reset the most recent commit, Discarding the changes of the commit)
$ git reset --hard HEAD~n     (Reset the n number of recent commits)


*** CAREFUL with: reset --hard ***
All local changes currently made will be lost.

To undo a reset - CANNOT though recover changes that were NOT previously Staged(added):

$ git reset HEAD@{1}


--- Uncommit a pushed commit ---
Use any of the above reset commands and then force push to change the remote branch:

$ git push -f

-------------------------------------------------------
                        REVERT
-------------------------------------------------------

Use to undo a Commit that is already pushed to repo

$ git revert <commit hash>  (Create a new commit which reverts the changes of the commit you specified)

-------------------------------------------------------
                        FETCH
-------------------------------------------------------

Update All REMOTE Branches of the cloned repo from Remote Origin

$ git fetch  (Update remote-tracking branches)

-------------------------------------------------------
                         PULL
-------------------------------------------------------

Automatically performs fetch first. Pull can cause merge-conflicts

$ git pull                (Update ONLY the CURRENT Local branch with its Upstream branch)
$ git pull origin master  (Update current Local branch with new Commits of Remote Master branch. Essentially a Merge)

-------------------------------------------------------
                        MERGE
-------------------------------------------------------

Can cause conflicts

Merge vs. Rebase
Merge is safer than Rebase. Merge creates an extra merge-commit and do NOT rewrite commit history

$ git merge <anyBranch>       (Merge the changes of anyBranch into the current branch)


--- Merge procedure ---

$ git checkout dev && git pull

Make sure dev is up to date with Master and resolve conflicts. Check that feature still works with new updates from Master
--> To avoid redundant conflicts and eventual following bugs to reach Master. Keep master stable. Creates an extra commit
$ git pull origin master


Perform the Merge to Master:
$ git checkout master && git pull
$ git merge dev
$ git push

-------------------------------------------------------
                      REBASE
-------------------------------------------------------

*** Commit history will diverge from origin ***
Be careful. Especially when rebasing public branches

If you fuck up: Rewind history to just before the current rebase operation:
$ git rebase –-abort


--- Rebasing instead of merging ---
Apply commits from one branch onto another - Without creating a merge commit
Result: Linear commit history, free of merge-commits
Rebase step: Rewinding Head to replay the commits of dev on top of master


- Rebase dev to master -

$ git checkout dev && git pull
$ git rebase origin/master       (Rebase the new commits of Master, to Dev)


$ git status
On branch dev
Your branch and 'origin/dev' have DIVERGED,
and have 3 and 2 different commits each, respectively.
(use "git pull" to merge the remote branch into yours)

  --> If we pull now, we merge the rebasing with origin... Not what we want.
  --> When rebasing, you’re changing the commit history-->need to Force Push to Remote branch

$ git push -f


- Merge to master -

$ git checkout master && git pull

Rebase to Fast-Forward master to dev:
$ git rebase dev

Or, since dev has been already been rebased on top of master:
 --> a merge will be performed by the Fast-Forwarding strategy automatically (No merge commit)
$ git merge dev

$ git push


--- Rebasing commits on own branch ---
To clean up the commit history of a private branch before merging back to master

$ git rebase -i HEAD~n  (Interactively rebase the last n commits from HEAD)
$ git push -f

To rebase multiple commits into one:
use squash on all commits except the oldest commit

-------------------------------------------------------
                      DIFF/SHOW
-------------------------------------------------------

$ git diff                (Check the changes of Unstaged files compared to last commit)
$ git diff <commit hash>  (Compare differences to a specific commit)

$ git show                (Check the changes of the last commit compared to the 2nd last commit)
$ git show <commit hash>  (Check the changes of a specific commit)

-------------------------------------------------------
                         LOG
-------------------------------------------------------

$ git log            (Display a list of all commits to current branch)
$ git log --oneline  (Cleaner log)

-------------------------------------------------------
                        STASH
-------------------------------------------------------

$ git stash         (Save changes (Both staged and unstaged) of current branch away to the Stash Stack)

$ git stash list    (List all stashed stashes on the Stack)

$ git stash drop    (Drop the top Stash on the Stack)
$ git stash drop i  (Drop the Stash at index i of the Stack)


$ git stash apply   (Resume the latest Stash on Stack)
$ git stash apply i (Resume the Stash at index i of the Stack)

$ git stash pop     (Resume the latest Stash and drop it from the Stack)

$ git stash show    (Summary of the top Stash)
$ git stash show i  (Summary of Stash at index i of the Stack)

-------------------------------------------------------
                        CLEAN
-------------------------------------------------------

Remove untracked files from current git branch

$ git clean -n  (Display which files will be deleted)
$ git clean -f  (Delete the untracked files. Actually deletes the files so be careful)
$ git clean -i  (Display the Clean interface)

-------------------------------------------------------
                        IGNORE
-------------------------------------------------------

Add files to the .gitignore to ignore them from being tracked by git

$ echo "AnyFile" >> .gitignore

Or just open .gitignore in a text editor











=======================================================
                   SETUP A NEW REPO
=======================================================

$ mkdir <folderName>
$ cd <folderName>

$ git init			       (Initialise a LOCAL git repository in current directory)
$ echo "# Some git repo" >> README.md  (Creates the README.md file)
$ vim .gitignore                       (Creates the .gitignore file)
$ git add README.md	               (Stages the README.md file)
$ git commit -m "first commit"         (Commit the Staged files)
$ git remote add origin <repo url>     (Stages new Origin Repo)
$ git push -u origin master            (Pushes staged Repo)

If creating a new git for an existing repository
git pull origin master --allow-unrelated-histories

--- Create a .gitignore ---


=======================================================
                    FAST-FORWARD
=======================================================

$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)

You’ll notice the phrase “fast-forward” in that merge. Because the commit C4 pointed to by the branch hotfix you merged in was Directly ahead of the commit C2 on Master, Git simply moves the pointer forward. To phrase that another way, when you try to merge one commit with a commit that can be reached by following the first commit’s history, Git simplifies things by moving the pointer forward because there is no divergent work to merge together — this is called a “fast-forward.”

--> This is the strategy of Rebasing


==> see the pointing of the local head and and the origin head of master
    --> only when it looks like this, then Fast-Forward of main is possible

$ git log
commit c708f92c46e9366cedf17811dbe098b1b74f6ec2 (HEAD -> gustav-main)
Author: GustavJaner <gustav.janer@gmail.com>
Date:   Thu Jul 18 12:24:48 2019 +0200

    :
    [UserName] Updates files after new join API of videos
    - Displays username instead of user id in PreviewItem and VideoItem
    - Reworks PreviewItem to a functional component

commit 3edfd07fe9d700e2fb1b562daa745e7df7a1cc4e (origin/master, origin/HEAD, master)
Author: GustavJaner <gustav.janer@gmail.com>
Date:   Wed Jul 17 10:43:13 2019 +0200

    [VideoPreview] Feed of video previews
    - Adds a container for video playback for a single video: path=/video/:id
    - Restructures the video feed into a preview feed
    - Click a video preview Link to play the specific video
    - Click user id Link to to be redirected to user profile


=======================================================
                   RECURSIVE/MERGE
=======================================================

$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)

This looks a bit different than the hotfix merge you did earlier. In this case, your development history has diverged from some older point. Because the commit on the branch you’re on isn’t a direct ancestor of the branch you’re merging in, Git has to do some work. In this case, Git does a simple three-way merge, using the two snapshots pointed to by the branch tips and the common ancestor of the two.


=======================================================
                  MERGE vs REBASE
=======================================================

If you prefer a clean, Linear history, free of any unnecessary merge-commits:
you should use git Rebase instead of git Merge when integrating changes from another branch
(git Merge creates extra merge-commits, Rebase does not)

On the other hand, if you want to preserve the complete history of your project and
avoid the risk of re-writing Public commits, you should use git Merge


=======================================================
               CHECKOUT out a BRANCH
=======================================================

Before switching branches/redirecting the HEAD pointer to another branch:
If your working directory or staging area has uncommitted changes that conflict with the branch you’re checking out, Git won’t let you switch branches. It’s best to have a clean working state when you switch branches.
