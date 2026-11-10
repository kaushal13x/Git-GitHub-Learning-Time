# Git & GitHub --- Day 1 Notes

**Level:** Beginner\
**Goal:** Understand how Git tracks changes and how to save your work on
GitHub.

------------------------------------------------------------------------

## 1. What are Git and GitHub?

-   **Git** is a version control system. It records changes to files so
    you can see history and go back to earlier versions.
-   **GitHub** is an online platform where you can store Git
    repositories and collaborate with other people.
-   A GitHub repository can be **public** (anyone can view it) or
    **private** (only people you allow can view it).

> Easy way to remember: **Git tracks your project; GitHub hosts and
> shares your Git project online.**

## 2. The three main areas in Git

When you work with Git, your files move through three important areas:

``` mermaid
flowchart LR
    A["Working Directory<br/>Files you edit"] -->|"git add"| B["Staging Area<br/>Changes selected for the next commit"]
    B -->|"git commit"| C["Local Repository<br/>Saved project history"]
    C -->|"git push"| D["GitHub Repository<br/>Online copy"]
    D -->|"git pull"| C
```

  -----------------------------------------------------------------------
  Area                                Meaning
  ----------------------------------- -----------------------------------
  Working Directory                   Your project folder, where you
                                      create and edit files.

  Staging Area                        A preparation area. You select
                                      which changes should go into the
                                      next commit.

  Local Repository                    Git's saved history on your
                                      computer.

  GitHub Repository                   The online repository hosted on
                                      GitHub.
  -----------------------------------------------------------------------

**Important:** `git add` does not upload a file to GitHub. It only
stages the change locally. `git commit` saves it in your local
repository. `git push` uploads commits to GitHub.

## 3. Configure Git for the first time

Set the name and email that Git will attach to your commits:

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

View your configuration:

``` bash
git config --list
```

View only global settings:

``` bash
git config --global --list
```

Check a single setting:

``` bash
git config user.name
git config user.email
```

Use the name and email you want associated with your commits. Your Git
commit email does not have to be publicly displayed on GitHub.

## 4. Start tracking a project

Move into your project folder first:

``` bash
cd /c/ArthNotes/Git-GitHub/Projects
```

Initialize Git in that folder:

``` bash
git init
```

-   `git init` creates a new local Git repository in the current folder.
-   It creates a hidden `.git` directory where Git stores repository
    data.
-   Run it once when starting a repository; do not repeatedly initialize
    an existing project.

Check the current state:

``` bash
git status
```

## 5. The basic Git workflow

Imagine you create a file called `test.txt`.

### Step 1 --- Create or edit a file

``` bash
vim test.txt
```

In Vim: 1. Press `i` to enter Insert mode. 2. Type your text. 3. Press
`Esc` to return to Normal mode. 4. Type `:wq` and press `Enter` to save
and exit.

### Step 2 --- Check what changed

``` bash
git status
```

This tells you which files are untracked, modified, staged, or
committed.

### Step 3 --- Stage a file

``` bash
git add test.txt
```

Stage all changes in the current folder:

``` bash
git add .
```

### Step 4 --- Check staged files

``` bash
git status
```

List files currently tracked by Git's index:

``` bash
git ls-files
```

Note: `git ls-files` lists files in Git's index (normally tracked files,
including staged additions). For a beginner, use `git status` to see
exactly what is staged for the next commit.

### Step 5 --- Commit the staged changes

``` bash
git commit -m "Add my first test file"
```

A **commit** is a saved snapshot of the staged changes. Write a short
message that explains what you changed.

### Step 6 --- Check the result

``` bash
git status
git log
```

If Git says `working tree clean`, there are no uncommitted changes in
your working directory or staging area.

## 6. Useful commands for viewing history and changes

  -----------------------------------------------------------------------
  Command                             What it does
  ----------------------------------- -----------------------------------
  `git log`                           Shows detailed commit history.

  `git log --oneline`                 Shows each commit in a short,
                                      one-line format.

  `git log --stat`                    Shows commits and a summary of
                                      files/lines changed.

  `git log --graph --oneline --all`   Shows a compact graph of commits
                                      across branches.

  `git show`                          Shows details and changes from the
                                      latest commit (`HEAD`).

  `git show HEAD~1`                   Shows the commit immediately before
                                      the latest commit.

  `git diff`                          Shows unstaged changes (working
                                      directory compared with staging
                                      area).

  `git diff --staged`                 Shows staged changes (staging area
                                      compared with latest commit).

  `git diff <commit1> <commit2>`      Compares two commits. Replace the
                                      placeholders with real commit IDs.

  `git ls-tree HEAD`                  Lists files and folders recorded in
                                      the latest commit.
  -----------------------------------------------------------------------

