# Git & GitHub Notes

To ignore files with `.gitignore`:

1. In the main directory of the repo, run `touch .gitignore`.
2. Open `.gitignore` and add all filenames you want to ignore.
3. Run `git add .`.
4. Run `git commit -m "add gitignore"`.
Branching practice notes:

```bash
git log
git switch "toHisDestination"
# modify his file
git commit -a -m "new comment"
```

```bash
git commit -a -m "new item"
```
Stages and commits in one line.

To create a commit and a new branch:

```bash
git switch master
# Make the “New list” change
git add .
git commit -m "New list"
git branch roadList
git switch roadList
# Make the “add item” change
git add .
git commit -m "add item"
```

```bash
git branch -m renameMe
git branch -D deleteMe
```
Renames a branch and deletes a branch, respectively.

“Steps to remove conflicts.”

```bash
git merge branchName
git merge fileName
```

---

```bash
git diff commitHashNumber commitHashNumber
git diff branch1 branch2
git diff HEAD [file name]
git diff HEAD --staged [filename]
git diff --staged
git diff --cached
git diff HEAD
git diff
```
These compare two commits, compare branches, show changes in a specific file, show staged changes, list working-tree changes since the last commit, or compare the staging area and working directory.

“How to read the git diff.”

---

```bash
git stash
git stash list
git stash pop
git stash apply
git stash drop stashId
git stash clear
```
`git stash` saves changes you are not ready to commit. `git stash list` shows stashes. `git stash pop` reapplies the stashed changes and removes the stash. `git stash apply` is similar to pop, but can apply the changes in multiple repos. `git stash drop` deletes a particular stash; `git stash clear` clears all stashes. The notes advise avoiding multiple stashes.

---

```bash
git revert commitHash
```
Undoes changes by creating a brand-new commit.

```bash
git reset --hard <commit>
git reset <commit-hash>
```
Resets the repo to a specific commit. The notes say `--hard` deletes the last commit and associated changes, and that the commits are gone after reset.

```bash
git restore --source HEAD~1
git restore --staged fileName
git restore fileName
```
Restores a file to where it was; unstages a file from the staging area; or restores the file to the contents in `HEAD`, respectively.

```bash
git checkout HEAD fileName
git checkout --
```
Discards file changes and reverts to `HEAD`.

```bash
git checkout HEAD~1
git checkout hashIdFirst7Digit
```
Checks out an earlier commit; the note says the hash command goes back to the previous commit.

---

```bash
git push -u origin master
```
Sets the upstream of the local master branch so it tracks the master branch on the origin repo.

```bash
git push <remoteName normally origin> <localBranch>:<remoteBranch>
git push origin pancake:master
```
Pushes a local branch to a remote branch; the example pushes local `pancake` to remote `master`.

```bash
git push <remote> <branch>
```
Pushes the branch to GitHub.

```bash
git remote rename <old> <new>
git remote remove <name>
git remote add <name> <url>
git remote -v
```
These manage remotes. A remote is a URL and a label. `git remote -v` displays existing remotes.

---

```bash
git pull
```
A shorter syntax for pulling from GitHub to the local machine.

```bash
git pull <remote> <branch>
```
Fetches the latest info from the origin master branch and merges those changes into the current branch.

```bash
git fetch origin master
git checkout origin/master
```
Fetches changes onto the machine. Checking out `origin/master` lets you see them; the local master branch remains untouched.

When cloning a repo from GitHub, the notes say:

- If you clone it in main, you will have files in main/master only.
- `git branch -r` shows other branches.
- `git checkout origin/branch` goes to that branch.
- `git switch nameOfBranch` creates a local branch connected to the same-named GitHub branch; the names must match.

```bash
git switch -
```
Switches back to the previous branch.

```bash
git branch -r
git checkout origin/master
```
Lists remote-tracking branches and checks out a remote branch pointer, respectively.

---

```bash
git merge --no --ff
```
Prevents a fast-forward merge.

“css cope link rel href”

“animated txt file with svg txt”

```bash
git pull
git remote add upstream <url>
```
`git pull` brings the latest updates. Add an `upstream` remote to keep a fork updated with changes from the original repository.

