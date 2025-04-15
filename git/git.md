# Git and GitHub

Author: **LBO** 🥖

Complementary links:
- Git software website: https://git-scm.com/
- An other git tutorial, by the Software Freedom Conservancy Inc. itself: https://git-scm.com/docs/gittutorial
- GitHub tutorial about SSH keys: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
- Git rebase documentation: https://git-scm.com/docs/git-rebase
- Semantic versioning: https://semver.org/
- Conventional commit: https://www.conventionalcommits.org/

# Introduction

*Git is a **local** version control tool that helps you track changes in files (like network configurations or scripts) on your own computer. Think of it as a time machine: it saves snapshots of your work, lets you undo mistakes, and compare versions over time.*

*GitHub, on the other hand, is a **cloud-hosted** service where you can store Git-managed files online for backup, collaboration, or sharing. While Git runs on your machine, GitHub is just one platform to host Git projects remotely. You can use Git entirely offline, but pairing it with services like GitHub simplifies teamwork.*

*The text above has been written with AI, but is pretty fine! (yeah, I am a modern man myself)*

This tutorial show you the basic git features and propose a way to organize your own workflow when working with git and GitHub.
For those who are familiar with git, I present how to create a branch per issue, and rebase it before merging, while squashing all the commits.
The basics are the parts are the parts I, II and III. Part IV details a way among a ocean of ways to organize your project management. Part V is a list of fun features.

## Requirements for Git

In order to start on the right foot, we need to setup some basic things for this tutorial.

If you are on **Windows** (you chose the hard path):

- Install **WSL** (Windows Subsytem for Linux).
The installation is easy, but will require you to reboot your computer. Just run `wsl --install` in a powershell, and let you guide.
  
- Check if you have Git with `git --version` in a WSL shell.
If you don't ... `sudo apt-get install git`
  
> WSL is a software that allows you to have a linux-like environment. You can manage without it, especially for Git or Python, but good luck developing anything in Windows vanilla.
> That said, WSL is not a perfect solution. It's a patch that will sometime cause you pain.
  
If you are on **MacOS** or **Linux**:

- Check if you have Git with `git --version`.
If you don't ... `sudo apt-get install git` on Linux or ... `brew install git` on MacOS

### Initial setup

The git software will allow you to develop programs with other persons. Thus, you need to indicate *who you are*.

```sh
git config --global user.name "Your Name"
git config --global user.email "youremail@it.iliad.com"
```

We also configure the default branch name, which should be `main`. We will go into the details of branches later. For now, just trust me.

```
git config --global init.defaultBranch main
```

## Requirements for GitHub

Git is a local software, while GitHub is hosted on a server somewhere. Thus, to use it you will need few things.

> Check the GitHub tutorial about SSH keys (at the top of the page).

- Make sure you have a **GitHub account**.

- Generate a pair of SSH keys (or use one you already have).
You can use the `ssh-keygen` CLI tool, as described in the GitHub tutorial.
  
- Setup your **public** key in your GitHub account.
Check the GitHub tutorial about SSH keys

- (Optional) Add you **private** key to your SSH agent.
Check the GitHub tutorial about SSH keys for your platform, as the process is different on Windows, Linux and MacOS. The agent will hold your SSH key, so that you do not need to type its password each time.

# Part I - First local Git repository

In this first part, we will make our first steps with Git. We will fully stay local, and thus, not work together yet. The premises of a great journey awaits ...

## Create a repository

A **repository** is just a fancy name to describe a folder, do not be scared. So to create one, just go with the regular:

```sh
mkdir a-super-and-personalized-name-for-my-repo
```

Ok, I lied in fact, a repo is a folder with a little something. A `.git/` folder should be present inside. You do not need to understand what is within this mysterious folder to use git. Just know that this is where every git related objects are stored.
But this time, we will let Git create it.

For now on, we will work within this folder we just created.
```sh
cd a-super-and-personalized-name-for-my-repo
```

And there is the command to generate the mysterious `.git/` folder within our own folder, turning it into a **repository**.

```sh
git init
```

You can check the content of our repo with `ls -lacv`. You should see something like this:

```
total 12
drwxr-xr-x  3 lbo462 lbo462 4096 Apr 14 11:22 .
drwxr-x--- 19 lbo462 lbo462 4096 Apr 14 11:23 ..
drwxr-xr-x  7 lbo462 lbo462 4096 Apr 14 11:22 .git
```

Cool ! Our `.git/` folder was created. Our working folder is now a repository, but it is empty.

Let's create a file. You can let your creativity flow, but I will not and just create a regular `README.md` file that is usually created for every repo. This file is the main page of the project, written in the markdown format.

```sh
touch README.md
```

I edit the file so that is it not empty. Mine looks like this:

```
# A super and personalized title for my project

Welcome the first super and personalized paragraph of the super and personalized project.
```

You can play around, add some other files and be creative, but I consider that this is a checkpoint of my project. Let us look what I have in my repository ...

```
total 16
drwxr-xr-x  3 lbo462 lbo462 4096 Apr 14 11:30 .
drwxr-x--- 19 lbo462 lbo462 4096 Apr 14 11:33 ..
drwxr-xr-x  7 lbo462 lbo462 4096 Apr 14 11:22 .git
-rw-r--r--  1 lbo462 lbo462  140 Apr 14 11:30 README.md
```

## Commit time

*The following has been written with AI.*

*\*A commit is a saved checkpoint of your files. It’s like taking a snapshot of your work and adding a note (a "commit message") to explain what changed. Once saved, you can always return to this version or compare it to others.*
*Think of it as saving progress in a video game, but for your code or configs. Each commit keeps your work organized and traceable.\**

