# INTRO-CS-6 - Source Control Using Git

## Feature branch exercise

This exercise will walk your team through a full release cycle for an example application.
Work in teams of 2 - 5 developers.

## Background

The print magazine _Flavor_ has asked your firm to build and maintain an app so they can share recipes with their hungry audience.

The typical development cycle for the project is as follows:

- On the 1<sup>st</sup> of every month, your team receives a list of each writer's pick for recipe of the month.
- On the 15<sup>th</sup> of every month, the new version of the app is pushed to a staging server where the magazine's editors can do a final review.
- On the last day of the month, the new version of the app is pushed to production.

Development for this project began in January. It is now the beginning of February and the development cycle for `v1.1` of the app has begun.

# 1. Setup

In order to contribute to a GitHub project, you will need two things: a GitHub Fork and a local clone of this project.

### 1 - Fork a Source Repository

Fork the source repository:
   1. Visit https://github.com/generation-org/git-flow-exercise
   2. Click the "fork" button, and choose your personal GitHub account if prompted.

### 2 - Clone your Fork

Clone this project to your local machine and change working directory to the cloned repository.

View existing remotes:
```sh
$ git remote
origin
```

You should see an `origin` remote that points to the source GitHub project:
```sh
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/source-username/repository-name.git
  Push  URL: https://github.com/source-username/repository-name.git
  HEAD branch: master
  Remote branches:
    develop tracked
    master  tracked
  Local branch configured for 'git pull':
    master  merges with remote master
  Local ref configured for 'git push':
    master  pushes to master (up to date)
```

View existing branches:
```sh
$ git branch
# show local branches
* master

$ git branch -r
# show remote branches
origin/HEAD -> origin/master
origin/develop
origin/master

$ git branch -a
# show all branches
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/develop
  remotes/origin/master
```

### 3 - Add Remote for your GitHub Fork

__All Team Members__

Add a `me` remote:
```sh
$ git remote add me https://github.com/your-username/repository-name.git
# add the me remote

$ git remote -v
origin https://github.com/source-username/repository-name.git (fetch)
origin https://github.com/source-username/repository-name.git (push)
me https://github.com/your-username/repository-name.git (fetch)
me https://github.com/your-username/repository-name.git (push)
```

You should see a `me` remote that points to your GitHub Fork repository:
```sh
$ git remote show me
* remote me
  Fetch URL: https://github.com/your-username/repository-name.git
  Push  URL: https://github.com/your-username/repository-name.git
  HEAD branch: master
  Remote branches:
    develop new (next fetch will store in remotes/me)
    master  new (next fetch will store in remotes/me)
  Local ref configured for 'git push':
    master  pushes to master (up to date)
```

All team members will need to pull changes from the source repository in order to branch from feature branches.

Fetch branch data from the `origin` remote:
```sh
$ git fetch origin
From https://github.com/source-username/repository-name
* [new branch]      develop    -> origin/develop
* [new branch]      master     -> origin/master

$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/develop
  remotes/origin/master
  remotes/me/develop
  remotes/me/master
```

---

Please wait until everyone has caught up.

---

### 4 - Configure Local Branches

__All Team Members__

By now you should have noticed that you do not have a local `develop` branch.
```sh
$ git branch
* master
```

Create a `develop` branch that tracks from `origin`'s `develop` branch:
```sh
$ git checkout -b develop --track origin/develop
Branch develop set up to track remote branch develop from origin
```

Notice that viewing the details for the `origin` remote indicates that the local `develop` and `master` branches are configured to push to and pull from the source GitHub repository's branches:
```sh
$ git remote show origin
...
   Local branches configured for 'git pull':
     develop merges with remote develop
     master  merges with remote master
   Local branches configured for 'git push':
     develop pushes to develop (up to date)
     master  pushes to master (up to date)
```


# 2. Feature Branches


`v1.0` was pushed to production yesterday without issue. All code from the release has been merged into the `master` and `develop` branches of the project. The new recipes for February have been sent to the team:

| Writer | Recipe |
| --- | --- |
| Cuba Pudding Jr. | http://allrecipes.com/recipe/228654/quick-oatmeal-pancakes/ |
| Eggs Benny | http://allrecipes.com/recipe/174361/asparagus-with-cranberries-and-pine-nuts/ |
| John Lemon | http://allrecipes.com/recipe/18241/candied-carrots/ |
| Madame Croque | http://www.realsimple.com/food-recipes/browse-all-recipes/roast-pork-sandwich/ |

## :running: Activities

