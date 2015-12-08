You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Bika LIMS Developer Guidelines

***

### Table of Contents

1. [Introduction](#introduction)
1. [Bika LIMS approach to branches and pull requests](#bika-lims-approach-to-branches-and-pull-requests)
1. [The master branch](#the-master-branch)
1. [The develop branch](#the-develop-branch)
1. [Pull requests](#pull-requests)
1. [long-life feature branches](#long-life-feature-branches)
1. [Project code](#project-code)
1. [Best practices](#best-practices)

***

### Introduction

Software development conventions are important in a multi-developer project and help developers read and understand each other's code.

Bika LIMS is a client/community-driven project, and new functionalities are developed based on the requirements of different clients, at different times and with different schedule constraints.  There is no 'in-house' development (product-driven approach), so the challenge is to create a structure to review, merge and release all the different features and bugfixes without affecting or breaking the work already done by others.

If we do this right, it becomes much easier for us all to work together.

Helpful reading:

- [PEP8](https://www.python.org/dev/peps/pep-0008/)
- [Developing for Plone](http://docs.plone.org/develop/index.html)

To get a better understanding of the git branching methods we use, you could also read the following:

- [A successful Git branching model (Vincent Driessen, 2010)](http://nvie.com/posts/a-successful-git-branching-model/)
- [Branch-per-feature (Adam Dimirtuk, 2012)](http://dymitruk.com/blog/2012/02/05/branch-per-feature/)
- [A pragmatic guide to the Branch Per Feature git branching strategy (Katherine Bailey, 2013)](https://www.acquia.com/blog/pragmatic-guide-branch-feature-git-branching-strategy)

If you are new to Git, good luck and welcome to the journey.

***

### Bika LIMS approach to branches and pull requests.

We follow a simple approach to the Branch-Per-Feature (BPF) workflow, and in order to maintain a relaxed atmosphere there are a few important rules that we follow.

#### The master branch

The master branch will always have the most stable version of the latest release, and all bugfixes must be done against this branch. Periodically, a hotfix release (this is the third version digit, e.g. 3.1.9 to 3.1.10) is made from the master branch, and this must be trusted not to cause problems with running systems.

#### The develop branch

New features, non-trivial improvements, and refactoring must be done against the develop branch. The develop branch represents the next feature release (this is the second version digit, e.g. 3.1.9 to 3.2), and so pull requests must be of high quality before being merged.

If a bug is discovered while working on a feature branch, the bug should be fixed in the master branch. Because there is a delay between creation of the bugfix pull request and it's acceptance into master, it's okay to cherry-pick the bugfix commits from the pull request into your current feature branch.

#### Pull Requests

- Pull requests must be associated with a ticket from the [issue tracker](https://jira.bikalabs.com) (Jira), and the ticket number must be included in the commit log. While it may seem pointless to create a Jira ticket for a one-liner or other trivial change, it helps us to keep organised, and to communicate to non-coders the changes that are taking place.

- Each pull request must be for a well-defined feature or bugfix, ideally a relation one-to-one with a Jira ticket.  Pull requests that address multiple bugfixes or features must always be declined.  This includes one-line changes and seemingly "trivial" fixes; these should be separated into distinct pull requests.

- The Pull Request title should match with the title from the ticket (e.g. [Pull Request #1707](https://github.com/bikalabs/bika.lims/pull/1707) relates unequivocally to [LIMS-1989](https://jira.bikalabs.com/browse/LIMS-1989))

- Whitespace and formatting changes may be included in any pull request, but these changes should always be contained in their own discrete commits; no other changes should be included in these commits.

- Pull requests must not be submitted without unit-tests, and may not be merged until all tests pass.  To prevent the reviewer from being required to run or fix tests, the log from all relevant tests should be included in the pull request.

- Pull requests must be merged by a different person than the primary coder.

- Committing directly to the master or develop branches is absolutely not permitted.

- Pull requests with only a part of the bugfix or feature completed may not be merged, and you should never submit a pull request if you know that there is any reason that it will not work.
  
- Always be sure to create or edit the upgrade step if required, and test your code after upgrading from a previous version.

- Remember also to update the CHANGELOG.txt.

- If feature branch: always do a `git merge merge -no-ff develop` to your feature-branch before a pull request.

- If bugfix branch: always do a `git merge -no-ff master` to your bugfix branch before a pull request.

#### Long-life feature branches

Architectural and project-wide changes - these are the things that may cause the worst impact.  Discuss them previously in the list and once a plan is agreed upon, tag them with arch/ prefix and don't expect them to be accepted soon.

Don't base your current development on them unless you want your work to fall into the world of orphans.

During development of these branches, periodically integrate the develop branch to prevent them from diverging too far.

If you are working on one of these branches, use of the [git rerere](http://git-scm.com/docs/git-rerere) extension can be invaluable.

***

### Project-specific code and development

During development for a specific client or project, it's best to fork the bika.lims repository to allow flexible development. We usually setup two different instances (production and test) in client-specific projects, so the best approach is to create two different branches in the project-specific repos: `master` (always synchronised with the production instance) and `wip`, (synchronised with the test instance). From now onwards in this section, when we talk about `origin/master` we'll refer to the project-specific `master`, and `upstream/master` for the project official's master.

#### Project-specific branches

- `origin/master`: always synchronised with project's production instance. Although it can sporadically receive PRs for fixing blocker bugs, in must cases it will only be updated upon project's manager acceptance after his/her review of new functionalities developed in `origin/wip`. Direct pushes are not allowed. After the branch is created for the first time, it will never track the upstream repos again, but will be replaced by `origin/wip` upon acceptance regularly.

- `origin/wip`: this is the project's working branch. `upstream/master` is regularly merged into this branch to guarantee the compatibility with upstream. Accepts any kind of Pull Request, but always following the bpf procedure explained before in this document. Direct pushes are not allowed.

#### Project-specific development basics

All client work should be submitted back to the main repository via pull request as soon as it is baked, unless it is sensitive, incompatible, or it doesn't make sense to have for all clients.

As mentioned previously, all bugs that are discovered during this work should be fixed in the main respository's master branch (unless project-specific bug), via a pull request.

All work that isn't intended to be merged back into the main repository should be created in a separate extension package, included using buildout.  A template for such a package is available at https://github.com/bikalabs/bika.custom

#### Branch per feature creation in project-specific repos

##### 1. Is **a bug, but project-specific**. Only happens in the production server (`origin/master`), but not in `upstream/master`?

Then update your remotes and create a branch directly from `origin/master` like this:

```shell
$ git remote update
$ git checkout -b issue/<LIMS-xxxx-two-three-descriptive-words> origin/master
```

`origin/master` will be merged into `origin/wip` later, so the bug will be fixed there too.

##### 2. Is **a bug, but not project-specific**. The bug comes from the official repository `upstream/master`?

Then, follow the project-non-specific procedures: the `origin/wip` branch will be updated regularly with latest `upstream/master`, so the issue will be fixed in your `origin/wip` as soon as the PR gets approved in `upstream/master` and `upstream/master` is merged into `origin/wip`. If is strictly necessary the bug to be fixed in `origin/master` too, then do a cherry-pick of the commits involved. In order to keep the integrity of the productivity instance, think twice before doing the latter.

```shell
$ git remote update
$ git checkout -b issue/<LIMS-xxxx-two-three-descriptive-words> upstream/master
```

##### 3. Is not a bug, but a new **project-specific feature/enhancement**?

Then, create a branch directly from `origin/wip`. If PR workflow is meant to be used in this particular project, open a Pull Request to `origin/wip`, otherwise merge the branch into `origin/wip`.

```shell
$ git remote update
$ git checkout -b feature/<LIMS-xxxx-two-three-descriptive-words> origin/wip
```

##### 4. Is not a bug, but a new **project-non-specific feature/enhancement**?

Then, follow the project-non-specific procedures. Create a branch hanging from `upstream/develop` and once finished, open a PR to `upstream/develop`. At the same time, you can cherry-pick the commits you've made into your `origin/wip`. This is the trickiest scenario and may require additional branches or procedures to overcome the fact that your `origin/wip` and `upstream/develop` have been diverged. An useful approach is to create to branches: one hanging from `upstream/develop` that will be later for the PR to the official repos and another one hanging from `origin/wip` that will be used to merge the feature in your project. You can do cherry-pick from the first branch to the second branch while you are progressing in the development of the new feature.

```shell
$ git remote update
$ git checkout -b feature/<LIMS-xxxx-two-three-descriptive-words> upstream/develop
$ git checkout -b feature/wip/<LIMS-xxxx-two-three-descriptive-words> origin/wip
```

***

### Best Practice

Adapted from *[Kepler's project development guide](https://kepler-project.org/developers/reference/software-development-guidelines)*

**Try not to hinder other developers**
* Make sure code compiles and passes all tests before committing
* Minimise the impact on other developers, use the branch-per-features approach

**Commits should be neat, portable and documented**
* Indent the code using spaces instead of tabs, 4 spaces per 'tab'
* 80 Characters per line max
* Use meaningful, descriptive names for classes and variables
* Comment classes, methods and everything else. More is better
* Remove stale code not used

**Communicate with other developers**
* Use the IRC channel and dev lists
* Commit frequently, in small and logically related patches with good log messages

In addition, please follow the [PEP-8 Style guide for Python Code](http://legacy.python.org/dev/peps/pep-0008/) in general.

We also expect coders to write short, to-the-point methods that encapsulate a very specific behaviour, rather than long procedural functions.

If your methods are longer that 30-40 lines of code, or if they have extensive conditional blocks or switch statements, break them up into more methods.

Related and equally important, is factoring common procedures into their own classes or methods. When you find yourself writing the same type of functionality more than once, it is time to refactor.