*Thank you AI, you are nice <3*

In order to commit our work, we have to go through two steps:

- Add the files in staging.
- Actually commit the staged files.

But first, let us check the status of the repo (repo is the kawai name of repository). The `git status` command allows us to do so. This is its output

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

So,
- `On branch main`, which does not matter for now
- `No commits yet`, yes, because we are about to make our first one.
- `Untracked files:` this is the list of files which are *untracked*, meaning that they are not staged yet, and thus, that will not be added to the next commit.
We can see that this is where my `README.md` file is. But I do want it to be added to my next commit ! We will what we can do later ...
- `nothing added to commit but untracked files present`. Well, this is a good summary of the status of our repo.
- `use "git add" to track`. That is what I want ! I want to add my `README.md` file to staging to track it. Let us do it.

```sh
git add -v README.md
```

> If you have other files, and you want to add every untracked files, go with `git add -v -A`.
> `-v` is just for the verbosity of the command.

In my case, it says `add 'README.md'`.
Check `git status` once again ...

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

`Changes to be committed` contains my `README.md` file now. So it ready for the commit. And nothing looks untracked, so let us commit.

```sh
git commit -m "a super and personalized commit message that describe my changes"
```

Keep the habit, check `git status` once again ...

```
On branch main
nothing to commit, working tree clean
```

So ... the commit was effective, right ?
You can list the list of every commits with the `git log` command. Here, the command displays the following:

```
commit 9145bb57ea69a94e8aef27d66a191668129b1b5e (HEAD -> main)
Author: lbo462 <lbo462@mail.com>
Date:   Mon Apr 14 11:57:31 2025 +0200

    a super and personalized commit message that describe my changes
```

So,
- `9145bb57ea69a94e8aef27d66a191668129b1b5e` This is the hash of the commit, nothing fancy, but it is the way each commit is identified.
- `Author: lbo462 <lbo462@mail.com>` Well, that is me, hello
- `Date:   Mon Apr 14 11:57:31 2025 +0200` The date of the commit, which I did the 14th of April
- `a super and personalized commit message that describe my changes` This the message I wrote when I did the commit, remember ?

Just to make sure everything is clear, let us try a **second commit**.

- First, make some changes (add files, edit the existing one, be creative)
Here, I will change the text in my `README.md` file.
  
To check the differences between my last commit and the current state of my repo, I can use `git status` to have a global view, or I can use `git diff` to have more details of what changed. This is the output of `git diff` for me:

```diff
diff --git a/README.md b/README.md
index e8aa6fb..a49f4e1 100644
--- a/README.md
+++ b/README.md
@@ -1,4 +1,4 @@
-# A super and personalized title for my project
+# A super and personalized title for my project but it's edited now

Welcome the first super and personalized paragraph of the super and personalized project.
```
  
- Add the files to staging, with `git add -v -A`
- Commit with `git commit -m "a super and personalized second commit message"`

Check back the list of commits with `git log`.

```
commit 7b87deabe4ceccd11f72b0f1f9c3f465b464b95c (HEAD -> main)
Author: lbo462 <lbo462@mail.com>
Date:   Mon Apr 14 12:11:56 2025 +0200

    a super and personalized second commit message

commit 9145bb57ea69a94e8aef27d66a191668129b1b5e
Author: lbo462 <lbo462@mail.com>
Date:   Mon Apr 14 11:57:31 2025 +0200

    a super and personalized commit message that describe my changes
```

We can see that we now have two commits.

## TL;DR

A repository is just a folder with a `.git/` folder within it, generated with `git init`.

A commit is a checkpoint for a repository.

You see the list of commits with `git log`.

You see the state of your files with `git status`.

You see the diff between the last commit and your files with `git diff`.

Before committing, you need to add your files to staging with `git add -v -A`.

To commit your staged files, use `git commit -m "a significant and impacting message"`.

# Part II - Let us work together

The repo thing is nice and all but meh ... what for ?

Before diving into this part, it is important to understand that Git alone is also useful even when not working with anyone. Remember the commits we did previously. Git allows you to travel back to an old commit, reverse others, keep track of the project history, create versions, and a lot of other advanced features.

> The point is: **Always use Git. Not only when you want to share your project!**

Disclaimer is done, let us connect ourselves to the World Wide Web, or at least, to GitHub.

*Even though this would have been nice, this part will feature no screenshots, as I am too lazy to handle pictures in markdown.*

## Create the repository on GitHub

This is simple, you just need to let you guide by the UI. Connect to the web interface of GitHub (URL of the top of the page) and click the button to create a new repository.

You will need to give a name, an optional description, and a bunch of other options that you can leave with their default value.

For us, the name will be the same of the name of our local repository, but this is not required. So we will call it `A super and personalized name for my repo`.

At first, you will see that the repository is empty, which is expected. The next step will be to push our local repo to this remote one we just created.

## Push the local repository

Do not forget that you will need your SSH key to access the remote repo, and push something to it. If that does not ring a bell, go check back the requirements.

On the GitHub web interface of the newly created repo, you have the option to clone the repo with two different methods: HTTPS and SSH.
Forget about HTTPS, it is a lot more convenient and common to use the SSH one for this tutorial and your future projects. HTTPS is usually better when you don't want to contribute to the project and just want to download it.

So, look for the SSH link to clone the repo, which should look like `git@github.com:namespace/a-super-and-personalized-name-for-my-repo.git`.

