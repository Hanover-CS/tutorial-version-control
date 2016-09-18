# Version Control with Git and GitHub

Reference links:

- [Official Git documentation](https://git-scm.com/doc)
- [GitHub guides](https://guides.github.com/)

## Table of contents

- [What Version Control is and why you should care](#what-version-control-is-and-why-you-should-care)
- [Standard Git operations](#standard-git-operations)
- [Standard GitHub utilities](#standard-github-utilities)
- [More advanced processes](#more-advanced-processes)

## What Version Control is and why you should care

**Version control** stores a complete history of the codebase, including information about *what* changed, *when* and by *whom*. This has many advantages:

- Can revert changes that broke something.
- Can identify at what point in time something broke and what changes occured at that time. For each line of code you can literally find out when it was introduced and by whom.
- Can review someone's changes compared to what the code looked like before those changes.
- Can maintain multiple code branches, for example a release/fixes branch, an experimental branch that tries to change something on the server side, another experimental branch that tries to change the UI and so on.
- Can have multiple people contribute to the same code without worrying about overwriting each other's work.
- Can allow people to offer changes, and choose whether you will incorporate them or not. Makes it easy to review what those changes are.

## Git, GitHub and related concepts

- **Git** is a program that works on your computer and maintains a repository of your program's evolution over time. It was initially created by Linus Torvalds, the creator of Linux, in order to host the Linux Kernel.
    - A **repository** is essentially a *managed* directory on your computer on which git maintains a project. All this information is stored in a hidden subdirectory called ".git".
    - The fundamental building block of any version control system is a **commit**. It contains:
        - Information about which prior commit it is based off.
        - A set of changes to the codebase, often called a **diff**.
        - A timestamp of when these changes where committed.
        - Author information about who committed these changes.
        - A brief message written by the author about the commit.
        - Commits in git are named via md5 hashes. So you'll see commits with names like `160d6693edd0fe306de0a703fe97a8b13908d68b`, and for almost all purposes you can use the first 5-6 characters from that as an identifier of the commit.
    - A **branch** is a series of commits, each based off the previous one. It is usually represented by the last commit in the chain, and you can follow the links from it back all the way to the start. The whole repository has in this sense a tree structure, with the leaves representing different branches.
    - It is very common practice to start a new branch in order to explore some ideas, then discard it or merge it to the main branch as appropriate. Creating a new branch is cheap and easy to do.
    - Git allows the notion of **remote repositories**, which establish a link between this repository and some repository that lives in another place (typically access via the web). This way you can have a "main repository" that all the individual team members' repositories connect to and update. There are various names for these remote repositories depending on use case, like "origin" or "upstream". We will discuss these further later.
- **GitHub** is one of many websites that offer an ecosystem around git repositories. Many other similar websites have similar features as described here.
    - It gives you a place to remotely store your repositories, and then **clone** them into any machine that needs to use them.
    - It allows project management via "issues tracker", with *issues*, *milestones* and *labels*.
    - It facilitates project sharing and contributing to open source projects via *forks* and *pull requests*.
    - It provides a Wiki for project documentation.
    - It offers a way to automatically create a web page from a branch or folder in your project.
    - Your GitHub page is effectively a part of your resume, and employers may look at it.
- When working with a Git repository, there are three important concepts to keep in mind:
    - The *repository*, which holds all the different commits and branches of your project. Typically you are looking at a particular commit at any given time, and that is termed the **HEAD**.
    - The **working directory** is the state of your current directory. Typically you started from a certain commit state at the HEAD of a branch. Then you changed some files, perhaps added some new files. You have not made any commits of these yet. All these constitute your working directory.
    - The **staging area** or **index** is an intermediate state of your changes. Looking at all the files in your working directory that differ from the HEAD, you can choose which ones or which parts of ones to include into your next commit. You then place those into the index. This step is typically called "staging".
    - When you create a commit, that commit is based off the staging area and it contains exactly the differences that you staged into the index.
    - So a typical workflow when working on a particular local issue would be as follows:
        - Switch to an appropriate branch to work on. This turns the entire directory into the state that the head of that branch looks like. The HEAD points to this latest commit in that branch.
        - Pull the most recent changes from the remote repository.
        - Make changes to files, add new files etc.
        - Review these changes, and stage some or all of those stages for commit.
        - Make a commit. This creates a new commit based off of the commit at the HEAD, and also changes the HEAD to now point to our new commit.
        - Rinse and repeat
        - Push the various commits you made back to the remote repository.

## Standard Git operations

Git is a command-line tool, to be precise a collection of such tools. We will describe those tools in this section. It is worth noting however, that there are various graphical interfaces that you can use to access most of these same functions. You can find many of those, many free, [here](https://git-scm.com/downloads). [GitKraken](https://www.gitkraken.com/) in particular looks quite nice. We start with a brief list of the various "verbs":

- `init` starts a new repository.
- `clone` clones/copies a repository, typically to create a local copy of it. The intent is to directly contribute to the repository.
- `fork` copies a repository, with the intent of possibly advancing the project to a new direction. A forked repository does not directly communicate with its "source" repository, while a cloned repository does.
- `status` shows information about the current status of the repository, index, head and other information.
- `add` adds changes to files, or even completely new files, to the staging area/index. A precursor to committing.
- `remove` marks a file to be removed from the repository (and often deleted at the same time). The repository will stop tracking this file.
- `diff` shows you difference between specified commits of your repository and or the index or working directory. Used to determine what will be committed.
- `commit` creates a new commit.
- `log` shows a history of prior commits.
- `stash` allows you to temporarily "hide" changes you've made, and produce a clean working directory. Often needed when changing branches or performing other directory changes.
- `checkout` allows you to switch to a new branch or go to a specific commit. This changes what the working directory and index look like.
- `reset` can clear up the working directory and/or index, and bring them to some specific state.
- `push` communicates your newly added local commits to a remote repository.
- `fetch` and `pull` retrieve commits from a remote repository.
- `merge` is used to merge a branch's changes into another branch.
- `rebase` allows restructuring of the commit history and shifting branches so they have a new start location.
- `cherry-pick` allows you to select a commit from anywhere and apply its effects to your current working directory.

Some of these are more advanced. We will now examine them in more detail. This section is broken into parts:

- [Starting a repository](#starting-a-repository)
- [Reviewing repository state](#reviewing-repository-state)
- [Making Commits](#making-commits)
- [Managing branches](#managing-branches)
- [Working with remotes](#working-with-remotes)

### Starting a repository

There are two main ways to start a repository. You can turn any directory in your computer into a repository via `git init`:
```bash
git init
```
This will turn the current directory into a repository. The other way to start a repository is to `git clone` an existing repository. For instance the following line will clone the GitHub repository hosting this file into a directory called `tutorial-git`:
```bash
git clone https://github.com/Hanover-CS/tutorial-version-control.git tutorial-git
```
This will download the entire state of the repository into that folder, and also set up remote repository links from our local clone to the GitHub repository. This is typically the easiest way around; create a repository in GitHub first, then clone it to your computer.

### Reviewing repository state

There couple of tools help you with determining your current repository state.

#### Status

First off is `git status`.
```bash
$ git status
On branch gh-pages
Your branch is up-to-date with 'origin/gh-pages'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   README.md
```

This tells us some key information:

- We are currently working on the gh-pages branch, and HEAD points to the head of the gh-pages branch.
- Our branch is related to a branch called `gh-pages` in the remote repository called `origin`, and we are currently in sync with it. On some occasions we might be ahead by some commits, meaning we've made some commits that we have not pushed to the remote yet, or the remote might be ahead meaning we have not updated our local repository with that remote information.
- Next we have a list of files that have had changes that are *staged*. When we make a commit, these changes will be committed. In this case it seems we have modified a file, called `README.md`. git also tells us how we would "unstage" a change.
- Next we have a list of files in the working directory that have some changes done to them, but we have not yet committed them. In this case it seems again that the `README.md` file also has some changes that have not been staged. This is an important option that git offers us: Only some of the changes in that file are scheduled for commit right now, and some others are staying put, waiting a later commit perhaps. You can choose parts of a file to stage.

There is also a "short" version of the output:
```bash
$ git status -s
MM README.md
```
The first M says that there are changes in this file that are staged, the second says that there are also changes that are not staged yet.

#### Log

`git log` shows us the history of recent logs. It has lots of options that you can find out by doing `git log --help` or by [reading the documentation](https://git-scm.com/docs/git-log). A particularly nice option is the "oneline" logs:
```bash
$ git log --oneline
3fcd19b Create gh-pages branch via GitHub
8e8ec9e Creating git sections
31b4fa1 Create gh-pages branch via GitHub
ad68152 Start on state section
35362d7 Initial Commit
```

This simply tells us, in chronological order from the most recent to the oldest commit, what the commit's hash is and what the message is.

You can do a lot more with log, like restrict it to only showing commits that changed a particular file:
```bash
$ git log --oneline -- README.md
8e8ec9e Creating git sections
ad68152 Start on state section
35362d7 Initial Commit
```

#### Diff

`git diff` allows us to visually see the differences between commits. Its output might look something like this:
```bash
$ git diff
diff --git a/README.md b/README.md
index 953c5ed..ab7f849 100644
--- a/README.md
+++ b/README.md
@@ -138,6 +138,30 @@ MM README.md
 this is the stuff that was in the file before

-this line was there before and is now removed. The minus in front tells us that.
+this is a new line we are adding.
+This too. And the empty line right below. The plus tells us these are new.
+
 this line existed before
```

A lot of this information is not for human consumption, but you can see that it describes what was added and where in the file it was added: All the lines starting with a plus are new, all the ones starting with a minus are removed, and the others were there before. Most graphical program have a nice color-rich way of showing these differences.

### Making Commits

#### Add

You can stage files for commit via `git add`:

```bash
$ git add README.md
```
This line makes all the changes that were in `README.md` to the staging area, to be committed.

If you want to add all the current changes from all files, you can use a dot to refer to the current directory:
```bash
$ git add .
```

Finally if you want to add parts of a file, you can do an *interactive add*, that brings up a mini interface. You would want to [read the documentation](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging) on how that works.
```bash
$ git add -i
```

Many GUI clients will offer you a way to do that directly. We will be looking at how we can do this with GitKraken in class.

#### Commit

To commit the staged changes, we use `git commit`:
```bash
$ git commit -m "Enter message here"
```
You can also add the staged changes onto the last commit, instead of creating a new one. Very useful if you forgot to include a file:
```bash
$ git commit --amend
```
Be advised that this would change the last commit you have made. If you have already pushed/synced that commit to the remote repository, bad things can happen. *Never make changes to commits that have been synced to a remote!*

#### Reset

Resetting allows you to back out of a commit you made that you did not mean to make. It is in general a dangerous operation, and you should only use it if you are sure you know what you are doing. There are three kinds of resets:
```bash
$ git reset --mixed
```
This is the "mixed" reset, which is the default. This will "unstage" all files that were staged for commit. It does not change or delete the files, it simply no longer marks them as ready for commit. We say that it changes the *index*, but it does not change the *working directory*.

```bash
$ git reset --soft HEAD~
```
This is a "soft" reset. It does not change your index or your working directory. Files that were staged remain staged, and files that have changes keep those changes. But it does change the commit that is at the HEAD, to instead point to the commit called `HEAD~`, which is a notation for the commit before HEAD. Essentially this undoes the last commit. It does not lose the files that that last commit did. It simply puts them back to the "unstaged files" section. It would be as if you had not run `git commit` yet.

```bash
$ git reset --hard
```
This is a destructive step. It will completely remove what is in the index and working directory, and set them both to the HEAD (or another commit if you specify that at the end of the line). You will lose all the changes you have made. On rare occasions, this is the right thing to do. But it should be avoided. It is however useful if you wanted to "rewind" the current branch to a previous commit, because you realized you did not need it. The [documentation](https://git-scm.com/docs/git-reset) offers the following example:
```bash
$ git branch experiment
$ git reset --hard HEAD~3
$ git checkout experiment
```
So what happens here is that we realized that our last 3 commits should have gone in a new branch, called `experiment`, instead of our current branch. The first line creates that new branch based off the current commits. The second line resets our current branch to go back 3 commits. The third line then switches us to the `experiment` branch to continue working on that.

### Managing branches

Creating new branches, switching branches.

TODO

### Working with remotes

Seeing what remotes we have available, pushing and pulling from them.

TODO

## Standard GitHub utilities

TODO

## More advanced processes

TODO
