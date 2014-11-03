You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Bika LIMS Developer Guidelines
***
### Table of Contents
1. [Introduction](#introduction)
2. [Release Cycle](#releases-life-cycle)
3. [Branches and Branch-per-feature (BPF) approach](#branches-and-branch-per-feature-bpf-approach)
4. [Best practices](#best-practices)

***

### Introduction

Software development conventions are important in a multi-developer projects and help developers read and understand each other's code. Please try to follow the guidelines described here. Your cooperation improves the effectiveness of the collaboration.

Please review the community channels for assistance on the [Community](https://github.com/bikalabs/Bika-LIMS/wiki/Community) page if you haven't done so yet.

### Release Cycle

Bika LIMS project developers are committed to [release early, release often (RERO)](http://en.wikipedia.org/wiki/Release_early,_release_often) to get better feedback from the user community and increase the quality of the system both from technical and functional perspectives. Please see [Release Cycles](https://github.com/bikalabs/Bika-LIMS/wiki/Release-cycle) for more details. Pay special attention to section [Releases and Branches](https://github.com/bikalabs/Bika-LIMS/wiki/Releases-life-cycle#releases-and-branches).

### Branches and Branch-per-feature (BPF) approach

Bika LIMS software development uses a Branch-per-feature approach. Before continuing, please read the [*A successful Git branching model (Vincent Driessen, 2010)*](http://nvie.com/posts/a-successful-git-branching-model/), [*Branch-per-feature (Adam Dimirtuk, 2012)*](http://dymitruk.com/blog/2012/02/05/branch-per-feature/) and [*A pragmatic guide to the Branch Per Feature git branching strategy (Katherine Bailey, 2013)*](https://www.acquia.com/blog/pragmatic-guide-branch-feature-git-branching-strategy).

BPF adheres to git-flow with additional constraints:

**Committing directly to hotfix branches is not permitted**
All changes to hotfix branches are submitted as pull requests, and the complete test log (with no failures) must be included in the pull request text.

**Only of trivial modifications may be Committed directly to the develop branch**

When

* Multiple files or functions are modified
* The modification caused refactoring not directly related
* More than 100 lines are changed, also for simple modifications
* You favour a code review or have questions

the commit is not trivial, please branch from develop.

**BPF branches are not be merged into develop until automated tests pass**

**BPF branches by a single coder must be reviewed by at least one other**
And should not be merged into develop by the original coder

#### hotfix/next branch

Only bug fixes and very small enhancements are allowed in the hotfix/next branch. This branch is only used for updates (minor releases) and is regularly merged to develop. 

If you need to fix a bug, create a fork from hotfix/next and fix the bug there. When finished, do a pull request to hotfix/next, including the [Tracker](https://jira.bikalabs.com) ID, a description of the fix and the full  bika.lims Robot Test log after running it. 

Remember also to update the CHANGELOG.txt. 

If all goes well, the Bika LIMS source code maintainers will merge your pull request into hotfix/next.

#### develop branch

The develop branch is the base for new features and enhancements. All branches per features (BPFs) must be created from develop.

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