We will need to setup our local git repo with this URL to connect it to the remote repo on GitHub.

*Now is the time when you open back your terminal in the folder of our project, aka, the local git repo.*

This link is done with the following command.

```sh
git remote add origin <the-url-for-your-super-and-personalized-repo>
```

- `git remote add` is the command to add a remote repository
- `origin` is the name you are giving to this remote repository, seen from your local git instance
- `<the-url-for-your-super-and-personalized-repo>` is the URL from GitHub.

So now, you have a new remote repo called `origin` that points to the remote repo on GitHub.

And once this is done, pushing your repo to the remote is pretty easy, just do the following command.

```sh
git push origin main
```

- `git push` is the command to push something to a remote
- `origin` is the name of the remote you are pushing to
- `main` is the name of the branch you are pushing

> For now, you have a single branch called `main`. You might not know what it is yet, but you will understand very soon.
> Enjoy this moment, because it is the last time you are going to push anything to the main branch. More details later ... Stay tuned.

You can now go back to the page of the repo on GitHub, refresh it, and check that your files are here. You can check the commits as well, and a bunch of other things that are coming from your local repo.

## Clone the remote repository

In this section, we are drifting a bit out of the road we established before, to slowly start to introduce collaboration.

We place ourselves on the position of someone that did not create the repository but will work on it. This person needs to do to opposite, which is to pull the remote repository to its local computer.
If you want to follow this section interactively, you first need to delete your local repo. Trust me, you won't loose anything.

```sh
rm -vr a-super-and-personalized-name-for-my-repo
```

The local repo is gone, but the remote one holds still. You can verify on GitHub, nothing happened.

In order to fetch the repository and have it locally, we will **clone** it, using the same URL we used in the previous part, when we pushed the local repo to the remote.

```sh
git clone <url>
```

> Remember: the URL can be found on GitHub and looks like something like this: `git@github.com:namespace/a-super-and-personalized-name-for-my-repo.git`
> Use the SSH URL, not the HTTPS one.

This command will create a new folder where you executed the command, named after the remote repository. In our case, as we called the remote repository `A super and personalized name for my repo`, the folder is called `a-super-and-personalized-name-for-my-repo`.

We can go inside it and check its content ...

```sh
cd a-super-and-personalized-name-for-my-repo
ls -lacv
```

```
total 16
drwxr-xr-x  3 lbo462 lbo462 4096 Apr 14 12:07 .
drwxr-x--- 19 lbo462 lbo462 4096 Apr 14 13:42 ..
drwxr-xr-x  8 lbo462 lbo462 4096 Apr 14 12:11 .git
-rw-r--r--  1 lbo462 lbo462  160 Apr 14 12:07 README.md
```

Everything we deleted is now back here. And this would have worked on an other computer as well, of course.

You retrieve everything you had previously, including the commits

```
commit 7b87deabe4ceccd11f72b0f1f9c3f465b464b95c (HEAD -> main)
Author: lbo462 <lbo462@mail.com>
Date:   Mon Apr 14 12:11:56 2025 +0200

    a super and personalized second commit message

commit 9145bb57ea69a94e8aef27d66a191668129b1b5e
Author: lbo462 <lbo462@mail.com>
Date:   Mon Apr 14 11:57:31 2025 +0200

    a super and personalized commit message that describe my changes
```

And, "la cerise sur le gâteau" (the cherry on the cake, as we say in France) is that the remote source is already setup. No need for the `git remote add` command.
You can use `git remote` to check that `origin` is indeed present, and display more details with `git remote show origin`.

## Actual fun, collaborative work

For this section, pair up with someone, create a repository, and have it on your two computers. To do so, one of you need to create the repo locally, push it to GitHub, and the other need to clone it. In the end, you need to have the same repo on your two computers, and a common remote repo on GitHub.

Once this is setup, one of your should make changes, create a commit for these changes and push all of it to GitHub.

Quick recap on how to do so:

*> Guy1*
```sh
git add -v -A
git commit -m "some super and personalized message"
git push origin main
```

On the other side, the second guy see no change on its local repo, and that is normal. The remote repo was updated, but the not local copies.
In order to update its local repo, the second guy need to **pull** to the remote repository, so its local repo is up-to-date with the remote repo.

*> Guy2*
```sh
git pull origin main
```

And now, the second guy should see the same thing as the first. You can check with `git log`.

You can do this process over and over, with as much commits as you want, emitting commits from one side or an other.

## What could go wrong ?

Imagine that one guy did not see that the other did a commit and didn't pull the repo before working ? Or, more common, how can the two guys works on the same file at the same time ? Their commits will interfere with each other, don't they ?

Let us demonstrate the issue.

*> Guy1* makes some changes and commit:
```sh
git add -v -A
git commit -m "some super and personalized commit from guy1"
git push origin main
```

*> Guy2* do not pull the remote repo, and makes some changes and commit:
```sh
git add -v -A
git commit -m "some super and personalized commit from guy2"
git push origin main
```

