You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Bika LIMS Developer Guidelines
***
### Table of Contents
1. [Introduction](#introduction)
2. [Releases life-cycle](#releases-life-cycle)
3. [Branches and Branch-per-feature (BPF) approach](#branches-and-branch-per-feature-bpf-approach)
4. [Best practices](#best-practices)

***

### Introduction

Software development conventions are important in a multi-developer projects and help developers read and understand each other's code. Please try to follow the guidelines described here. Your cooperation improves the effectiveness of the collaboration.

Please review the community channels for assistance on the [Community](https://github.com/bikalabs/Bika-LIMS/wiki/Community) page if you haven't done so yet.

### Releases life-cycle

Bika LIMS project developers are committed to [release early, release often (RERO)](http://en.wikipedia.org/wiki/Release_early,_release_often) to get better feedback from the user community and increase the quality of the system both from technical and functional perspectives. Please see [Release life-cycles](https://github.com/bikalabs/Bika-LIMS/wiki/Releases-life-cycle) for more details. Pay special attention to section [Releases and Branches](https://github.com/bikalabs/Bika-LIMS/wiki/Releases-life-cycle#releases-and-branches).

### Branches and Branch-per-feature (BPF) approach

Bika LIMS software development uses a Branch-per-feature approach. Before continuing, please read the [*A successful Git branching model (Vincent Driessen, 2010)*](http://nvie.com/posts/a-successful-git-branching-model/), [*Branch-per-feature (Adam Dimirtuk, 2012)*](http://dymitruk.com/blog/2012/02/05/branch-per-feature/) and [*A pragmatic guide to the Branch Per Feature git branching strategy (Katherine Bailey, 2013)*](https://www.acquia.com/blog/pragmatic-guide-branch-feature-git-branching-strategy).

BPF adheres to git-flow with additional constraints:

1) Committing directly to any hotfix branch is not permitted.  All changes to hotfix branches are submitted as pull requests, and the complete test log (with no failures) must be included in the pull request text.

2) Committing directly to the develop branch is permitted only in the case of trivial modifications. The distinction between trivial and important commits is a subjective one. When

* Multiple files or functions are modified
* The changes caused some refactoring not directly related to it
* More than 100 lines are changed, even for simple modifications
* You feel the need for code review, or have questions

the commit is not trivial, please branch from develop.

3) BPF branches may not be merged into develop until automated tests pass

4) BPF branches written by a single coder must be reviewed by at least one other, and should not be merged into develop by the original coder

#### hotfix/next branch

Only bug fixes and very small enhancements are allowed to hotfix/next branch. This branch is only used for updates (minor releases) and is regularly merged to develop. If you want to fix a bug, create a fork from hotfix/next and fix the bug there. When finished, you can do a pull request to bikalabs' hotfix/next, including the JIRA's ID issue number, a deescription of the fix and the full log obtained after running the bika.lims Robot Tests. Remember also to update the CHANGELOG.txt. 

If all goes well, the Bika LIMS source code maintainers will merge your pull request into hotfix/next.

#### develop branch

The develop branch is the base branch for new features and enhancements. In consequence, all branch-per-features should be created from develop.

### Best practices

Adapted from *[Kepler's project development guide](https://kepler-project.org/developers/reference/software-development-guidelines)*

1. Try not to hinder other developers
    - Make sure code compiles and passes all tests before committing it. 
    - Try to minimize the impact to other developers. Use branch-per-features approach.

2. Commits should be neat, portable and documented:
    - Indent the code using spaces instead of tabs, 4 spaces per tab.
    - Try to use 80-characters per line at maximum
    - Use appropriate, descriptive names for classes and variables
    - Comment the classes, methods and whatever. Better to be in excess than in lack.
    - Remove extraneous code that is not used

3. Communicate with other developers
    - Use the IRC channel and dev lists
    - Commit frequently, in small and logically related patches with good log messages

In addition, try to follow the [PEP-8 Style guide for Python Code](http://legacy.python.org/dev/peps/pep-0008/) in general.

We also recommend that people write short, to-the-point methods that encapsulate a very specific behavior, rather than long procedural functions. If your methods are longer that 30-40 lines of code, or if they have extensive conditional blocks or switch statements, they might be broken up into several methods. But this is very subjective. Related to this is the importance of factoring out common procedures into their own classes or methods; if you find yourself writing the same type of functionality multiple times, it's definitely time to refactor.