---
title: Transtats 0.1.0
type: blog
date: 2017-03-17 15:56:42 +0530
prev: blog/i18n_l10n/
---

From last few months, working on an idea. Globalization market is evolving and we have better translation platforms out in the market, some of them are closed source but some are open. In the whole process, success is defined when users identify some-software xyz-release is now available in so and so languages. The complexity increases when a project involves various packages. Is translation progress of all involved packages up-to-the-mark for the project's next release? [Transtats](http://transtats.org/) is an answer to this!

Though it is just started and in very initial stage, one can use it for tracking translation progress. **Transtats** tries to create mapping between upstream project - translation platform and release streams. You may checkout [docs](http://docs.transtats.org/en/latest/) for deeper insights. It has some inventory on which jobs operate, to create graphs. Lets see few steps how we get started using it.

#### Check Inventory

Graphs will be generated for enabled languages, aliases are used while syncing. One can create a language set, which can be associated with a release branch. Multiple instances of a translation platform can be added. RHEL and Fedora are release streams whereas Fedora 26 is a branch. A release branch should have a language set and a schedule. Transtats jobs sync at intervals for keeping stats and schedule latest.

![inventory](https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/transtats/inventory.png?raw=true)


#### Add and Configure Packages

While adding a package, upstream URL is required. And package name is verified with selected translation platform. Translation of a package can be tracked for multiple release streams. Package can be sync'd while adding itself. Refreshing a package updates its details, translation stats and creates branch mapping. It maps Transtats release branches with most suitable project versions available at translation platform.

![package](https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/transtats/package.png?raw=true)

#### Add Graph Rule

Slug form of rule name would be saved. This should be specific for a release branch. Packages having branch mapping created can only be included here. Either languages would be picked from associated language set with release branch or can be chosen. Somehow if a package is not tracked for a release stream and selected for inclusion Transtats would show an error.

[graphrule](https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/transtats/graphrule.png?raw=true)

#### Experience

Transtats has two graph views: specific to a package in all languages for all branches and another specific to a release branch in chosen languages for selected packages. Latter is a graph rule representation and its accuracy depends on branch mapping of packages. A sync'd package can appear in former view whereas branch mapping is necessary for latter.

![graph](https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/transtats/graph.png?raw=true)

Transtats has reached **0.1.0 release**. If you like the idea and got something to share/contribute feel free to make it [together](https://github.com/transtats).