Output from the second guy:
```
To github.com:namespace/a-super-and-personalized-name-for-my-repo.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:namespace/a-super-and-personalized-name-for-my-repo.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

He couldn't push his commit to the remote, because `the remote contains work that you do not have locally`. The message is pretty clear.

The problem raises from the fact that they are working on the same branch, and share the commits history. If your commits branch is not up-to-date, how can you update it on the remote ?
And could you make it always up to date, without waiting for your colleagues to finish their work ? Are we condemned to work alone on each project ? What is the purpose of Git then ?

## TL;DR

To push a local repository to a remote one, you need to setup a remote source locally using the command `git remote add <name> <url>`.

The chosen name for the default remote is usually `origin`.

To push the branch `main` to `origin`, use `git push origin main`.

To fetch a remote repository and have it locally on any device, use `git clone <url>`.

Use `git pull origin <branch>` to update your local repository with the last commits present on the `origin` remote.

Using only `git push` and `git pull` does not allow multiple persons to work at the same time, we need a better solution.

# Part III - Branches

The solution of the problem we encounter on the part II is solved with branches. We will get into the details of how we can use them, but let first discuss the main concept. *May I get some help for AI for this ?* ...

*\*A Git branch is an isolated workspace within your repository where you can make changes to files without affecting the original version. Think of it as creating a temporary copy of your project to test new ideas—say, reworking a firewall rule or tweaking an automation script. Changes in a branch stay separate until you decide to merge them back into the main codebase. You can create, switch between, or delete branches freely, keeping experiments contained and organized.\**

*Okay, this was not as good as your previous interventions, but it will do.*

To bring back our original case from the part II, how can we make two (or more) persons working on the same files ? Now, the answer appears somewhere in the mist: *branches*! What if one of the guy creates a new branch, and adds its commits to it? The AI said *"without affecting the original version"*, which in our case, is the main branch we have from the beginning. So, adding commits to a branch do not affect the other branches. In other words, the two guys could actually make parallel changes in their branch, without affecting the others. But in that case, what are the correct changes?

What we propose here, is the following:

- We keep the `main` branch as our base, and only add commits to it when it is necessary, so that this branch always reflects the last stable state of the project.
The word **stable** is important here. Someone from the outside should be able to deploy the project from this branch, even if there are some work ongoing on different branches.
- We create a new branch for each issue (fix or feature). All the work that takes place in the other branches are only related to their original purpose: solving the issue they are assigned.
Also, to continue avoiding the problem of branches not up-to-date because someone committed something (problem from part II), each branch should be managed by a single individual.
    
>  Every new branch should take its root in the main branch.

These two rules are simple, but maybe too simple. We will need to add some things later. For now, could our two guys have done better, is they have followed these rules?

- First, they definitely wouldn't have pushed anything to the `main` branch. Why? Because their work as not been reviewed by anyone, how can we make sure that we want their work in the **stable** branch?
- The first guy would create branch for his modifications, and push its commits to it.
- The second guy would do the same.

The result is that we now have three branches, which are:
- The `main` branch, clean from everything
- A branch created by the first guy, with his changes: `branch-1`
- A branch created by the second guy, with his changes: `branch-2`

Okay, way too much talking, let see how they could have done that. Only two steps are required:
- Create the branch with `git branch <branch-name>`
- Move to this new branch `git checkout <branch-name>`

You can check the list of branches, and your current branch with `git branch`.
You can create new branches with `git branch <branch-name>`.

> `git branch <branch-name>` creates a new branch from the current branch you are in. As every branch should takes his root in the main one, we consider that we are always in the branch `main` when calling `git branch <branch-name>`.

In practice, that is what the two guys from part II should have done:

*> Guy1*
```sh
git branch branch-1    # Create a branch called "branch-1" from "main"
git checkout branch-1  # Move to this branch

