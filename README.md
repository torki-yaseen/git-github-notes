<div align="center">

# 🌳 The Git & GitHub Cheat Sheet 🚀

### *My personal notes from the course: from `git init` to time-travelling with `reflog`*

![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Notes](https://img.shields.io/badge/Status-Always%20Learning-brightgreen?style=for-the-badge)
![Commands](https://img.shields.io/badge/Commands-100%2B-blueviolet?style=for-the-badge)


</div>

---

## 🗺️ Table of Contents

| # | Chapter | Vibe |
|---|---------|------|
| 1 | [Commits & `.gitignore`](#1--commits--gitignore) | 🙈 Hide the junk |
| 2 | [Working With Branches](#2--working-with-branches) | 🌿 Parallel universes |
| 3 | [Merging Branches](#3--merging-branches) | 🤝 Bring them together |
| 4 | [Git Diff](#4--comparing-changes-with-git-diff) | 🔍 Spot the difference |
| 5 | [Stashing](#5--stashing) | 📦 Hide your work for later |
| 6 | [Undoing Changes & Time Travel](#6--undoing-changes--time-traveling) | ⏪ Oops-proofing |
| 7 | [GitHub Basics (Remotes & Push)](#7--github-the-basics) | ☁️ Go online |
| 8 | [Fetching & Pulling](#8--fetching--pulling) | 📥 Sync with the cloud |
| 9 | [GitHub Grab Bag](#9--github-grab-bag) | 🎁 Pages & extras |
| 10 | [Collaboration Workflows](#10--collaboration-workflows) | 👯 Team up |
| 11 | [Rebasing](#11--rebasing-the-scariest-git-command) | 😱 Rewrite history |
| 12 | [Interactive Rebase](#12--interactive-rebase-clean-up-your-history) | 🧹 Tidy up |
| 13 | [Tags](#13--git-tags) | 🏷️ Mark milestones |
| 14 | [Git Internals](#14--git-behind-the-scenes) | 🧠 Under the hood |
| 15 | [Reflog](#15--the-power-of-reflogs) | 🧙 Recover "lost" work |
| 16 | [Custom Aliases](#16--custom-git-aliases) | ⚡ Type less |

---

## 🧭 The Big Picture

```mermaid
flowchart LR
    A[📝 Working Directory] -- git add --> B[🎭 Staging Area]
    B -- git commit --> C[🏛️ Local Repo]
    C -- git push --> D[☁️ Remote / GitHub]
    D -- git fetch --> C
    D -- git pull --> A
    B -- git restore --staged --> A
```

---

## 1. 🙈 Commits & `.gitignore`

> A long lecture, summarized in 4 steps.

**Ignoring files with `.gitignore`**

```bash
touch .gitignore                 # create it in the main directory of the repo
start .gitignore                 # open it (Windows) and list the files/folders to ignore
git add .
git commit -m "add gitignore"
```

---

## 2. 🌿 Working With Branches

**Create a commit, then a new branch**

```bash
git switch master
# ...make the "New list" change...
git add .
git commit -m "New list"
git branch roadList              # create the branch
git switch roadList              # hop onto it
# ...make the "add item" change...
git add .
git commit -m "add item"
```

**⚡ Stage + commit in one line** *(tracked files only!)*

```bash
git commit -a -m "new item"
```

**🕵️ Jump to a different point in history, edit, commit**

```bash
git log
git switch <destination>
# ...modify the file...
git commit -a -m "new comment"
```

**🗑️ Deleting & ✏️ renaming branches**

```bash
git branch -D deleteMe           # force delete a branch
git branch -m renameMe           # rename the CURRENT branch
```

> 💡 **How Git stores HEAD & branches:** a branch is just a pointer to a commit, and `HEAD` is a pointer to "where you are right now".

---

## 3. 🤝 Merging Branches

```bash
git switch master                # always merge INTO the branch you're standing on
git merge branchName
```

| Merge type | When it happens |
|-----------|-----------------|
| ⏩ **Fast-forward** | No new commits on the target branch, so Git just moves the pointer |
| 🔀 **Merge commit** | Both branches have new work, so Git creates a new commit joining them |
| 💥 **Conflict** | Same lines changed in both branches, so you decide |

**Resolving merge conflicts**

1. Open the conflicted file(s) and find the `<<<<<<<`, `=======`, `>>>>>>>` markers
2. Edit the file to keep what you want and delete the markers
3. `git add .`
4. `git commit`

---

## 4. 🔍 Comparing Changes With Git Diff

| Command | What it shows |
|---------|---------------|
| `git diff` | Working directory vs. staging area |
| `git diff HEAD` | Everything changed since your last commit |
| `git diff --staged` *(or `--cached`)* | Only the staged changes |
| `git diff HEAD [filename]` | Changes within one specific file |
| `git diff --staged [filename]` | Staged changes within one specific file |
| `git diff branch1..branch2` | Differences between two branches |
| `git diff <hash1> <hash2>` | Differences between two commits |

> 📖 **Reading a diff:** lines starting with `-` were removed, lines starting with `+` were added.

---

## 5. 📦 Stashing

> Save changes you aren't ready to commit yet. Perfect for "wait, I need to switch branches!" moments.

```bash
git stash                        # stash your changes away
git stash pop                    # bring them back AND remove them from the stash
git stash apply                  # bring them back but KEEP them in the stash
                                 # (handy for applying the same changes to multiple branches)
git stash list                   # see all stashes
git stash drop stash@{0}         # delete one specific stash
git stash clear                  # delete ALL stashes 💣
```

> ⚠️ You can stash multiple times, but it's usually better to avoid piling up stashes.

---

## 6. ⏪ Undoing Changes & Time Traveling

### 🚗 Visiting old commits

```bash
git checkout <first-7-chars-of-hash>   # go to an old commit (detached HEAD)
git checkout HEAD~1                    # one commit before HEAD
git switch -                           # jump back to the previous branch
```

### 🧽 Discarding / un-modifying files

```bash
git checkout HEAD fileName       # discard changes, revert to HEAD
git checkout -- fileName         # same idea (older syntax)
git restore fileName             # modern way: restore file to HEAD contents
```

### 🎭 Un-staging

```bash
git restore --staged fileName            # take a file OFF the staging area
git restore --source HEAD~1 fileName     # get a file back as it was one commit ago
```

### 💣 Undoing commits

| Command | Effect |
|---------|--------|
| `git reset <commit-hash>` | Moves the branch back to that commit. The commits are gone, but your changes stay in the working directory |
| `git reset --hard <commit>` | Moves back AND deletes the associated changes 😬 |
| `git revert <commitHash>` | Undoes the changes by creating a **brand new commit** (safe for shared history ✅) |

> 🧠 **Rule of thumb:** `reset` rewrites history (local work), `revert` adds to history (shared work).

---

## 7. ☁️ GitHub: The Basics

### 📡 Remotes

> A remote is really just **two things: a URL and a label.**

```bash
git remote -v                          # view existing remotes
git remote add <name> <url>            # add a new remote (needs both!)
git remote rename <old> <new>
git remote remove <name>
```

### 🚀 Pushing

```bash
git push <remote> <branch>             # push to GitHub

git push <remote> <localBranch>:<remoteBranch>
# ex:
git push origin pancake:master         # push local "pancake" to the remote's "master"

git push -u origin master
# -u sets the upstream, so local master TRACKS origin/master
# (afterwards, plain `git push` / `git pull` just work)
```

---

## 8. 📥 Fetching & Pulling

### 🛰️ Remote-tracking branches

```bash
git branch -r                    # list remote tracking branches
git checkout origin/master       # check out a remote branch pointer
git switch -                     # back to the previous branch
```

**Working with remote branches after a clone**

- When you clone a repo you only get the default branch locally
- `git branch -r` shows the other branches
- `git checkout origin/branch` lets you look at one
- `git switch nameOfBranch` creates a local branch that tracks the remote one of the **same name**

### 🎣 Fetch vs 🧲 Pull

```bash
git fetch origin master          # download changes, but DON'T touch my local branch
git checkout origin/master       # peek at what was fetched

git pull <remote> <branch>       # fetch + merge into the current branch
git pull                         # shorter version, uses the upstream
```

> 🧠 **`git pull` = `git fetch` + `git merge`**

---

## 9. 🎁 GitHub Grab Bag

**GitHub Pages demo**
- ✨ An animated text effect using SVG text
- 🎨 Linking a CSS file with `<link rel="stylesheet" href="...">`

---

## 10. 👯 Collaboration Workflows

**Merging pull requests with conflicts**

```bash
git merge --no-ff branchName     # prevents a fast-forward merge (always makes a merge commit)
```

**🍴 The Fork & Clone workflow**

1. **Fork** the original repo on GitHub
2. **Clone** your fork
3. Add the original repo as `upstream` to keep your fork up to date:

```bash
git remote add upstream <original-repo-url>
git pull upstream master         # bring in the latest updates
```

---

## 11. 😱 Rebasing: The Scariest Git Command?

```bash
git switch feature
git rebase master
```

- Rebases `feature` **onto** `master`: the whole branch is moved so it begins at the tip of `master`
- All the work is still there, but **history has been rewritten** ✍️
- Instead of a merge commit, rebase creates **new commits** for each of the original feature commits

> ☠️ **Golden rule:** never rebase commits that you've already pushed and shared with others.

---

## 12. 🧹 Interactive Rebase: Clean Up Your History

Rebase can be used as a **merging tool** *and* as a **cleanup tool**.

```bash
git rebase -i HEAD~4             # edit the last 4 commits
```

You're not rebasing onto another branch here, you're specifying how far back to rewrite.

| Keyword | What it does |
|---------|--------------|
| `pick` | ✅ Use the commit as is |
| `reword` | ✏️ Use the commit but edit the message |
| `edit` | 🛑 Use the commit but stop to amend it |
| `fixup` | 🧲 Meld it into the previous commit and discard its message (keeps the changes) |
| `drop` | 🗑️ Remove the commit **and** its changes |

```bash
git commit --amend               # tweak the most recent commit
```

---

## 13. 🏷️ Git Tags

> Mark important moments in history, like releases.

```bash
git tag                                  # list all tags
git tag -l "*beta*"                      # list only tags matching a pattern
git checkout v0.0.0                      # view the repo at a tag
git diff v1.0.0 v0.0.0                   # compare two tags

git tag <tagname>                        # lightweight tag
git tag -a <tagname>                     # annotated tag (opens your editor for a message)
git tag -a <tagname> -m "message"        # annotated tag, message inline

git tag <tagname> <commit-hash>          # tag an OLDER commit
git tag -f v17.0.0 <commit-hash>         # force-move an existing tag
git tag -d <tagname>                     # delete a tag
```

> ### ❗ IMPORTANT: Pushing tags
> By default, `git push` does **not** push your tags.
> ```bash
> git push --tags
> ```

---

## 14. 🧠 Git Behind The Scenes

### ⚙️ Config

```bash
git config user.name                         # show the user name
git config --local user.name "Your Name"     # set it for this repo only
git config --global user.name "Your Name"    # set it globally
```

### #️⃣ Hashing & storing objects

```bash
echo 'hello' | git hash-object --stdin
# `echo` repeats text, we pipe it to `git hash-object` to get a 40-character SHA-1 hash

git hash-object fileName.txt -w              # -w stores the object in .git/objects
git cat-file -p <object-hash>                # -p pretty-prints the object's content
git cat-file -p <object-hash> > color.txt    # save the output to a file (creates it if missing)
```

**Handy terminal helpers**

```bash
touch fileName.txt               # create a file
start fileName.txt               # open it in the editor (Windows)
cat fileName.txt                 # print its contents
```

### 🧱 Git objects

| Object | What it is |
|--------|-----------|
| **Blob** | *Binary Large OBject.* Stores a file's **contents only**: no filename, no other data |
| **Tree** | Stores directory structure (filenames + pointers to blobs and other trees) |
| **Commit** | Points to a tree plus metadata (author, message, parent) |

```bash
git cat-file -p master^{tree}    # view the tree of the latest commit on master
```

---

## 15. 🧙 The Power of Reflogs

> **Reflog = reference log.** Git quietly records every move of `HEAD`, even for commits that look "lost". 🔮

```bash
git reflog show main                 # reflog for the tip of the main branch
git checkout HEAD@{2}                # where HEAD was 2 MOVES ago
git checkout HEAD~2                  # 2 COMMITS back (detached HEAD)
```

⏰ **Time-based qualifiers** *(entries kept for about 90 days)*

```bash
git reflog master@{one.week.ago}
git checkout bugfix@{2.days.ago}
git diff main@{0} main@{yesterday}
```

### 🛟 Rescuing lost commits

```bash
git reset --hard db727ba             # 😱 oh no, work deleted!
git reflog show master               # find the hash of the commit you needed
git checkout <hash>                  # you're back! (or `git reset --hard <hash>`)
```

### ✨ Undoing a rebase, "it's a miracle!"

```bash
git reflog show flowers              # find the state before the rebase
git log --oneline
git reset --hard c47ecfc             # reset to that hash
```

---

## 16. ⚡ Custom Git Aliases

```bash
git config --global user.email                        # show your email
git config --global user.email "youremail@gmail.com"  # set your email
cat ~/.gitconfig                                      # view your global settings
```

**Create a shortcut:**

```bash
git config --global alias.br branch
```

Now instead of `git branch` you just type:

```bash
git br
```

---

<div align="center">

## 🎉 That's a wrap!

**Commit early, commit often, and when in doubt, check the `reflog`.** 😉

⭐ *If these notes helped you, drop a star on the repo!* ⭐

</div>
