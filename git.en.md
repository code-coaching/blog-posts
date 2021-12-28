---
title: What is Git and what is GitHub? Explaining the basics of Git and GitHub!
slug: what-is-git-and-what-is-github-explaining-the-basics-of-git-and-github
tags:
  - Git
  - GitHub
categories:
draft: true
---

Git is a version control system. Nice, but what does that mean? A system to control versions. Also nice, but what does it mean to control versions? To explain this, it is easier to look at an example!

When there is a reference to `terminal` in the text below and Windows is used as an operating system, it means that the command needs to be executed in `Git Bash`. If CTRL+V does not work to paste the copied content in the terminal, try SHIFT+INSERT.

## Installing Git

Perhaps it is already installed. Open a terminal (Linux/macOS) or command prompt (Windows). Type `git --version` and press `Enter`.

```sh
git --version
```

If Git is installed, you should see something like this (numbers can differ): `git version 2.30.2` or `git version 2.30.0.windows.2`.

If Git is not installed, no version will be shown. To install Git:

### [Linux](https://git-scm.com/download/linux)

Execute the following command in a terminal (Ubuntu):

```sh
sudo apt install git
```

### [macOS](https://git-scm.com/download/mac)

Execute the following command in a terminal (macOS):

Installing [Homebrew](https://brew.sh/#install).

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Installing Git.

```sh
brew install git
```

### [Windows](https://gitforwindows.org/)

- Click on Windows above.
- Click on `Download`.
- Double-click on the downloaded file.
- Keep everything default and keep hitting `Next`.
- Click on `Install`.

This will also install `Git Bash`. When there is a reference to `terminal` in the text below and Windows is used as an operating system, it means that the command needs to be executed in `Git Bash`.

## Working with the terminal

The terminal can also be used to navigate the file system of the computer, creating new files and creating new folders. This will also be used to learn how to use Git. Thanks to Git Bash it is possible to use the same commands on Windows as on Linux and macOS.

### pwd

After opening a terminal, it will probably start at the home directory. This is often symbolised by `~`. To know where this directory is, type `pwd` (**p**rint **w**orking **d**irectory) and press `Enter`.

```sh
pwd
```

The output of the command:

- Linux: `/home/JohnDuck`
- macOS: `/Gebruikers/JohnDuck`
- Windows: `/c/Gebruikers/JohnDuck`
  - This is equal to: `C:/Gebruikers/JohnDuck`

Note: Unless JohnDuck is chosen as a user name, this will differ on the local system.

### cd

To be certain that the working directory is the home directory, `cd` (**c**hange **d**irectory) can be used.

```sh
cd ~
```

Note: `~` is the home directory.

Image there is a folder called `projects`. To go to this folder, type `cd projects` and press `Enter`.

```sh
cd projects
```

Note: If there are spaces in the name of the folder, either use quotes or use `\` to escape the spaces.

```sh
cd 'Folder With Spaces In The Name'
```

```sh
cd Folder\ With\ Spaces\ In\ The\ Name
```

Tip: Start typing and press `tab`, this will autocomplete the folder name.

### ls

To know which files and folders are in the current directory, type `ls` (**l**ist **s**ystem) and press `Enter`.

```sh
ls
```

It also accepts arguments.

```sh
ls -a
```

The "-a" stands for "all" argument will show hidden files and folders (files and folders that start with a dot `.`). This will also show `./` and `../`. One dot refers to the current directory. Two dots refer to the parent directory (one directory above the current directory).

### mkdir

To create a new folder, type `mkdir` (**m**ake **d**irectory) and press `Enter`.

```sh
mkdir projects
```

This will create a folder called `projects` in the current directory.

### touch

To create a new file, type `touch`, followed by the name of the file, and press `Enter`.

```sh
touch example.txt
```

This will create a new file called `example.txt` in the current directory.

## Using Git locally

Git is software without a GUI (Graphical User Interface). There is no icon that can be clicked to start the program. Git is a command line interface, which means that it can be used from the command line/terminal. Start by opening a terminal.

Create a new folder called `git-example` in the home directory and change the location to this folder.

```sh
cd ~
```

This will change the location to the home directory.

```sh
mkdir git-example && cd git-example
```

By using `&&` it is possible to execute multiple commands in one command. This will create a new folder called `git-example` and change the location to this folder.

### Making a folder into a Git repo

The word `repo` refers to `repository`. A repository is a folder that contains all the files that are part of a project.

Currently the folder `git-sexample/` is not a Git repo, it is an ordinary folder.

Use `ls -a` to see the files and folders in the current directory, it will not show anything, since the directory is empty.

```sh
ls -a # The output is: ./ ../
```

Create a Git repo by using `git init`.

```sh
git init #  This will output "Initialized empty Git repository in [this part differs per user]/git-example/.git"
```

Use `ls -a`, it will now show the folder `.git`.

```sh
ls -a # Dit heeft als output: ./ ../ .git/
```

Look at the folder `.git/` and see what is inside.

```sh
cd .git/ && ls -a
```

This will have the output: `./ ../ HEAD config description hooks/ info/ objects/ refs/`. It is not necessary to change anything here, it is enough to know that this is the location where Git keeps track of all the changes.

Go up one directory.

```sh
cd ..
```

Changes are recorded in `commits`. A commit is a snapshot of the current state of the repository. To know which changes already happened, `git log` can be used.

```sh
git log # The output: fatal: your current branch 'master' does not have any commits yet
```

Because there are not commits yet, an error message is shown. There is a reference to `branch` in the error message, currently there will only be worked with one branch, named `master`.

### Adding a commit to the Git repo

With `git status` the current status of the Git repo can be checked.

```sh
git status

###
# The output:
# On branch master
#
# No commits yet
#
# nothing to commit (create/copy files and use "git add" to track)
###
```

Executing `git status` tells us that there are no changes yet, there is nothing to `commit` yet.

Create a new file called `README.md`. A `.md` file is a Markdown file. Dit is a text file like a .txt file, but with an added benefit that `Markdown` syntax can be used. In this example the Markdown syntax will not be used, but still there is a reason that this file is called `README.md`. This is the file that will automatically be loaded when visiting a Git repo on the internet.

```sh
touch README.md

```

With `ls` it can be checked that the file now exists in the current directory.

Execute `git status` again. It will still say that we currently are on the master branch `On branch master`. It will still say there is nothing committed yet `No commits yet`. What is different now, is the `untracked files`. This means that there are new files that are not tracked by Git.

```sh
git status

###
# On branch master

# No commits yet

# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         README.md
#
# nothing added to commit but untracked files present (use "git add" to track)
###
```

To track this file with Git from this point onwards, it is necessary to add it to the Git repo. This is done by using `git add`.

```sh
git add README.md

###
# On branch master
#
# No commits yet
#
# Changes to be committed:
#  (use "git rm --cached <file>..." to unstage)
#        new file:   README.md
###
```

Execute `git status` again. It still says we are on the master branch `On branch master`. It still says there is nothing committed yet `No commits yet`. This is indeed the case, since nothing has been committed yet, but there is a new file being tracked by Git.

To permanently add this file to the Git repo, it is necessary to `commit` the changes. This is done by using `git commit`.

<details>
  <summary>Do this once before the first commit</summary>

```sh
git config user.name "Bart Duisters"
```

Change "Bart Duisters" to the name that should be shown when adding changes.

```sh
git config user.email "bartduisters@bartduisters.com"
```

Change "bartduisters@bartduisters.com" to a personal email address.

Tip: In case a GitHub account has already been created, use the email address that is associated with the GitHub account. In case no GitHub account has been created yet, use a personal email address that later will be associated with the GitHub account.

This will configure the username and email address for this specific Git repo. In case all the Git repos will use the same username and email address, it is not necessary to configure this for each Git repo. Instead, it can be configured globally.

```sh
git config --global user.name "Bart Duisters"
```

```sh
git config --global user.email "bartduisters@bartduisters.com"
```

</details>

```sh
git commit -m "Add README.md"

###
# [master (root-commit) c0aba7a] Add README.md
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  create mode 100644 README.md
###
```

Note: The `c0aba7a` will differ. This is the beginning of the `commit hash`, which is a unique identifier for the commit.

The `-m` (message) is a parameter that is used to describe the changes that are being committed. The message between the double quotes can be anything, it describes the changes that are being committed.

An overview about the changes is shown `1 file changed, 0 insertions(+), 0 deletions(-)`. It shows that there is one file that has been changed (added in this case), and that there are no insertions or deletions.

Execute `git status` again.

```sh
git status

###
# On branch master
# nothing to commit, working tree clean
###
```

This shows that there are no changes to commit, and that the working tree is clean `working tree clean`.

### Viewing history

Using `git log` it is possible to see the history of the Git repo. All commits done throughout the history of the Git repo can be seen.

```sh
git log

###
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b (HEAD -> master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

This shows the commit message `Add README.md`. The full commit hash (unique identifier of this commit) `c0aba7a9e06057c325719bac3357faff70ec9d2b`. The author `Bart Duisters <bartduisters@bartduisters.com>` (this is the value assigned to `user.name` and `user.email`). The date of the commit `Thu Jun 24 22:25:20 2021 +0200`.

Open the file `README.md` in a text editor and add the text `bla bla bla`. Looking at `git status` it is now clear that there are changes to commit. Git already knows that there are changes to commit (because `README.md` is already tracked by Git, it is a `tracked file`).

```sh
git status

###
# On branch master
# Changes not staged for commit:
#  (use "git add <file>..." to update what will be committed)
#  (use "git restore <file>..." to discard changes in working directory)
#        modified:   README.md
#
# no changes added to commit (use "git add" and/or "git commit -a")
###
```

Hints are shown about what can be done next. With `git add README.md` the change can be staged to eventually be committed. With `git restore README.md` the change can be discarded.

Add the changes. Commit the changes and look at the history again.

```sh
git add README.md
git commit -m "Add text to README.md"
###
# [master 81f48fc] Add text to README.md
#  1 file changed, 1 insertion(+)
###
```

<details>
  <summary>Windows users, click here!</summary>

When executing `git add` on a file that has changed lines, an extra message will be shown.

```sh
###
# warning: LF will be replaced by CRLF in README.md.
# The file will have its original line endings in your working directory
###
```

Unix operating systems (like Linux and macOS) use a LF (Line Feed, also visualized as `\n`) line ending. On Windows this is a CRLF (Carriage Return and Line Feed, also visualized as `\r\n`). Line Feed and Carriage Return are terms based on the actions that had to be performed on a typewriter. Line Feed, a new line was achieved by physically moving the sheet one line up. Carriage Return, the part of the typewriter that is used to move the sheet back to the left margin.

To make sure the code is compatible with both operating systems, Git will replace the CRLF line endings with LF line endings when doing a `git add`. When a repo is pulled from a remote repo, Git will replace the CRLR line endings with LF line endings.

</details>

A new commit is created `[master 81f48fc] Add text to README.md` which shows that one file has been changed, and that there is one insertion `1 file changed, 1 insertion(+)`.

```sh
git log

###
# commit 81f48fc07e47f264d5912b56104c20852b819e1d (HEAD -> master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#    Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

The Git log shows all commits done throughout the history of the Git repo.

### The power of Git

Time travel is not possible. Or isit? Through `git checkout` it is possible to revert to a previous commit. Imagine 100 commits being done over the course of a year of which the commits above are the first two commits.

To see how this file looked after the first commit, execute `git checkout` on the Git hash of the first commit.

```sh
git checkout c0aba7a9e06057c325719bac3357faff70ec9d2b

###
# Note: switching to 'c0aba7a9e06057c325719bac3357faff70ec9d2b'.
#
# You are in 'detached HEAD' state. You can look around, make experimental
# changes and commit them, and you can discard any commits you make in this
# state without impacting any branches by switching back to a branch.
#
# If you want to create a new branch to retain commits you create, you may
# do so (now or later) by using -c with the switch command. Example:
#
#  git switch -c <new-branch-name>
#
# Or undo this operation with:
#
#  git switch -
#
# Turn off this advice by setting config variable advice.detachedHead to false
#
# HEAD is now at c0aba7a Add README.md
###
```

This provides a lot of information, this will not be explained in detail here. Take a look at `README.md` in the text editor and notice that `bla bla bla` is no longer present. This is correct, because after the first commit the file was added, but no text was added to it.

Use `git switch -` to no longer watch the first commit.

```sh
git switch -
```

To know the difference between two commits, use `git diff`. This will show the changes between the two commits `git diff <old commit> <new commit>`, where `<old commit>` is the commit hash of the oldest commit between the two and `<new commit>` is the commit hash of the newest commit between the two.

```sh
git diff c0aba7a9e06057c325719bac3357faff70ec9d2b 81f48fc07e47f264d5912b56104c20852b819e1d

###
# diff --git a/README.md b/README.md
# index e69de29..cd591db 100644
# --- a/README.md
# +++ b/README.md
# @@ -0,0 +1 @@
# +bla bla bla
###
```

This shows that `bla bla bla` was added.

This also shows that everything that was ever added to the history of Git, can always be seen in the history. It is very important to **never add secrets** to the files that will be committed. For software developers this could mean `passwords`, `API keys`, `environment variables` ...

## GitHub - using Git online

Since the 13th of august 2021 it is no longer possible to use a username and password to pull and push to GitHub. The easiest way to now use GitHub is to use `ssh`. There is an article on this blog that can be followed to setup SSH to use with GitHub.

### Coupling a remote repo to an existing local repo

Until now all the commits were tracked in the local `.git` folder and as long as the `.git` folder is present, it is possible to see the history of the repo. But, imagine, there are two devices present (a desktop and a laptop) and sometimes the desktop is used and sometimes the laptop is used. To make sure all commits are present on both devices, the `.git` folder must be present on both devices and these must be kept in sync. The same is true if multiple people are working on the same repo, the `.git` folder must be present on all devices and these must be kept in sync.

Nice, so the `.git` folder can be put in `Google Drive`, `OneDrive` or `Dropbox`, this way the `.git` folder is always in Sync? No! The cloud storage to use is [MEGAsync](https://mega.nz/pro/aff=xLXv1HsQvUE) (referral, 20 GB free storage). That being said, a Git repo is not kept in sync by placing it in a cloud storage.

Wat is used to keep a repo in sync on multiple devices? Online platforms that provide extra functionality around the `.git` folder, such as creating `releases`. One of the most used platforms is GitHub. One of the possible alternatives is [Bitbucket](https://bitbucket.org/). To follow allong with this article, sign up for [GitHub](https://github.com/signup).

After logging in, it is possible to create a new repo by clicking on [new](https://github.com/new). Enter a `repository name`, e.g. `git-example-remote`. By default this repo will be public, this means that anyone can see the repo (also by people without a GitHub account). In case the content of the repo should not be viewable by the public, the repo can be made private by clicking on `private`.

Next there are three checkboxes. When creating a new repo that does not need to be connected to an existing local repo, it is handy to check all three of the checkboxes. Since there already is a local repo, **leave them all unchecked**.

- `Add a README file`, this can be check to automatically generate a `README.md` when creating a repo.

- `Add .gitignore`, this will be added manually in this article.

- `Choose a license`, by default the code is **not** opensource. So even if the repo is public, if it does not contain a LICENSE file, the code is **not** opensource. If the code is meant to be used by others, it is important to add an opensource license.

Click on `Create repository`. Make sure that **no checkbox is checked** while following this article.

What this does behind the scenes, is creating a `.git` folder on the server of GitHub. This means that there is now a `.git` folder on the local system and a separate `.git` folder on the system of GitHub (remote system). To sync the remote Git repo with the local Git repo, a `remote origin` can be added to the local repo. This is done from the local repo in the terminal.

```sh
git remote add origin git@github.com:bartduisters/git-example-remote.git
```

Note: In case the same name `git-example-remote` was used during the creation of the remote repo, then only `bartduisters` needs to be swapped out for your own GitHub username.

The origin can be found by clicking on the `clone` button on the GitHub page of the repo and then selecting the `SSH` tab.

![SSH tab](/img/blog/git-ssh.png)

```sh
git branch -M master
```

Git works with `branches`, in this article only one `branch` will be used. This step is not necessary at this moment, but in case in the future the primary branch is called difference (`main` is used more and more), then this will rename it to `master`, such that all next steps in this article will keep working.

By now the local Git knows about the existence of the remote Git. To make sure the local commits are also present on the remote Git, the `git push` command can be used. Since this is the first push, we need to specify to which remote branch we want to push.

```sh
git push -u origin master

###
# Enumerating objects: 6, done.
# Counting objects: 100% (6/6), done.
# Delta compression using up to 12 threads
# Compressing objects: 100% (2/2), done.
# Writing objects: 100% (6/6), 451 bytes | 451.00 KiB/s, done.
# Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/bartduisters/git-example-remote.git
# * [new branch]      master -> master
# Branch 'master' set up to track remote branch 'master' from 'origin'.
###
```

All future commits of this repo can be pushed by using `git push`.

Go to the remote repo on GitHub and refresh the page. This will now show the `README.md` file.

It is also possible to do quick edits online:

- Click on README.md.
- Click on the edit icon (top right `Edit this page`).
- Change `bla bla bla` to `bla John Duck`.
- Scroll down and click on `Commit changes`.
  - By default the commit message is `Update README.md`.
  - This is the same as `git commit -m "Update README.md"`.

Return to the home page of the repo and click on `3 commits` (below the green button with `Code`). This will show the first two commits done on the local repo `Add README.md` and `Add text to README.md` and also the commit done online `Update README.md`.

In the local terminal, execute `git log`.

```sh
git log

###
# commit 81f48fc07e47f264d5912b56104c20852b819e1d (HEAD -> master, origin/master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#    Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

The remote commit `Update README.md` is not shown in the local log.

Open `README.md` in a text editor and change `bla bla bla` to `John Duck lala`. Also create a new file.

```sh
touch info.md
```

Add both files to the list to be added to the commit.

```sh
git add README.md info.md
```

An alternative way to add all files with changes in the current directory is to use `git add .`.

```sh
git add .
```

Commit these changes locally.

```sh
git commit -m "Change README.md, add info.md"
###
# [master 5246933] Change README.md, add info.md
#  2 files changed, 1 insertion(+), 1 deletion(-)
#  create mode 100644 info.md
###
```

Look at the Git log.

```sh
git log

###
# commit 5246933337d680ac44d6137af58eb5b9f6144176 (HEAD -> master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Fri Jun 25 01:18:47 2021 +0200
#
#   Change README.md, add info.md
#
# commit 81f48fc07e47f264d5912b56104c20852b819e1d (origin/master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#   Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#   Add README.md
###
```

In the remote there now are three commits, of which the third commit is `Update README.md`. Locally there are also three commits, of which the third commit is `Change README.md, add info.md`.

Also note that the local Git repo has information about the remote Git repo up to the second commit (`(origin/master)` behind the second commit hash).

Push the commits.

```sh
git push

###
# To https://github.com/bartduisters/git-example-remote.git
#  ! [rejected]        master -> master (fetch first)
# error: failed to push some refs to 'https://github.com/bartduisters/git-example-remote.git'
# hint: Updates were rejected because the remote contains work that you do
# hint: not have locally. This is usually caused by another repository pushing
# hint: to the same ref. You may want to first integrate the remote changes
# hint: (e.g., 'git pull ...') before pushing again.
# hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

This will show a **big fat error**. It tells us the push has failed because there is work on the remote repo that is not yet present in the local repo. And this is correct, `Change README.md, add info.md` is present in the remote repo and not in the local repo. It also says that `git pull` can be executed to get the changes from the remote repo.

```sh
git pull

###
# remote: Enumerating objects: 5, done.
# remote: Counting objects: 100% (5/5), done.
# remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
# Unpacking objects: 100% (3/3), 639 bytes | 63.00 KiB/s, done.
# From https://github.com/bartduisters/git-example-remote
#    81f48fc..db5955a  master     -> origin/master
# Auto-merging README.md
# CONFLICT (content): Merge conflict in README.md
# Automatic merge failed; fix conflicts and then commit the result.
###
```

God dang it! A merg conflict! Git can merge a lot of changes automatically, but not everything. A merge conflict occurs when two commits have changes on a same line in the same file. It shows `Merge conflict in README.md`.

Take a look at `git status`, this will show extra information.

```sh
git status

###
# On branch master
# Your branch and 'origin/master' have diverged,
# and have 1 and 1 different commits each, respectively.
#  (use "git pull" to merge the remote branch into yours)
#
# You have unmerged paths.
#  (fix conflicts and run "git commit")
#  (use "git merge --abort" to abort the merge)
#
# Unmerged paths:
#  (use "git add <file>..." to mark resolution)
#        both modified:   README.md
#
# no changes added to commit (use "git add" and/or "git commit -a")
###
```

Since more commits will occur in the future it is not a solution to use `git merge --abort`. But `fix conflicts and run "git commit"` is a solution.

Open the file `README.md` in which the conflict occurs. This shows total chaos.

```text
<<<<<< HEAD
John Duck lala
=======
bla John Duck
>>>>>> db5955a1a7bb936c7fee06a365203177e721d254
```

Note: Normally it will show one extra `<` and `>`.

Everything between `<<<<<<< HEAD` and `=======` is the local change.
Everything between `=======` and `>>>>>>> [git hash]` is the remote change.

It is the task of the developer to decide what should stay. In case `John Duck lala` should stay, then `<<<<<<< HEAD`, `=======`, `bla John Duck` and `>>>>>>> [git hash]` should be deleted.

The result after solving the conflict.

```
John Duck lala
```

Since a change is made, this change must be added to the list of files to be added to the commit.

```sh
git add README.md
```

Normallly a commit message should be add to the `git commit` command. But since the repo is going through a difficult period in its life (merge conflict), it is sufficient to just type `git commit`. This will automatically open the default text editor. It will automatically add a commit message. It is sufficient to close the file in the text editor and the commit will be added.

Tip: In case a text editor is opened in the terminal, this will probably be `Nano` or `Vim`.

<details>
  <summary>Nano: In case a `^X` and `^R` are shown at the bottom ... Click here!</summary>
  The terminal text editor Nano has opened itself. `^` stand for `CTRL` and `X` stands for the button `x`. Click CTRL + X to close Nano. In case another question is asked, type `y` and click on enter.
<details>

<summary>Vim: In case no `^X` and `^R` are shown at the bottom ... Click here!</summary>
  The terminal text editor Vim has opened itself. This is a modal text editor. In case the mode is changed by accident, click `Escape`. To exit Vim, type `:w!` and click on enter.
</details>

Take a look at `Git log`.

```sh
git log

###
# commit 9d204398f076c28ddd997c56f4b70ec6556bc3c1 (HEAD -> master)
# Merge: 5246933 db5955a
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Fri Jun 25 01:46:41 2021 +0200
#
#    Merge branch 'master' of https://github.com/bartduisters/git-example-remote
#
# commit 5246933337d680ac44d6137af58eb5b9f6144176
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Fri Jun 25 01:18:47 2021 +0200
#
#    Change README.md, add info.md
#
# commit db5955a1a7bb936c7fee06a365203177e721d254 (origin/master)
# Author: Bart Duisters <43420049+bartduisters@users.noreply.github.com>
# Date:   Fri Jun 25 01:07:05 2021 +0200
#
#    Update README.md
#
# commit 81f48fc07e47f264d5912b56104c20852b819e1d
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#    Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

It shows **five** commits. De first two, the third is a remote commit, the fourth is a local commit and finally the fifth one is a merge commit. Also note that `(origin/master)` is no longer behind the second commit, but it is now behind the third commit. The local `master` is placed on the fifth commit. This is because the local `master` is ahead of the remote `master` by two commits.

Executing `git status` will confirm this.

```sh
git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#  (use "git push" to publish your local commits)
#
# nothing to commit, working tree clean
```

It shows `Your branch is ahead of 'origin/master' by 2 commits.`. We already know what to do to sync local commits to the remote repo.

```sh
git push
```