# go on with the regular committing process ...
git add -v -A 
git commit -m "some super and personalized message related to the changes made by the guy 1"
git push origin branch-1  # we are not pushing the main branch anymore, be careful!
```

*> Guy2*
```sh
git branch branch-2
git checkout branch-2
git add -v -A
git commit -m "some super and personalized message related to another feature by guy 2"
git push origin branch-2
```

We saw earlier that we could update the local repository from the remote using `git pull origin`, but what about branches? No matter the amount of `git pull` the guys will do, they will never be able to fetch the branches from the remote.
*"[...] they will never be able to fetch"* ... **Fetch**, yes, you can do `git fetch origin -v` to fetch the branches from the remote.

## Move between branches

We already saw that the `git checkout` command allows us to change the working branch. Let us play with for few seconds.

Imagine the following scenario:
- I create a repo, with a `main` branch, and add some commits to it. The current repo contains a single file, a `README.md` file containing a simple message `Hello world!`.
- Then, I create a new branch named `1-add-presentation` and checkout to it.

I check the files I have. What do I see ? (try to guess)

*[Contains spoilers ...]* I see no change. Thus, I see a `README.md` file containing `Hello world!`.
Do not think that creating a new branch will delete the commits of the branch it is based on. This new branch `1-add-presentation` still has all the commits contained in the main branch.

- I now edit the `README.md` file and add a new line: `This is where begins my super and personalized presentation ...`. I also create a new file named `presentation.md` but I leave it empty.
- I track the files and commits them.
- And now, I checkout back to the `main` branch.

I check the files I have. What do I see ? (try to guess)

*[Contains spoilers ...]* I see a single `README.md` file containing a simple message `Hello world!`, nothing more.

So ... where are my changes? Where is my `presentation.md` file now? (try to guess)

*[Contains spoilers ...]* They are in the branch `1-add-presentation`, even though they are not visible anywhere in your files. In fact, the commits holding all the necessary data to re-apply these changes are within the mysterious `.git/` folder.

And finally, what do I see after I do `git checkout 1-add-presentation`?

*[Contains spoilers ...]* Well, you see the `README.md` file with the line `This is where begins my super and personalized presentation ...` and the `other.md` empty file as well.

If that demo is not clear for you, play around with branches and commits so that you accommodate to it. You can even ask question to your favorite AI who can probably help you on this.

## TL;DR

We can solve the problem of working together using branches assigned to an issue and a single person.

We keep a `main` branch as clean as possible so that the project is always stable on this branch.

Every branch should be created from the main branch.

You can create a branch with `git branch <branch-name>`. Its base will be the current branch you are in.

You can see the list of branches and your current branch with `git branch`.

You can change the current branch with `git checkout <branch-name>`.

Now we still need to talk about how we merge branches, and we will need GitHub.

You can fetch remote branches with `git fetch origin -v`.

# Part IV - Work with GitHub: issues & pull requests

The previous part was fully local, we didn't discuss about GitHub. Even if merging branches can be done locally with Git, we will not see that in this part. Instead, we will focus about project management through GitHub, and that is where we will be merging branches, with a nice UI.

Remember what we said previously? *"[...] branches assigned to an issue"*. Well, how do we define what an `issue` is? *I am calling AI once again ...*

\**An issue is a trackable task in GitHub (or similar platforms) used to document a bug, request a feature, or plan work (e.g., "Update firewall rules" or "Fix VLAN config"). It acts as a central thread for discussion, progress tracking, and linking to code changes. Branches are often tied to issues to keep work organized—each branch solves one specific problem, ensuring changes stay focused and traceable.*\*

*Yes, AI is right.* Let us make a bullet point summary:
- We create an issue on GitHub to keep track of a task (a fix, a feature, or something else ...)
- We create a branch that we assign to the issue.
When creating an issue on GitHub, you can directly create an assigned branch to it, and then fetch it locally.
- The commits that are related to the task take place inside the assigned branch

Okay, but there is something we need to discuss: 

> *How to update the main branch if we never work on it?*

We need to introduce the \*`pull request`\*. It is request to merge a branch to an other.
When a task is finished (`issue` is resolved) one can create a pull request to ask to merge his branch (the one assigned to the issue) to the `main` branch.

> A pull request can also be called a *merge request* on certain platforms, like GitLab. It is often referred by PR (Pull Request) or MR (Merge Request).

This pull request allows to have someone to:
- Review the changes
- Approve (or not) the changes

Once the pull request is approved, the branch can be merged with a simple click on the web interface.

## Merging styles

There are different merging styles, and we are not going to through all of them. Instead, we will focus on a single one, and explain why this is a great idea.

This is the basic scenario:

```
A---B main
     \
      C---D---E branch-1
```

The above scheme describe a `main` branch with two commits `A` and `B`, and a diverging branch `branch-1` that adds three commits `C`, `D`, and `E`.

The classic merging style will lead to this, after merging:

```
A---B---C---D---E main
     \
      C---D---E branch-1
```

> Here, we did not delete the `branch-1`. When merging, this is not always automatic.
> To delete the remote branch (on GitHub), you will need to use the web interface.
> To delete the local version of the same branch, you can use `git branch -d <branch-name>`.
> In the rest of the tutorial, we consider that you always delete merged branch.

After deleting the branch `branch-1` with `git branch -d branch-1` (locally) and using the UI to delete its remote counterpart, we then have the following.

```
A---B---C---D---E main
```

A single branch with all the commits of the old branch `branch-1`, inside `main`. We finally updated the branch `main`, but do we really want it like this? Here, we had only three commits in the branch `branch-1`, and so it looks pretty fine. But what if we have more of them, like 10 or 20? The commit history of the branch `main` will quickly become unreadable, and it is going to be hard to understand what commit was for what issue. Instead, we are going to `squash` the commits before merging, making every commits of the branch, a single one, like this:

```
A---B main
     \
      F branch-1
```

and then merge to obtain this:

```
A---B---F main
```

> We deleted the branch `branch-1`, as described above.

The commit `F` contains the changes of the commits `C`, `D`, and `E`, so no work is lost. Now, imagine that the project goes on, and we add more and more commits to the branch `main`. We will easily be able to look back in time to check what were the changes introduced by the PR that produced `F`.
That way, you always keep a clean and linear history on the branch `main`, where each commit correspond to an issue that was solved.

## What could go wrong ?

We are going to introduce new persons working on the project, on different issues. We can imagine the following scenario:

```
A---B main
    |\
    | C---D---E branch-1
    \
      I---J---K branch-2
```

> To avoid fancy weird tree, we **always** create diverging branches from the branch `main`. Trust me, you do not want to do otherwise!

Okay, the issue #1 is solved, the PR is accepted. We then have:

```
A---B---F main
     \
      I---J---K branch-2
```

Do you the problem we have here? How will we be able to merge `branch-2` into to the main branch since a new commit was added to it? The `branch-2` does not have the changes introduced by the commit `F`.

This is where you have two solutions:
- Try to merge anyway and solve merge conflicts directly in the GitHub UI, and hope you didn't break anything
- Rebase `branch-2` so that it starts from the last commit of `main`, and do a regular clean merge.

Even if the two options exists, I strongly recommend the second one, that allows you solve the conflicts locally, so that you can verify that everything still works after the process.

Okay, I create a new section for this as it is a bit tricky.

## Rebasing

> You can check the documentation of `git rebase` for more details. Link at the top of this tutorial.

We define our objective. We have this:

```
A---B---F main
     \
      I---J---K branch-2
```

and we want this:

```
A---B---F---L main
```

With `L` containing all the changes of `I`, `J` and `K`.

But for that, we need few intermediary steps, which are:

1. Rebase `branch-2` to `main` (we will go into the details after)

```
A---B---F main
         \
          I---J---K branch-2
