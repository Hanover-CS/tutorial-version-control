# Version Control with Git and GitHub

Reference links:

- [Official Git documentation](https://git-scm.com/doc)
- [GitHub guides](https://guides.github.com/)

## What Version Control is and why you should care

**Version control** stores a complete history of the codebase, including information about *what* changed, *when* and by *whom*. This has many advantages:

- Can revert changes that broke something.
- Can identify at what point in time something broke and what changes occured at that time. For each line of code you can literally find out when it was introduced and by whom.
- Can review someone's changes compared to what the code looked like before those changes.
- Can maintain multiple code branches, for example a release/fixes branch, an experimental branch that tries to change something on the server side, another experimental branch that tries to change the UI and so on.
- Can have multiple people contribute to the same code without worrying about overwriting each other's work.
- Can allow people to offer changes, and choose whether you will incorporate them or not. Makes it easy to review what those changes are.

## Git, GitHub and related concepts

- **Git** is a program that works on your computer and maintains a repository of your program's evolution over time.
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
- `push` communicates your newly added local commits to a remote repository.
- `fetch` and `pull` retrieve commits from a remote repository.
- `merge` is used to merge a branch's changes into another branch.
- `rebase` allows restructuring of the commit history and shifting branches so they have a new start location.
- `cherry-pick` allows you to select a commit from anywhere and apply its effects to your current working directory.

Some of these are more advanced. We will now examine them in more detail. This section is broken into parts:

- [Starting a repository](#starting-a-repository)

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

A couple of tools help you with determining your current repository state.

TODO

### Making Commits

How to prepare content for commits, and making commits

TODO

### Working with remotes

Seeing what remotes we have available, pushing and pulling from them.

TODO

### Managing branches

Creating new branches, switching branches.

TODO

## Standard GitHub utilities
