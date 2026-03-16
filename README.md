# Git Notes (Learning Git from Scratch)

A personal set of notes while learning **Git internals, commands, and
workflows**.\
Focused mainly on **individual development workflows** rather than team
collaboration.

These notes cover:

-   Git internals (objects, SHA, snapshots)
-   Useful CLI commands
-   Branching concepts
-   Fast-forward merge
-   Rebase
-   Reset
-   Remotes and GitHub

------------------------------------------------------------------------

# 1. Removing Files

### Remove file from Git + Working Directory

``` bash
git rm filename.extension
```

Removes the file from: - Git tracking - Your working directory

### Remove file only from Git (Unstage)

``` bash
git rm --cached filename.extension
```

Removes the file from Git tracking **but keeps it in your working
directory**.

------------------------------------------------------------------------

# 2. Git Configuration

Git configs exist at **three levels**:

System → Global → Local

Lower levels override higher levels.

### Set Global Config

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Stored in:

    ~/.gitconfig

### Local Config

``` bash
git config user.name "Your Name"
```

Stored inside:

    .git/config

### List Config

``` bash
git config --list --local
```

### Get a Specific Config

``` bash
git config get <key>
```

Example:

``` bash
git config get user.name
```

### Set Config

``` bash
git config set <key> <value>
```

### Unset Config

``` bash
git config unset <key>
```

Remove multiple values:

``` bash
git config --unset-all <key>
```

### Remove Section

``` bash
git config --remove-section <section>
```

------------------------------------------------------------------------

# 3. Fixing a Commit Message

If you messed up a commit message:

``` bash
git commit --amend -m "New commit message"
```

------------------------------------------------------------------------

# 4. Git Stores Snapshots

Git **does NOT store just changes**.

Instead, Git stores a **complete snapshot of the repository at that
moment**.

Each commit represents the **entire state of the project**.

------------------------------------------------------------------------

# 5. Commit Hash (SHA)

Each commit has a unique identifier.

Example:

    5ba786fcc93e8092831c01e71444b9baa2228a4f

You can usually use **first 7 characters**:

    5ba786f

Git uses a **cryptographic hash function: SHA‑1**.

These hashes are called **SHAs**.

------------------------------------------------------------------------

# 6. Git Objects

Git stores objects inside:

    .git/objects

Example:

    .git/objects/92/f27e9984619042b9bdaa6573f0dbb6bc7239c3

Structure:

    .git/objects/
     ├── 92/
     │    └── f27e9984619042...

Git splits hashes into directories to prevent **inode busting**.

------------------------------------------------------------------------

# 7. Viewing Git Objects

You *could* inspect objects manually:

``` bash
cat .git/objects/92/...
```

But Git provides a better tool:

### git cat-file

``` bash
git cat-file -p <hash>
```

Example:

``` bash
git cat-file -p 082b
```

Output example:

    tree 6494e0aa9c5e9f103587d4d7c185e00da10e95db
    parent 92f27e9984619042b9bdaa6573f0dbb6bc7239c3
    author ...
    committer ...

    B: Add title.md

------------------------------------------------------------------------

# 8. Tree and Blob Objects

Git stores files using objects.

### Blob

Represents **file contents**.

### Tree

Represents **directory structure**.

Example:

    100644 blob e69de29bb2d1d6434b8b29ae775ad8c2e48c5391 content.md

------------------------------------------------------------------------

# 9. Useful Log Commands

### Last Commit

``` bash
git log -1
```

### Log without Pager

``` bash
git --no-pager log -n 10
```

### Graph Log

``` bash
git log --oneline --decorate --graph --parents
```

Shows branch structure visually.

------------------------------------------------------------------------

# 10. Branching

A branch is simply:

> A named pointer to a commit.

The commit a branch points to is called the **tip of the branch**.

### Create a Branch

``` bash
git switch -c new_branch
```

Both branches initially point to the **same commit**.

### Create Branch from Older Commit

``` bash
git switch -c branch_name <commit_sha>
```

### Delete Branch

``` bash
git branch -d branch_name
```

You cannot delete the branch you are currently on.

------------------------------------------------------------------------