```

2. Squash the commits of `branch-2`

```
A---B---F main
         \
          L branch-2
```

3. Finally merge `branch-2` to `main`

```
A---B---F---L main
```

Clear, right? You already know how to do the steps 2 and 3, so we need to discuss the first step: `rebase`. Ans that step is be done fully locally, no need to use GitHub for this.

But first, you need to make sure that your local branch `main` is up-to-date with the remote branch `main`:

```sh
git checkout main
git pull origin main
```

Next, go back to the branch you want to rebase:

```sh
git checkout branch-2
```

And finally, use the `git rebase` command to rebase your branch from the branch `main`:

```sh
git rebase main -i
```

> `-i` stands for *interactive*, which mean that you will have to do something, let see what.

You will have an editor with the following content:

```
pick ccf1d7a I
pick b28aee1 J
pick 93e294a K
```

> The number between `pick` and the commit message is a shortened version of the commit's hash, which is its identifier.

Here, you can see the commits `I`, `J` and `K`, and they are set to `pick`. That means that they all will be taken for the rebase.
The order is also important here. The rebase works in the following manner: Git starts from the branch `main`, which the last commit is `F`, then try to apply the changes of the commit `I`, then `J` and finally `K`. So for three commits, there will be three steps, one per commit. Conflicts can occur in any of these steps.

We keep the default config we are provided, and close the editor. But, we have a merge conflict to resolve:

```
Auto-merging file.md
CONFLICT (content): Merge conflict in file.md
error: could not apply ccf1d7a... I
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply ccf1d7a... I
```

Let's see ...

```
Auto-merging file.md
CONFLICT (content): Merge conflict in file.md
```

Means that git could not automatically merge the file `file.md`, and that this is where I will need to solve the conflict. Let us check the content of the file:

```
<<<<<<< HEAD
# Some test

Coucou
=======
# Some test, again
>>>>>>> ccf1d7a (I)
```

It seems that it was edited by git, and that two sections are defined:

```
<<<<<<< HEAD
# Some test

Coucou
=======
```

This section, which is present on `HEAD` (here, `HEAD` is at the commit `F`), and this section:

```
=======
# Some test, again
>>>>>>> ccf1d7a (I)
```

Which comes from the commit `ccf1d7a (I)`, so the first new commit of `branch-2`.

Basically, we need to choose between this, which comes from `F`:

```
# Some test

Coucou
```

and this, which comes from `I`:

```
# Some test, again
```

To resolve the conflict, I just to edit the file with I want to have as my final version. Here, I want a combination of the two, so I can just edit `file.md` and leave it like this:

```
# Some test, again

Coucou
```

> Note that I removed the `<<<<<<< HEAD`, `=======` and `>>>>>>> ccf1d7a (I)`.

The file is now what I want it to be, so I add it to stage with the classic `git add`.

```
git add file.md
```

And then, I can continue with my rebase, which is still on-going.

```
git rebase --continue
```

A new editor pops in, that wants it to edit the commit message for `I`, since the commit resulting of the solved conflict is not `I` anymore, but something else. I could keep the same commit message, but to make things clear, I will change it with `I'`.

Other merge conflicts can occur in the next steps, and you will have to do the same as I explained here. At the end, I get a happy:

```
Successfully rebased and updated refs/heads/branch-2.
```

If I do `git log`, I now get:

