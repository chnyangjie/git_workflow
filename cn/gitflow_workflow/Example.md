# Example

The example below demonstrates how this workflow can be used to manage a single release cycle. We’ll assume you have already created a central repository.

## Create a develop branch

![PNG](static/06.svg)

The first step is to complement the default master with a develop branch. A simple way to do this is for one developer to create an empty develop branch locally and push it to the server:

```bash
git branch develop
git push -u origin develop
```

This branch will contain the complete history of the project, whereas master will contain an abridged version. Other developers should now clone the central repository and create a tracking branch for develop:

```bash
git clone ssh://user@host/path/to/repo.git
git checkout -b develop origin/develop
```

Everybody now has a local copy of the historical branches set up.

## Mary and John begin new features

![PNG](static/07.svg)

Our example starts with John and Mary working on separate features. They both need to create separate branches for their respective features. Instead of basing it on master, they should both base their feature branches on develop:

```bash
git checkout -b some-feature develop
```

Both of them add commits to the feature branch in the usual fashion: edit, stage, commit:

```bash
git status
git add <some-file>
git commit
```

## Mary finishes her feature

![PNG](static/08.svg)

After adding a few commits, Mary decides her feature is ready. If her team is using pull requests, this would be an appropriate time to open one asking to merge her feature into develop. Otherwise, she can merge it into her local develop and push it to the central repository, like so:

```bash
git pull origin develop
git checkout develop
git merge some-feature
git push
git branch -d some-feature
```

The first command makes sure the develop branch is up to date before trying to merge in the feature. Note that features should never be merged directly into master. Conflicts can be resolved in the same way as in the Centralized Workflow.

## Mary begins to prepare a release

![PNG](static/09.svg)

While John is still working on his feature, Mary starts to prepare the first official release of the project. Like feature development, she uses a new branch to encapsulate the release preparations. This step is also where the release’s version number is established:

```bash
git checkout -b release-0.1 develop
```

This branch is a place to clean up the release, test everything, update the documentation, and do any other kind of preparation for the upcoming release. It’s like a feature branch dedicated to polishing the release.

As soon as Mary creates this branch and pushes it to the central repository, the release is feature-frozen. Any functionality that isn’t already in develop is postponed until the next release cycle.

## Mary finishes the release

![PNG](static/10.svg)

Once the release is ready to ship, Mary merges it into master and develop, then deletes the release branch. It’s important to merge back into develop because critical updates may have been added to the release branch and they need to be accessible to new features. Again, if Mary’s organization stresses code review, this would be an ideal place for a pull request.

```bash
git checkout master
git merge release-0.1
git push
git checkout develop
git merge release-0.1
git push
git branch -d release-0.1
```

Release branches act as a buffer between feature development (develop) and public releases (master). Whenever you merge something into master, you should tag the commit for easy reference:

```bash
git tag -a 0.1 -m "Initial public release" master
git push --tags
```

Git comes with several hooks, which are scripts that execute whenever a particular event occurs within a repository. It’s possible to configure a hook to automatically build a public release whenever you push the master branch to the central repository or push a tag.

## End-user discovers a bug

![PNG](static/11.svg)

After shipping the release, Mary goes back to developing features for the next release with John. That is, until an end-user opens a ticket complaining about a bug in the current release. To address the bug, Mary (or John) creates a maintenance branch off of master, fixes the issue with as many commits as necessary, then merges it directly back into master.

```bash
git checkout -b issue-#001 master
# Fix the bug
git checkout master
git merge issue-#001
git push
```

Like release branches, maintenance branches contain important updates that need to be included in develop, so Mary needs to perform that merge as well. Then, she’s free to delete the branch:

```bash
git checkout develop
git merge issue-#001
git push
git branch -d issue-#001
```