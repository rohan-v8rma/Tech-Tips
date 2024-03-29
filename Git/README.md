# INDEX

- [INDEX](#index)
- [What is a repository?](#what-is-a-repository)
- [Commits in git](#commits-in-git)
  - [Identifying commits (using *hash IDs*)](#identifying-commits-using-hash-ids)
  - [Modifying commits](#modifying-commits)
- [Upstream and Downstream](#upstream-and-downstream)
- [Branches in git](#branches-in-git)
  - [Example to understand branches in git](#example-to-understand-branches-in-git)
  - [What is the HEAD pointer in Git?](#what-is-the-head-pointer-in-git)
  - [Detached HEAD state](#detached-head-state)
  - [Visualizing branching using `ADOG`](#visualizing-branching-using-adog)
  - [Creating \& switching to a branch (`git checkout -b <branchname>`)](#creating--switching-to-a-branch-git-checkout--b-branchname)
  - [Fetching changes from a remote repo (`git fetch <remotename> <branch>`)](#fetching-changes-from-a-remote-repo-git-fetch-remotename-branch)
  - [*Technological uniqueness* in git](#technological-uniqueness-in-git)
  - [Merging changes of another branch into current branch using (`git merge`)](#merging-changes-of-another-branch-into-current-branch-using-git-merge)
  - [Creating a local version of a remote branch after fetching its changes](#creating-a-local-version-of-a-remote-branch-after-fetching-its-changes)
- [Steps for merging a branch](#steps-for-merging-a-branch)
  - [](#)
  - [`git cherry-pick`](#git-cherry-pick)
  - [`git branch -m <newname>`](#git-branch--m-newname)
  - [`git remote add <remotename> <url>`](#git-remote-add-remotename-url)
  - [`git commit –amend -m "new message"`](#git-commit-amend--m-new-message)
  - [`git branch`](#git-branch)
  - [`git remote --verbose` OR `git remote -v`](#git-remote---verbose-or-git-remote--v)
- [Initializing a repository](#initializing-a-repository)
  - [Cloning a repository](#cloning-a-repository)
- [Git commands explained](#git-commands-explained)
  - [`git pull` and `git push`](#git-pull-and-git-push)
    - [Brief description](#brief-description)
    - [In-depth explanation of working](#in-depth-explanation-of-working)
    - [About the names of branches in `git pull` and `git push`](#about-the-names-of-branches-in-git-pull-and-git-push)
      - [In `git pull`](#in-git-pull)
      - [In `git push`](#in-git-push)
- [Format for commit messages](#format-for-commit-messages)


# What is a repository? 

A directory or storage space where your projects can live. Sometimes GitHub users shorten this to `repo`. 

It can be local to a folder on your computer, or it can be a storage space on GitHub or another online host. You can keep code files, text files, image files, you name it, inside a repository.

# Commits in git

Git bases everything off of commits. Everything. 

Everything that isn’t a commit, is just another way of referencing a commit. Branches, tags, etc. are all just references.

While it may seem like one branch is a branch off another branch, your branch is anchored to an upstream or parent commit.

## Identifying commits (using *hash IDs*)

Identifying individual commits is an essential task in version control. 

For example:

- To create a branch, you must choose a commit from which to diverge.
- To compare code variations, you must specify two commits, and; 
- To edit commit history, you must provide a collection of commits.

The most rigorous name for a commit is its ***hash identifier***. 

- The *hash ID* is an absolute name, meaning it can only refer to exactly one commit. 

- It doesn't matter where the commit is among the entire repository's history; the *hash ID* always identifies the same commit.

## Modifying commits

- It is not possible to *change* a commit. 

  - Things that seem to change a commit, really work by adding a *new* commit, that resembles the old one, and then they cover up the old one and show you the new one instead. 

  - This makes it *look like* the commit has changed, but it hasn't. 

- One also can't remove commits, or at least, not directly.

  - All one can do is make them *unreachable*, from branch and tag names and the like. 
  - Once a commit is *unreachable*, Git's maintenance **"garbage collector"** eventually removes them. 
  
  - Making `git gc` remove them ***now*** can be difficult. 
  
    Git tries really hard to let the user get their commits back, even if they want them gone.

But, all of this applies only to commits, hence the rule of thumb: "commit early and often". 

Anything actually committed, Git will try to let one retrieve again later, usually for up to 30 or 90 days.

# Upstream and Downstream

In the context of Git, "upstream" and "downstream" are typically used to refer to the direction of data flow between repositories or branches.

- "Upstream" refers to the remote repository that a local repository is forked from, or the branch that a local branch is tracking. In other words, it is the repository or branch that a local repository or branch is synchronized with when pushing or pulling changes.

- "Downstream" refers to the local repository or branch that is derived from the upstream repository or branch. In other words, it is the repository or branch that receives updates from the upstream repository or branch, and potentially incorporates those changes into its own codebase.

> To give an example, suppose you have a forked repository on your local machine that you cloned from a remote repository on GitHub. In this case, the remote repository on GitHub is the upstream repository, and your local repository is the downstream repository. 
> 
> When you make changes to the code in your local repository and want to update the upstream repository with those changes, you would push your changes upstream. 
> 
> Conversely, if there are updates to the code in the upstream repository that you want to incorporate into your local repository, you would pull those changes downstream.

# Branches in git

Whenever you are working with branches in Git—and you're always working with branches, so this is really just "whenever you're working with Git"—it's important to keep the commit graph in mind. 

The graph, or `DAG` (*Directed Acyclic Graph*), is always there, usually lurking just out of sight.

## Example to understand branches in git

Here's a graph of a repository with nothing but a *master* branch, with four commits on it:

```
o--o--o--o   <-- master
```

- The *master* branch **"points to"** the tip-most commit. In this graph, with newer commits at the right, that's the right-most commit.

- Each commit also points backwards, to its parent commit. 

  That is, the lines in `o--o--o` really should be arrows: `o <- o <- o`. 
  
  But these arrows all point backwards, which is annoying and mostly useless to humans, so it's nicer to just draw them as lines. 
  
  > ***Note***: These backwards arrows are how Git finds earlier commits, because branch names only point to the tip-most commit.

---

## What is the HEAD pointer in Git?

Git also has the name `HEAD`, which is a symbol for the **"current commit"**. 

The way `HEAD` normally works is that it actually contains the *branch name*, and the *branch name* then points to the *tip-commit*. 

We can draw this with a separate arrow:
```
                  HEAD
                   |
                   v
o--o--o--o   <-- master
```
but that takes too much room, so another representation would be:

```
o--o--o--o   <-- master (HEAD)
```

Git will discover that `HEAD` is **"attached to"** *(contains the name)* `master`, then follow the backwards arrow from `master` to the tip commit.

> ***Note***: `git log --decorate` prints the decoration as `HEAD -> master`, when `HEAD` points to `master`. 
>
> When `HEAD` points directly to a commit, Git calls this a ***detached HEAD***, and you might see `HEAD, master` instead. 
> 
> This formatting trick was new in Git 2.4: before that, it just showed `HEAD, master` for both ***detached HEAD*** mode, and ***non-detached-HEAD mode***, for this case. 
> 
> ***Non-detached** `HEAD` can be referred to as an attached `HEAD`, and can be the **"attachment"** can be represented using `master (HEAD)`.

## Detached HEAD state

When `HEAD` points directly to a commit, Git calls this a ***detached HEAD***. Read more about it [here](https://www.cloudbees.com/blog/git-detached-head).

---

## Visualizing branching using `ADOG`

The best command for seeing the graph is *`A DOG`*:

```
git log --decorate --oneline --graph --all
```

See the explanation for each flag over [here](https://expla inshell.com/explain?cmd=git+log+--all+--graph+--decorate+--oneline+--simplify-by-decoration).


Note `origin/master` and `master` are two different branches. Fetching origin/master will not automatically update master as well.

---

## Creating & switching to a branch (`git checkout -b <branchname>`)

The `git checkout -b misc` step creates a new branch name. 

By default, this new branch name points to the current `(HEAD)` commit, so now we have:

```
o--o--o--o   <-- master, misc (HEAD)
```

---

## Fetching changes from a remote repo (`git fetch <remotename> <branch>`)

What git fetch does can be summarized as:

- call up another Git;
- ask it which commits it has; and
- collect those commits, plus whatever else is required to make those commits sensible, and add them to your repository.

> **_Note_**: Fetching is what you do when you want to see what everybody else has been working on.
> 
> After this, if we go to a pre-existing branches using checkout we will get a message that local branch some amount of commits behind the remote branch. 
> 
> We can resolve this issue using `git merge <remotename>/<branchname>`.

So, let's see what happens when you `git fetch origin`. You have this:

```
o--o--o--o   <-- master, misc (HEAD)
```

They have this, which has several extra commits on their master (and we don't care about *their* `HEAD` now):

```
o--o--o--o--o--o   <-- master
```

Your Git *renames* their `master`, calling it `origin/master` on your own end, so that you can keep them straight. 

Their two new commits are added to your repository. 

Those new commits point back to the existing four commits, with the usual backwards arrows, but now it takes more room to draw the graph:

```
o--o--o--o     <-- master, misc (HEAD)
          \
           o--o   <-- origin/master
```

> ***Note***: None of *your* branches are changed. Only the `origin` ones change. 

Your Git adds their [technological uniqueness](#technological-uniqueness-in-git), and re-points *your* `origin/master` to keep track of **"where master was on origin the last time I checked."**

---

## *Technological uniqueness* in git

This is where the SHA-1 IDs of each commit come in. 

The hashes are how Git can tell which commits are unique to which repository. 

The key is that the same commit *always* makes the same hash ID, so if *their* Git has commit `12ab9fc7...`, and *your* Git has commit `12ab9fc7...`, your Git already has their commit, and vice versa. 

The mathematics behind all this and to understand what `SHA` actually is, refer [this](https://www.encryptionconsulting.com/education-center/what-is-sha/) link.


## Merging changes of another branch into current branch using (`git merge`)

The second half of `git pull` is to run `git merge`. 

It runs the equivalent of `git merge origin/master`. 

- The `git merge` command starts by finding the *merge base*, and this is where the graph suddenly really matters.

- The *merge base* between two commits is, loosely speaking, **"the point in the graph where the lines all come back together."** Usually the two commits are two branch-tips, pointed-to by two branch names. 
  
  A typical, and nicely obvious, case occurs with this:
  ```
            o--o      <-- branch1 (HEAD)
            /
  o--o--o--*
            \
            o--o--o   <-- branch2
  ```

- What `git merge` does is to locate the *nearest common-ancestor* commit, which I've drawn as `*` instead of just `o` here. 

  That's the merge base. It's simply the point from which the two branches **"fork off"**.

## Creating a local version of a remote branch after fetching its changes

Once you have fetched a remote branch and it has been created locally as a tracking branch, you can check it out and make changes to it just like any other local branch. Here are the steps to checkout a remote branch that has been fetched locally and commit on it:

- Fetch the remote branch using git fetch:

  ```console
  git fetch origin remote-branch-name
  ```

  This will fetch the remote branch named `remote-branch-name` from the `origin` remote repository.

- Create a local tracking branch for the remote branch using git checkout with the -b option:

  ```console
  git checkout -b local-branch-name origin/remote-branch-name
  ```

  This will create a new local branch named `local-branch-name` that tracks the remote branch `remote-branch-name`.

- Make changes to the local branch as usual using `git add` and `git commit`:

  ```console
  git add .
  git commit -m "my changes"
  ```

  This will stage and commit your changes to the local branch.

- Push the changes to the remote branch using `git push`:

  ```console
  git push origin local-branch-name:remote-branch-name
  ```

  This will push your changes from the local branch `local-branch-name` to the remote branch `remote-branch-name` on the `origin` remote repository.

  Note that you will need write access to the remote repository to be able to push changes to a remote branch.

# Steps for merging a branch 

- Pull remote main to local feature (merging the work of main onto feature) (resolve conflicts in this). 
- Pull local main branch onto feature, then resolve merge conflicts 
- Then push local feature branch to remote feature branch.

```
Git fetch
``` 
This gets the latest commits on `origin/master`.

```
Git checkout master
```
This moves to the `master` branch.

```
Git merge origin/master
```
This merges the latest commits of `origin/master` onto local `master` branch.


## 

Checking out a branch and creating it if not already created


## `git cherry-pick`

## `git branch -m <newname>`

Renames a branch (execute this command while on the branch you want to rename).

## `git remote add <remotename> <url>`

Adding a remote to a repository

## `git commit –amend -m "new message"` 

Modifying a commit

## `git branch`

Display all branches

## `git remote --verbose` OR `git remote -v`

Display all remotes









# Initializing a repository 

When we initialize a repository in local, we use `git init` either manually or initialize using GUI of VS Code. 

After that, when we make the first commit and want to push it, we will have to create a repository in code hosting platform and declaring a remote locally, so that we can push the commits done locally on our branch.

The other way to do it is to create a repository in our code hosting platform using remote commits and clone the repository (using ```git clone <url of repository>```, note that this cloning can;t be done in a folder where git init has been run once)
when we clone a repository and open it in our editor, it will be checked out on the master.

Git repository initialized in local can only be pushed if remote has a blank slate. 
Two different points of origin can be created if we initialize repository with commits in local and remote both.

## Cloning a repository

SYNTAX: ```git clone <url of repository>```

Make sure to open a terminal in the directory you want the folder of the repository to be located, then enter the above command. A folder containing all the files of the repository will automatically be created within the parent folder.
Commits don’t require internet
When we are working on local, we are making changes to our .git folder. Only when we want to interact with the remote, we need internet.
If

Push and Pull (Basically merges)
Whenever we pull or push, we are merging branches since 
Taking pull
git pull origin master



Git branches should always have the same point of origin. If you try to push a branch to the remote which doesn’t have the same point of origination as the remote branch, it wouldn’t be able to reconcile the differences between them (because a branch will just be dangling) and push will fail. Origin point has to be same. 
Filemode in Windows and Linux difference

git config core.filemode false
git config --global core.filemode false
Deleting branches

Git remote add <remote name> <https or ssh verification link>

`git branch -d <branchname>`

if no unmerged(into master) commits are on this branch then this command will work

`git branch -D <branchname>`

force delete for when unmerged commits are present on the branch to be deleted

`git push -d <remotename> <branchname>`

so that the branch is deleted at the remote as well.

search and compare option is available in gitlens, where we can search commits and compare references (references are points in version history, for example: branch, commit sha, working tree)
The branches which are to be compared should have same origination point. 

Commits

Git commit -m “commit message” -m “commit description”

# Git commands explained

## `git pull` and `git push`

### Brief description

- A `git pull` is what you would do to bring a local branch up-to-date with its remote version, while also updating your other remote-tracking branches.

  In other words: `git pull` will always merge into the current branch. So you select which branch you want to pull from, and it pulls it into the current branch. 

- `git push` is roughly the inverse of `git pull`. 

  By default, it updates the remote branch of the same name with any local changes.

  
Read this article for more info:
https://koukia.ca/difference-between-git-pull-and-git-fetch-16e30c3849c9

### In-depth explanation of working

The `git pull` command is actually a combination of two other commands, `git fetch` followed by `git merge`. 

1. In the first stage of operation `git pull` will execute a `git fetch` scoped to the local branch that HEAD is pointed at. 

    By default, git fetch updates all of your remote-tracking branches, which are local copies of the remote branches in the remote repository.

    However, when `git pull` is executed, the `git fetch` command is scoped to the local branch that HEAD is pointing to. 
    
    This means that `git fetch` only retrieves the latest changes for the current branch, rather than for all branches in the remote repository.

    By scoping the `git fetch` command to the current branch, the `git pull` command ensures that only the changes that are relevant to the current branch are downloaded from the remote repository. This can help to minimize the amount of data that needs to be downloaded and can make the git pull operation more efficient.

2. Once the content is downloaded, `git pull` will enter a `merge` workflow. A new merge commit will be-created if auto-merging is successful (manual resolution of merge conflicts might be required) and HEAD updated to point at the new commit.


### About the names of branches in `git pull` and `git push`

#### In `git pull`

The remote and local branch names do not have to be the same. When you use `git pull` to update a local branch with changes from its remote counterpart, Git will look for the corresponding remote branch based on the configuration of the local branch.

- By default, if just `git pull origin` is executed, Git will use a branch with the same name on the remote repository as the remote branch for the local branch. 

  However, you can specify a different remote branch to pull changes from by providing the remote name and branch name as an argument to the `git pull` command. For example, you can pull changes from a remote branch called `feature-branch` in the `origin` remote repository into your local branch (which you are currently on) called `my-branch` using the following command:

  ```console
  git pull origin feature-branch
  ```

- Alternatively, you can configure your local branch to track a specific remote branch using the git branch `--set-upstream-to` command. This allows you to use `git pull` without specifying the remote branch name each time. 

  For example, to set up tracking between your local `my-branch` and the `feature-branch` on the `origin` remote repository, you can run:

  ```console
  git branch --set-upstream-to=origin/feature-branch my-branch
  ```

  After setting up tracking, you can simply use git pull to update your local branch with changes from the remote `feature-branch`.

#### In `git push`

You will usually push your local branch to a remote branch of the same name—but not always.

- Executing `git push` without any arguments pushes changes to a remote branch of the same name; creating it if it doesn't exist. 
  
  Or, if you've used the `--set-upstream-command` to change your upstream branch, it will push to that branch.

- In order to push to a branch of a different name without changing your upstream branch, you just need to specify the branch you want to push and the name of the branch you want to push to separated by a colon (:).

  For example, if you want to push a branch called `some-branch` to `my-feature`:
  
  ```console
  git push origin some-branch:my-feature
  ```

# Format for commit messages

```
[<ticket number>] <change type>: <message>
<change type> : feat, docs, improv/enhance, fix, devops, chore, refactor
```
-	`feat:` A new feature
-	`fix:` A bug fix
-	`style:` Additions or modifications related to styling only
-	`refactor:` Code refactoring
-	`test:` Additions or modifications to test cases
-	`docs:` README, Architectural, or anything related to documentation
-	`chore:` Regular code maintenance


Each subsequent description (second `-m` onwards, the text in quotes represents commit description) has a one line gap from the commit description 