```
commit d67e85e5a0215c34794a204cca4fc19e96c92db5 (HEAD -> branch-2)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:07:27 2025 +0200

    K

commit 1b9c71b6f43e8ce68599aa3c220388535fcf35b1
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:07:10 2025 +0200

    J

commit 37664f30793fe6e921aaa6c0d73d4957c616eec7
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:06:50 2025 +0200

    I'

commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

And If I do `git log` after checkout to `main`:

```
commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (HEAD -> main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

Which means that this is my current state:

```
A---B---F main
         \
          I'---J---K branch-2
```

> `branch-2` has been rebased, and `main` was not affected whatsoever.

As we can see, `I'` is the result of the solved conflict between `F` and `I`, but this could have occured anywhere else, like at `J` or `K`, or even at multiple places, leading to slight changes in the commits I am rebasing.

> Now, the history seems ready for the merge, but it is important to check that your project is still working here, on `branch-2`, before merging. We solved merge conflict, the code has changed, it is important to run tests before going further.

What we did was fully local, so let push everything to the remote `branch-2` now.

```sh
git push origin branch-2
```

```
To github.com:namespace/a-super-and-personalized-name-for-my-repo.git
 ! [rejected]        branch-2 -> branch-2 (non-fast-forward)
error: failed to push some refs to 'github.com:namespace/a-super-and-personalized-name-for-my-repo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. If you want to integrate the remote changes,
hint: use 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Oh ... it failed, that is annoying, but expected. We just rewrote history, GitHub does not like that. We are not just adding a new commit, we are also editing the existing ones. GitHub warns us here, but we know what we are doing. So go ahead, and push that branch anyway.

```sh
git push origin branch-2 --force
```

```
[...]
To github.com:namespace/a-super-and-personalized-name-for-my-repo.git
 + 60a9ac3...1087caa branch-2 -> branch-2 (forced update)
```

`(forced update)`, yes I know, sorry.

Now, when you will click on the button to squash and merge on GitHub, you will not have any conflict to resolve, and your history will be linear, and you will finally obtain what you wanted from the beginning.

```
A---B---F---L main
```

## TL;DR

You create an issue on GitHub for each task implying changes, like fixing something or adding a new feature.

You create a branch, then you assign this branch to an issue, on GitHub.

Every commits of a branch are related to the issue the branch was created for.

You need to create a new *pull request* to merge a branch to the main one, always on GitHub.

To keep a linear history, every branches should be rebased before merging.

Before locally rebasing, do not forget to update the main branch with `git checkout main` then `git pull origin main`.

To rebase a branch use `git checkout <branch>` and then `git rebase main -i`.

You might need to solve conflicts during the process.

After rebasing, you need to force push your branch to GitHub with `git push origin <branch> --force`.

It is important to locally test the project before merging, especially after a rebase, when one is needed.

The PRs can be reviewed and approved by pairs.

We squash commits before merging, which can be done automatically by GitHub.

We delete the branches that were merged, especially on GitHub.

# Part V - Advanced features

You already have what you need in the previous part. This one lists some more advanced features of git.

> This is not an exhaustive list of things that are possible, but a small list of very useful features you might need one day.

## Git log and HEAD

*AI call ...*

*\*HEAD is Git’s current position marker, like a digital bookmark pointing to the commit or branch you’re actively working on. It automatically tracks your latest changes (e.g., edits to a config file) or the branch you’ve checked out. When you switch branches or revisit past commits, HEAD updates to reflect where you are in your project’s timeline, ensuring edits and commits happen in the right context. Think of it as your “you are here” label in the repository’s history.\**

When I do `git log`, I can see this:

```
commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (HEAD -> main)
```

The `(HEAD -> main)` means that `HEAD` is pointing to the last commit of `main`, so that I am at the last commit of `main`.
If I am on an other branch in advance of `main`, I can see this:

```
commit 1ccde84beca97b30348e48a894b748338d0cd59b (HEAD -> branch-2)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:06:50 2025 +0200

    X

commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F
```

Here, we see that `main` is at the commit `F` and `branch-2` is at `X`. And I am at `HEAD`, so I am at `X`, the last commit of `branch-2`, which is not included in `main`.

When you call `git checkout`, you are only moving `HEAD` to different positions. But you need to be aware of the `detached HEAD` mode.

```
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
```

This happen when you checkout to a commit directly, not to a branch. This is like a "ghost mode" where you can wander around back in the past to see the state of the project at previous commits. You can escape from this mode by doing `git checkout <branch>` which will take you back to the last commit of a branch.

In this mode, when you do a git log, you will a slight change:

```
commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (HEAD, main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

Can you see it ? We previously had `(HEAD -> main)`, but we now have `(HEAD, main)`. This means that `HEAD` is at `main`, but does not point to it. In other words, you are not attached to `main`: you are in `detached HEAD` mode. You can change that with `git checkout main`, which will bring back the good old `(HEAD -> main)`.

> You can use the HEAD pointer when you want to refer to a commit, instead of its hash. For example `HEAD^` is the commit just before `HEAD`, `HEAD~3` is the commit that is three commits behind of `HEAD`, etc.

## Git stash, handle uncommitted changes

This is probably one the most useful features I will present here. It allows you to save some changes somewhere and bring them back later.
One important thing that you will discover soon, is that you need to have no on-going changes when travelling inside a git tree. Here is what I get when I do some uncommitted changes and try to use `git checkout`:

```
error: Your local changes to the following files would be overwritten by checkout:
        file.md
Please commit your changes or stash them before you switch branches.
Aborting
```

But what if I am currently working and I don't want to commit now, nor abort my changes, but I still need to check something quickly somewhere else in the git tree?

```sh
git stash
```

This will create a `stash` object containing your changes. Once you have done this command, every changes from the previous commit are reverted and saved inside this `stash` object. You can list these objects with `git stash list`.

Now, you are on a clean working git environment, and you can checkout as you like, where you want.

In order to apply the last saved `stash` object, you can do

```sh
git stash apply
```

You can find out more about this with `git stash --help`.

## Git tags

Git tagging is used in every library project, to release versions of a project to others. A tag usually looks like a version number, like `1.0.3` or `v2.4.2`.

> Make sure to respect semantic versioning when you create a new version of you project. You can find a link about it at the top of this document.

In git, you tag **commits**. As we want to use the branch `main` as our main branch for releases, we will only tag commits of this branch. Tagging a commit if done using the command `git tag`.

Example ... we have the following history:

```
commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (HEAD -> main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

And we consider that the commit `F` is our first release, and we want to tag it the version `0.0.1`.

```
git tag 0.0.1
```

This wil tag the `HEAD` commit with the tag `0.0.1`. We now see:

```
commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (HEAD -> main, tag: 0.0.1)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

We see that the commit `F` is tagged with `tag: 0.0.1`. And this will stay like this, even when I add commits later on, like here:

```
commit 6bb79fea1ec9e91ccaa792ae15c4045364a7bb9b (HEAD -> main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 15:35:39 2025 +0200

    X

commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (tag: 0.0.1)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

`F` is still tagged with `tag: 0.0.1`, and still correspond to still version, no matter what comes after it.

To push a tag to a remote, simply use `git push origin <tag>`, like here:

```sh
git push origin 0.0.1
```

You can also checkout to tags with `git checkout <tag>`.

> Tags can be used as conditions to trigger CI/CD jobs that deploys new releases, and are a great way to keep track of a project versions.

## Reset to previous commit

What if you want to go back in time but not in detached HEAD mode? Let's say you want to revert one or several commits. In other words, you want to reset the tree to a previous commit. For that, you can use `git reset`, with one of the two options:

- `--soft`: This reverts to a commit, but keeps the changes of the deleted commits as unstaged
- `--hard`: This reverts to a commit, but delete the changes of the deleted commits

When resetting, you reset **to** a previous commit. For example, if I have the following history:

```
commit 1ccde84beca97b30348e48a894b748338d0cd59b (HEAD -> branch-2)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:06:50 2025 +0200

    X

commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

And I do

```sh
git reset --hard e7efcf2683c9ee9c4eda834fe84c5517a5adb95d  # commit F
```

I will be left with

```
commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

and no unstaged changes, which means that my files are back to their previous version, like if the commit `X` never existed.

If I did

```sh
git reset --soft e7efcf2683c9ee9c4eda834fe84c5517a5adb95d
```

I would have the same history, but the files would not have changed, so I would have unstaged things to commit, that correspond to the lost commit `X`.

> You can do `git checkout .` to go back to the last commit, reverting all your changes. This will have the same effect as `git reset --hard HEAD`

> Take note that nothing is never really deleted with Git, and that you can refer to orphan commits with their hash.

## Re-apply or revert previous commit

Cherry-picking is a less used feature that allows you to replay a previous commit. The concept is pretty simple. You can do the following:

```sh
git cherry-pick <commit>
```

This will create a **new** commit with the same changes as the selected commit, at the tip of `HEAD`. Note that some conflicts might occur here.

```sh
git revert <commit>
```

Is doing the same but creates a new commit that **reverts** the changes of the selected commit, which is often more useful than cherry-picking.

## Squashing locally

In the previous part, we saw that we wanted to squash commits when merging a branch. GitHub allowed us to do so seamlessly. But maybe, you will want some day to squash commits locally using only `git`.

For that, we will use a command we already used previously, which is `git rebase`. But this time, you will rebase on a commit that you will clearly identify. To identify a commit, we need its hash. We can read it in the `git log`.

```
commit d67e85e5a0215c34794a204cca4fc19e96c92db5 (HEAD -> branch-2)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:33:27 2025 +0200

    K

commit 1b9c71b6f43e8ce68599aa3c220388535fcf35b1
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:33:11 2025 +0200

    J

commit 37664f30793fe6e921aaa6c0d73d4957c616eec7
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:06:50 2025 +0200

    I'

commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

Let say I want to squash the commits from `I'` to `K`.

I have:

```
A---B---F---I'---J---K
```

I want:
```
A---B---F---X
```

That means that I need to replay every commits starting from `F`, and squash them into a new commit `X`. The ID of `F` is `e7efcf2683c9ee9c4eda834fe84c5517a5adb95d`. I can use the `git rebase` command this way:

```sh
git rebase -i e7efcf2683c9ee9c4eda834fe84c5517a5adb95d
```

I am then showed every commits that come after `F`, in a text editor:

```
pick 37664f3 I'
pick 1b9c71b J
pick d67e85e K
```

> This is similar than what we did in the previous part when rebasing a branch, right ? We used the command `git rebase -i main`.
> In fact, rebasing always starts from a commit. When selecting a branch, like `main`, we are just selecting the last commit of that branch. Manually passing the ID of the last commit of `main` would have been the exact same.

By default, it wants me to pick every commits that comes after `F`. But here, I do not only want to just apply them. I am on a single branch with a linear history. Leaving the values by default would have no impact. Instead, I changed what I have with this:

```
pick 37664f3 I'
squash 1b9c71b J
squash d67e85e K
```

This means that:
- I want to keep the commit `37664f3 I'`
- I want to merge the commit `1b9c71b J` with the previous one, which is `37664f3 I'`
- I want to merge the commit `d67e85e K` with the previous one, which is `37664f3 I'` since `1b9c71b J` was squashed as well.

The result will be a single commit containing `I'` where `J` and `F` are squashed.

It proposes me to edit the commit message for the new commit I am creating. I will write `X`.

And there is the result in `git log`:

```
commit 1ccde84beca97b30348e48a894b748338d0cd59b (HEAD -> branch-2)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:06:50 2025 +0200

    X

commit e7efcf2683c9ee9c4eda834fe84c5517a5adb95d (main)
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:09:53 2025 +0200

    F

commit 66a43b4e62a990c01df5af626c060564d225981e
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:58 2025 +0200

    B

commit 3eb7d53a4a785608119810dc58dd832f5da99591
Author: lbo462 <lbo462@mail.com>
Date:   Tue Apr 15 11:04:34 2025 +0200

    A
```

I now have:
```
A---B---F---X
```

`X` indeed contains the changes from the commits I just squashed, so my files did not change, only the git commits.

To push that to the remote, I will need to do a forced update for the same reasons as previously when we rebased branches.

# Some good practices while working with git

You want some more information? Well, remember that there is a lot of documentation on the web about git, so feel free to read about it, and to play around with some local repository. This is the only way to truly understand the essence of git. Now comes some advices ...

- Commit as much as you want, and even more.
Remember that you are always working on separated branches that will be squashed. Commit often, and always push your changes the remote, so that you never loose work. It will also facilitate you the use of certain command like cherry-picking or revert that will be more precise.
- Use the conventional commit for merges to the main branch. It is a convention to write commit messages. It is widely used and allows to clearly define what a commit does, and its range. You will find a link at the top of the page.
- When creating a branch from the main branch, or while rebasing, make sure your that your local branch is up-to-date with the remote counterpart. It will avoid you problems later.
- You can use some fancy tools like VSCode or GitKraken to deal with git, but I do recommend to use with its regular CLI, at least when you start, so that you can truly grasp the essence of git.