“briefly”

“conclusion”

---

```bash
git switch feature
git rebase master
```
Rebases the feature branch onto master. This moves the branch so it begins at the tip of master. The work remains, but history is rewritten. Instead of a merge commit, rebase creates new commits for each original feature commit.

---

```bash
git commit --amend
```
Amends the commit itself.

```bash
git rebase -i HEAD~<number>
```
Opens an editor for interactive rebase. Specify how far back to rewrite commits. This can be used to edit commits, add files, or drop commits; the note says this is not rebasing onto another branch. Rebase can be used as a merging tool or a cleanup tool.

Interactive rebase commands:

- `pick` — use the commit
- `reword` — use the commit but edit its message
- `edit` — use the commit but stop for amending
- `fixup` — use the commit contents but meld it into the previous commit and discard the commit message
- `drop` — remove the commit

Choose `drop` to delete the commit and its changes. Choose `fixup` to delete the commit but keep its changes.

```bash
git rebase -i HEAD~4
```
Enters interactive mode; choose how far back to rewrite commits.

---

```bash
git push --tags
```
Pushes all tags. By default, pushing a commit does not push all tags.

```bash
git tag -d <tagname>
```
Deletes a tag.

```bash
git tag v17.0.0 <hash> -f
```
Force the tag onto the commit.

```bash
git tag <myCommit> <hash>
```
Tags a previous commit.

```bash
git tag -a <tagname>
git tag -m
```
Creates an annotated tag; Git opens the default editor for additional information. Use `-m` to provide a message directly and skip opening the editor.

```bash
git checkout v0.0.0
git diff v1.0.0 v0.0.0
```
Checks out the repo state at a tag and lists differences between the two tags.

```bash
git tag -l "*beta*"
git tag
```
Lists tags matching `*beta*`, or all tags in the current repo.

---

```bash
git cat-file -p master^{tree}
```
Prints Git objects; this example views the trees.

A blob (Binary Large Object) is the Git object type used to store file contents in a repository. Blobs do not include filenames or other data; they store only the contents of a file.

```bash
git cat-file -p hashObjectCode > color.txt
git cat-file -p hashObjectCode
```
The first command copies the object info into `color.txt` (creating the file if needed). The second prints the info in the command line.

```bash
git hash-object fileName.txt -w
```
Stores the hash in `.git/object`.

```bash
touch fileName.txt
start fileName.txt
cat fileName.txt
```
Creates `fileName.txt`, opens it in the editor, and displays its contents in the command line, respectively.

```bash
git cat-file -p <object-hash>
```
Retrieves data stored in Git's object database. The `-p` option pretty-prints the object content based on its type.

```bash
echo 'hello' | git hash-object --stdin
```
Pipes the output of `echo` to `git hash-object` to get a SHA-1 hash (40 characters).

```bash
git config user.name
git config --local user.name "any name will show your name"
git config --global user.name "any name will show your name"
```
The first command shows the user name. The global setting applies globally.

---

To undo changes after a rebase, the notes show:

```bash
git reflog show flowers
git log --oneline
git reset --hard c47ecfc
```

To recover work after resetting, inspect the reflog and check out the needed commit:

```bash
git reset --hard db727ba
git reflog show master
git checkout hashNumber
```
The reflog can show the hash for the needed commit.

Examples of time-based reflog references:

```bash
git reflog master@{one.week.ago}
git checkout bugfix@{2.days.ago}
git diff main@{0} main@{yesterday}
```
These filter reflog entries by time and date (the note says up to 90 days).

```bash
git checkout HEAD@{2}
git checkout HEAD~2
```
`HEAD@{2}` means go two moves ago. `HEAD~2` goes back two commits and leaves you in detached HEAD state.

```bash
git reflog show main
```
View the reflog for the tip of the `main` branch. Reflog means reference log.

---

```bash
git config --global alias.br branch
```
That creates an alias, so instead of typing `git branch` every time, you can type `git br`.

```bash
cat ~/.gitconfig
```
Displays your global Git settings, like your username and email.

```bash
git config --global user.email
git config --global user.email "youremail@gmail.com"
```
The first command shows your email; the second sets it.

