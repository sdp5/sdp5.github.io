---
title: Zanata-Python-Client v1.4.1
type: blog
date: 2016-01-14 16:15:14 +0530
prev: blog/i18n_l10n/
---

A new release - v1.4.1 is **out**.<br/> 
This brings feature which makes getting started with **ZPC** more fun. GitHub Release [Link](https://github.com/zanata/zanata-python-client/releases/tag/zpc-v1.4.1)

## What's New
* commands - init, stats - enhances **CLI** experiences.
* Some bug fixes - including critical issues.
* Now the code is more robust, and better to understand.

## Getting Started with Zanata and Python Client
Starting out with translation project is a breeze with Zanata. Let's get started...

> Server Side - User Settings

Point your favorite browser to [```https://translate.zanata.org/zanata/```](https://translate.zanata.org/zanata/) and login (*signup for a new account*). You may find [this](http://zanata-client.readthedocs.org/en/latest/configuration/#user-configuration) useful.

* Navigate to ```Settings``` and see for ```Client```.
* Generate a new **API** key, *ignore if it is already there*.
* Look for user configuration contents for ```zanata.ini```.

*Done! Let's move on...*


> Client Side - CLI tasks

Create a file ```zanata.ini``` at ```$HOME/.config/``` and copy the contents. Now, have **ZPC** in place.

* Download and Install

```sh
$ wget https://github.com/zanata/zanata-python-client/archive/zpc-v1.4.1.zip
$ unzip zpc-v1.4.1.zip
$ cd zanata-python-client-zpc-v1.4.1
$ make install
```

* By now, we should have zanata python client installed. Time to roll translation project.

```sh
$ zanata help
$ zanata <command> --help
```

*Done! Let's move on...*

> Initialize Project Configuration

With ```zanata init``` it's much convenient to specify project details or create a new profile.

```sh
$ zanata init --user-config /path/to/zanata.ini
```

This takes you for an interaction about...

* Project's id, name, description (*option to create a brand new project*).
* Project version (*option to create new version within project selected*).
* Set project type {```gettext``` or ```podir```}
* Set ```translation template``` and ```translation target``` directories.

Aha, just ```ls``` your directory and you can see ```zanata.xml``` created. Now, language resources can be pushed.

*Done! Let's move on...*

> Get Documents Translated

* Push all templates, translations (if flagged) onto server. Cool! translation activities can be started, now. Get translations back, may be to compile lang-packs.

```sh
$ zanata push --help
$ zanata push [OPTIONS] 
$ zanata pull --help
$ zanata pull [OPTIONS]
```

## Tracking Translation Progress

With this version translators can track translation progress for enabled ```locales``` for a ```project-version```. Considering ```zanata.ini``` and ```zanata.xml``` in place, following commands will display stats.

```sh
$ zanata stats
$ zanata stats --help 
$ zanata stats --details    # statistics for all docs in project version
$ zanata stats --word       # include word level statistics as well
$ zanata stats --docid      # statistics will be fetched for doc specified
$ zanata stats --lang       # stats for specified locales will be fetched
```

Happy Translation !!