# 11. Git HEAD

`HEAD` points to:

    the latest commit of the current branch

Example:

    HEAD → main → commit

------------------------------------------------------------------------

# 12. Fast Forward Merge

Fast-forward happens when:

> The branch you merge into is exactly at the merge base.

Example:

    A --- B (main)
           \
            C (feature)

Merge feature into main:

    A --- B --- C

No new commit is created.

------------------------------------------------------------------------

# 13. Rebase

Rebase **replays commits on top of another branch**.

Before:

    A - B - C  (main)
         \
          D - E (feature)

After Rebase:

    A - B - C - D - E

Rebased commits get **new SHAs** because the parent commit changes.

### Important Rule

Never rebase **public branches** like `main`.

------------------------------------------------------------------------

# 14. Git Reset

`git reset` moves the HEAD pointer.

## Soft Reset

``` bash
git reset --soft <commit>
```

Effects:

-   HEAD moves
-   Changes remain **staged**

## Hard Reset

``` bash
git reset --hard <commit>
```

Effects:

-   HEAD moves
-   Staging cleared
-   Working directory reset

⚠ **Danger:** This can permanently delete work.

------------------------------------------------------------------------

# 15. Git Remotes

A **remote** is simply another Git repository.

Examples:

-   GitHub repo
-   Another local repo
-   Company Git server

Convention:

    origin

means the **primary remote**.

### Add Remote

``` bash
git remote add origin <repo-url>
```

### Fetch Remote Data

``` bash
git fetch
```

Fetch downloads metadata and commits but does not merge.

### View Remote Logs

``` bash
git log origin/main
```

### Merge Remote Branch

``` bash
git merge origin/main
```

------------------------------------------------------------------------

# 16. GitHub Workflow

Create a repo on GitHub.

Then connect your local repo.

### Add GitHub Remote

``` bash
git remote add origin <github-url>
```

### Verify Remote

``` bash
git ls-remote
```

### Push Code

``` bash
git push origin main
```

### If Histories Differ

``` bash
git pull origin main --no-rebase --allow-unrelated-histories
```

Then push again.

------------------------------------------------------------------------

# 17. Git Commands Types

Two types of Git commands exist.

### Porcelain

User-friendly commands.

Examples:

    git commit
    git push
    git add

### Plumbing

Low-level internal commands.

Example:

    git cat-file

------------------------------------------------------------------------

# 18. Useful Linux Commands with Git

### Manual

``` bash
man git
```

Shows Git documentation.

### List Files

``` bash
ls -l
```

### Show Hidden Files

``` bash
ls -al
```

Shows `.git` directory too.

------------------------------------------------------------------------

# Final Thoughts

Git is essentially:

> A content-addressable filesystem + version control system.

Everything in Git is an **object referenced by SHA hashes**.

Understanding **objects, trees, blobs, and commits** makes Git much
easier to reason about.


#                        TEAM
------------------------------------------------------------------------

# 1. Fork

A **Fork** is a copy of a repository that you can modify **without
affecting the original repository**.

Typical use case:

-   Contributing to open source projects
-   Experimenting without touching upstream code

Important:

> Forking is **NOT a Git operation**.

It is a **GitHub / GitLab platform feature**.

------------------------------------------------------------------------

# 2. RERERE

Saw this config while learning:

``` bash
git config set --local rerere.enabled false
```

`rerere` means:

    reuse recorded resolution

Git can remember how you resolved a conflict and automatically reuse the
same resolution later.

For now it was disabled.

------------------------------------------------------------------------

# 3. Pull Request (PR) Workflow

Typical **open source contribution workflow**:

### Step 1 --- Fork the repository

Create your own copy of the repo on GitHub.

### Step 2 --- Clone your fork

``` bash
git clone <your_fork_repo_url>
```

### Step 3 --- Create a feature branch

``` bash
git switch -c your_feature
```

### Step 4 --- Make changes

Edit files locally.

### Step 5 --- Commit and push

``` bash
git add .
git commit -m "feature: added improvement"
git push origin your_feature
```

### Step 6 --- Create Pull Request

Create a PR from:

    your_username/repo → original_owner/repo