Follow along with the activities below to walk through the process of creating a feature branch and creating a Pull Request to merge your changes into the source repository.

### 1 - Create a Feature Branch

__All Team Members__

Choose a writer &mdash; you will add their recipe to the project within a new feature branch. If there are more writers than people, that is fine; only one feature branch will ultimately be merged.

Create a feature branch off of the `develop` branch that contains the writer's name and the month of the pick:
```sh
$ git checkout develop

$ git checkout -b cuba-pudding-jr-feb

$ git branch
* cuba-pudding-jr-feb
  develop
  master
```


---

Please wait until everyone has caught up.

---

### 2 - Make Changes to the Project

__All Team Members__

In your text editor, make the following changes:

1. Add the new recipe under the [`/app/recipe/feb/`](/app/recipe/feb/) directory.
2. Update the writer's page in the [`/app/writer/`](/app/writer/) directory.
3. Update the main mage [`/app/index.md`](/app/index.md).

Since other people are going to be making changes at the same time, be careful not to make changes to lines of code that are not relevant to your change.


### 3 - Review Changes

__All Team Members__

If you view the current git status, you will see 2 files with unstaged changes and a new folder that has not been tracked by git:
```sh
$ git status
On branch cuba-pudding-jr-feb
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   app/index.md
        modified:   app/writer/cuba-pudding-jr.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        app/recipe/feb/

no changes added to commit (use "git add" and/or "git commit -a")
```

If you run a diff, you should see the changes that have been made to files that are tracked by git:
```sh
$ git diff
diff --git a/app/index.md b/app/index.md
index ac3abad..9777cd5 100644
--- a/app/index.md
+++ b/app/index.md
@@ -8,7 +8,7 @@ Welcome to _Flavor_, the only place on the planet where your taste buds won't be

 ### [Cuba Pudding Jr.](writer/cuba-pudding-jr.md) | cubapud@flavor.magazine

-[Grilled Peach Salad](recipe/jan/grilled-peach-salad.md)
+[Quick Oatmeal Pancakes](recipe/feb/quick-oatmeal-pancakes.md)

 ### [Eggs Benny](writer/eggs-benny.md) | englishmuffin@flavor.magazine

diff --git a/app/writer/cuba-pudding-jr.md b/app/writer/cuba-pudding-jr.md
index 250faea..7315699 100644
--- a/app/writer/cuba-pudding-jr.md
+++ b/app/writer/cuba-pudding-jr.md
@@ -4,4 +4,5 @@

 Recipe Picks:

+- February: [Quick Oatmeal Pancakes](../recipe/feb/quick-oatmeal-pancakes.md)
 - January: [Grilled Peach Salad](../recipe/jan/grilled-peach-salad.md)
(END)
```


### 4 - Stage and Commit Changes

Stage all of the changes that you've made thus far:
```sh
$ git add app/*
# Careful, using a wildcard adds any file that has changed that matches the pattern.

$ git status
On branch cuba-pudding-jr-feb
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   app/index.md
        new file:   app/recipe/feb/quick-oatmeal-pancakes.md
        modified:   app/writer/cuba-pudding-jr.md
```

:bulb: If you run a diff now, you will not see any changes. This is because `git diff` compares all staged changes to unstaged changes.

Now that all of your changes have been staged, commit them with an appropriate message:
```sh
$ git commit -m "Adding Cuba Pudding Jr.'s Feb Recipe"
```

### 5 - Push Changes to Fork

Now that your changes have been committed, let's get them published to your Fork on GitHub:
```sh
$ git push -u me HEAD
```

:bulb: Specifying the HEAD reference instructs git to push to the same branch as the HEAD of your local project, which is currently your feature branch.

:bulb: The `-u` flag instructs git to link your local branch with the remote's branch so that future push & pull commands do not require you
to specify where you would like to push or pull code from.

Navigate to your Fork on Github, you should now see your new branch in the interface.


### 6 - Open a Pull Request

On your GitHub fork, you should see a block at the top indicating that you've recently pushed a branch. If this is visible, click the "Compare & Pull Request" button to the right of your feature branch. If it is not available, choose your branch in the dropdown just above the code directory listing and then click the "New Pull Request" button to the right of the dropdown.

On the Pull Request interface, make sure that the base fork is `source-username\repository-name` and the base branch is `develop`. This means that you are requesting to merge your changes into the `develop` branch of the source repository. At this time also make sure that the head fork and compare branches match your GitHub Fork and feature branch, respectively.

Please draft a message documenting the changes that you've made and then click the green "Create pull request" button.
