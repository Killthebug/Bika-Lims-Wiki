You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Bika LIMS Developer Guidelines

***

### Table of Contents

1. [Introduction](#introduction)
1. [Bika LIMS approach to BPF development](#bika-lims-approach-to-bpf-development)
1. [The master branch](#the-master-branch)
1. [The develop branch](#the-develop-branch)
1. [Pull requests](#pull-requests)
1. [long-life feature branches](#long-life-feature-branches)
1. [Project code](#project-code)
1. [Best practices](#best-practices)

***

### Introduction

Software development conventions are important in a multi-developer project and help developers read and understand each other's code. Please try to follow the guidelines described here. Your cooperation improves the effectiveness of the collaboration.  Please review the community channels for assistance on the [Community](https://github.com/bikalabs/Bika-LIMS/wiki/Community) page if you haven't done so yet.

Required reading:

- [PEP8](https://www.python.org/dev/peps/pep-0008/)
- [Developing for Plone](http://docs.plone.org/develop/index.html)

To get a better understanding of the git branching methods, you should also read the following:

- [A successful Git branching model (Vincent Driessen, 2010)](http://nvie.com/posts/a-successful-git-branching-model/)
- [Branch-per-feature (Adam Dimirtuk, 2012)](http://dymitruk.com/blog/2012/02/05/branch-per-feature/)
- [A pragmatic guide to the Branch Per Feature git branching strategy (Katherine Bailey, 2013)](https://www.acquia.com/blog/pragmatic-guide-branch-feature-git-branching-strategy)

### Bika LIMS approach to BPF development

Bika LIMS is a client/community-driven project, and new functionalities are developed based on the requirements of different clients, at different times and with different schedule constraints.  There is no 'in-house' development (product-approach), so the challenge is creating a structure to merge all the different features into releases without affecting or breaking the work already done by others.

We follow a simple approach to the Branch-Per-Feature (BPF) workflow, and in order to maintain a relaxed atmosphere, there are a few important rules that enable us to work without stepping on each other's toes, or setting ourselves up for problems in future.

#### The master branch

The master branch will always have the most stable version of the latest release.

All bugfixes should be done against this branch, using pull requests.

Periodically, a hotfix is released based on the current master branch, and this should be able to apply to all instances without fear of additional features or changes being present in the code.

#### The develop branch

New features, improvements, and refactoring must be done against the develop branch, using pull requests.

The develop branch represents the next feature release, and so pull requests must be of high quality before being merged.

If a bug is discovered while working on a feature branch, the bug must be fixed in master branch, via pull request.  Because there is a delay between creation of the bugfix pull request, and it's acceptance into master, it's okay to cherry-pick the bugfix commits from the pull request into your current BPF branch.

#### Pull Requests

- Pull requests must be associated with a Jira ticket, and the ticket number must be included in the commit log.  While it may seem pointless to create a jira ticket for a one-liner or other trivial change, it helps us to keep organised, and to communicate to non-coders the changes that are taking place.

- Each pull request must be for a well-defined feature or bugfix, ideally a relation one-to-one with a Jira ticket.  Pull requests that address multiple bugfixes or features must always be declined.  This includes one-line changes and seemingly "trivial" fixes; these should be separated into distinct pull requests.

- Whitespace and formatting changes may be included in any pull request, but these changes should always be contained in their own discrete commits; no other changes should be included in these commits.

- Pull requests must not be submitted without unit-tests, and may not be merged until all tests pass.  To prevent the reviewer from being required to run or fix tests, the log from all relevant tests should be included in the pull request.

- Pull requests must be merged by a different person than the primary coder.

- Committing directly to the master or develop branches is absolutely not permitted.

- Pull requests with only a part of the bugfix or feature completed may not be merged, and you should never submit a pull request if you know that there is any reason that it will not work.
  
- Always be sure to create or edit the upgrade step if required, and test your code after upgrading from a previous version.

- Remember also to update the CHANGELOG.txt.

- If feature branch: always do a `git merge merge -no-ff develop` to your feature-branch before a pull request.

- If bugfix branch: always do a `git merge -no-ff master` to your bugfix branch before a pull request.

#### long-life feature branches

Architectural and project-wide changes - these are the things that may cause the worst impact.  Discuss them previously in the list and once a plan is agreed upon, tag them with arch/ prefix and don't expect them to be accepted soon.

Don't base your current development on them unless you want your work to fall into the world of orphans.

During development of these branches, periodically integrate the develop branch to prevent them from diverging too far.

If you are working on one of these branches, use of the [git rerere](http://git-scm.com/docs/git-rerere) extension can be invaluable.

#### Project code

During development for a specific client or project, it's best to fork the bika.lims repository to allow flexible development.

All client work should be submitted back to the main repository via pull request as soon as it is baked, unless it is sensitive, incompatible, or it doesn't make sense to have for all clients.

As mentioned previously, all bugs that are discovered during this work should be fixed in the main respository's master branch, via a pull request.

All work that isn't intended to be merged back into the main repository should be created in a separate extension package, included using buildout.  A template for such a package is available at https://github.com/bikalabs/bika.custom

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