Example:

    your_feature → main

------------------------------------------------------------------------

# 4. HEAD

`HEAD` tells Git **where you currently are in the repository**.

It is a **reference to the current branch**.

Example:

    HEAD → main → latest commit

------------------------------------------------------------------------

# 5. HEAD Is Just a File

Git literally stores HEAD in a file.

You can inspect it:

``` bash
cat .git/HEAD
```

Example output:

    ref: refs/heads/main

This means HEAD currently points to the **main branch**.

------------------------------------------------------------------------

# 6. git reflog

Reflog = **Reference Log**.

Command:

``` bash
git reflog
```

Unlike `git log`, reflog shows:

-   movements of HEAD
-   branch switches
-   resets
-   rebases

It is extremely useful for **recovering lost commits**.

------------------------------------------------------------------------

# 7. Recovering Deleted Work

Scenario:

You deleted a branch that had a unique commit.

### Step 1 --- Use reflog

``` bash
git reflog
```

Find the commit hash.

### Step 2 --- Inspect the commit

``` bash
git cat-file -p <hash>
```

You can explore deeper using:

-   trees
-   blobs

### Step 3 --- Recover the content

Manually recreate the file and commit again.

------------------------------------------------------------------------

# 8. Easier Recovery (Better Way)

Instead of manually digging blobs, you can simply do:

``` bash
git merge <commitish>
```

`commitish` can be:

-   commit hash
-   branch
-   tag
-   HEAD@{1}

Example:

``` bash
git merge HEAD@{2}
```

------------------------------------------------------------------------

# 9. Working Directory Across Branches

Interesting behavior discovered.

### Scenario

Created branch:

    add_customers

Modified files.

But accidentally ran:

``` bash
git add .
```

inside a subfolder.

So **one file got staged, another didn't**.

Committed staged changes.

Then switched branches:

``` bash
git switch add_customers
```

Git showed:

    M customers/all.csv

### Why?

Because:

> Git allows **unstaged changes to move across branches**.

This prevents losing work accidentally.

------------------------------------------------------------------------

# 10. Same Line Changes

It is completely fine if:

-   the same line is modified in a commit
-   and modified again in a later commit

Git handles that normally.

------------------------------------------------------------------------

# 11. When Conflicts Happen

Problems occur when:

> Two commits modify the **same line** but are **not in a parent‑child
> relationship**.

Example:

Two branches modify the same line independently.

This causes a **merge conflict**.

------------------------------------------------------------------------

# 12. Merge Conflict Example

Example file content:

    first_name,last_name,company,title
    <<<<<<< HEAD
    karson,yummy,intercooler,ceo
    =======
    jayson,gross,htmz,contributor
    >>>>>>> main

Meaning:

-   `HEAD` → current branch
-   `main` → incoming branch

------------------------------------------------------------------------

# 13. Resolving Merge Conflict

Steps:

1.  Open the conflicted file
2.  Remove markers:

```{=html}
<!-- -->
```
    <<<<<<<
    =======
    >>>>>>>

3.  Keep the correct content
4.  Stage the file

``` bash
git add .
```

5.  Complete the merge with a commit

``` bash
git commit
```

------------------------------------------------------------------------

# 14. Ours vs Theirs

During a merge conflict:

    Ours   → branch you are currently on
    Theirs → branch being merged

------------------------------------------------------------------------

# 15. Resolving Multi‑File Conflicts

Git allows choosing changes automatically.

Example:

``` bash
git checkout --ours file_path
```

or

``` bash
git checkout --theirs file_path
```

This replaces the file with the selected version.

------------------------------------------------------------------------

# 16. Example

``` bash
git checkout --theirs customers/all.csv
```

or

``` bash
git checkout --ours customers/all.csv
```

------------------------------------------------------------------------

# Key Takeaways

-   Forking is a **GitHub concept**, not a Git command.
-   `HEAD` is literally just a file pointing to your branch.
-   `reflog` can **save you from losing work**.
-   Merge conflicts occur when **unrelated commits modify the same
    line**.
-   `--ours` and `--theirs` help resolve conflicts quickly.