**Spelling tip:** The correct option is `--oneline`, not `--onenline`.

### Understand `HEAD`

`HEAD` is Git's name for the commit you currently have checked out
(usually the latest commit on your current branch).

``` text
HEAD       = current commit
HEAD~1     = one commit before HEAD
HEAD~2     = two commits before HEAD
```

## 7. Branch basics

A **branch** is a separate line of work. It lets you develop a feature
without changing the main line of work immediately.

See branches:

``` bash
git branch
```

Rename the current branch to `main`:

``` bash
git branch -M main
```

`-M` force-renames the current branch if needed. For a new repository,
this is commonly used to name the main branch `main`.

## 8. Connect your local repository to GitHub

First, create an empty repository on GitHub. Copy its repository URL.
Then, in your project folder, run:

``` bash
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
```

Check the remote:

``` bash
git remote -v
```

Rename your branch if you want it to be called `main`:

``` bash
git branch -M main
```

Push your commits to GitHub:

``` bash
git push -u origin main
```

-   `origin` is the usual nickname for the remote GitHub repository.
-   `main` is the branch being pushed.
-   `-u` sets the upstream connection, so future pushes can usually be
    done with `git push`.

For future updates, use:

``` bash
git add .
git commit -m "Describe your update"
git push
```

**Note:** If your current branch is still named `master`, either push
`master` (`git push -u origin master`) or rename it to `main` first and
push `main`. The branch name in the push command must match your local
branch.

## 9. Undoing or removing changes --- beginner guide

Be careful with undo commands. Some actions can permanently discard
work.

  ---------------------------------------------------------------------------------
  Situation               Command                           Result
  ----------------------- --------------------------------- -----------------------
  Unstage a file but keep `git restore --staged test.txt`   Removes it from
  its edits                                                 staging; keeps the file
                                                            changes.

  Discard unstaged edits  `git restore test.txt`            Restores the file to
  to a file                                                 its last committed
                                                            version. **Local edits
                                                            are lost.**

  Remove a file from Git  `git rm test.txt`                 Stages the file
  and your folder                                           deletion.

  Stop tracking a file    `git rm --cached test.txt`        Stages removal from
  but keep it on your                                       Git, but leaves the
  computer                                                  local file.

  Safely undo a commit    `git revert <commit-id>`          Creates a new commit
  that was already shared                                   that reverses the
                                                            selected commit.
  ---------------------------------------------------------------------------------

Avoid `git reset --hard` while learning: it can permanently discard
uncommitted work.

## 10. Quick revision sheet

``` bash
git config --list                 # View Git configuration
git init                          # Initialize a repository
git status                        # Check file status
git add test.txt                  # Stage one file
git add .                         # Stage all changes
git commit -m "message"           # Save a commit
git log                            # View commit history
git log --oneline                 # Compact history
git log --graph --oneline --all   # Graph of all branches
git show                          # Inspect latest commit
git diff                          # View unstaged changes
git diff --staged                 # View staged changes
git branch                        # List branches
git branch -M main                # Rename current branch
git remote -v                     # View remote URLs
git push -u origin main           # Push branch to GitHub
git pull                          # Fetch and integrate remote changes
```

## 11. Day 1 practice task

Try this yourself in your practice repository:

1.  Create a file named `day1.txt`.
2.  Write 2--3 lines about what you learned today.
3.  Run `git status`.
4.  Stage the file using `git add day1.txt`.
5.  Run `git diff --staged` to inspect it.
6.  Commit it with a meaningful message.
7.  Run `git log --oneline`.
8.  If your GitHub remote is configured, push the commit.

### Example commit message

``` bash
git commit -m "Add Day 1 Git and GitHub notes"
```

------------------------------------------------------------------------

**Day 1 takeaway:** `git add` prepares changes, `git commit` saves them
locally, and `git push` sends committed changes to GitHub.

Keep practising these commands until you can explain what each one does
without looking at the notes.
