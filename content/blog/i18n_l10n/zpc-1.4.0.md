---
title: Zanata-Python-Client v1.4.0
type: blog
date: 2015-12-15 16:40:42 +0530
prev: blog/i18n_l10n/
---

**ZPC** is a convenient way to communicate with Zanata server to push text for translation and pull translated text back for inclusion in software or documentation builds. This release brings fresh experiences to translators and contributors. GitHub Release [Link](https://github.com/zanata/zanata-python-client/releases/tag/v1.4.0)

## What's New
* Use latest list of languages defined in the server for a project when using the client.
* Should be able to specify minimum percentage of messages translated on pull.
* Keep source and target dir paths in `zanata.xml`
* Code improvements for better maintainability.

## Translators
I am sure, your search for translations platform, which can cater all your requirements, would come to an end, when you look at [Zanata](http://zanata.org), and if your project is already here - we have some new exciting features waiting for you to check them out. Let's get started...


> Preparation

Keep your language resources ready (POT file/s or folder/s). Have `zanata.ini` in-place, details [here](http://zanata-client.readthedocs.org/en/latest/configuration/#user-configuration). Write your project configuration `zanata.xml` and keep it at root directory of your project, this helps ZPC to have some prior information of the project while talking to Zanata Server. Structure would be...
```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<config xmlns="http://zanata.org/namespace/config/">
        <url>ZANATA_SERVER_URL</url>
        <project>PROJECT_ID</project>
        <project-version>PROJECT_VERSION_ID</project-version>
        <src-dir>SOURCE_DIR</src-dir>
        <trans-dir>TRANS_DIR</trans-dir>
</config>
```
[This](http://zanata-client.readthedocs.org/en/latest/configuration/#project-version-configuration) may help you here.
*Done! Let's move on...*

> Get ZPC Running

* Download and Install
```sh
$ wget https://github.com/zanata/zanata-python-client/archive/v1.4.0.zip
$ unzip v1.4.0.zip
$ cd zanata-python-client-1.4.0
$ make install
```
* Now, we should have ZPC installed. Help will guide us: how we can communicate. `list`, `project {create, info}`, `version {create, info}`, `push`, `pull` and `glossary push` are the commands we have. You may find [this](https://github.com/zanata/zanata-python-client/wiki#use-cases) interesting.
```sh
$ zanata help
$ zanata <command> --help
```
*Done! Let's move on...*

> Push - Translate - Pull

* Create project and version, can be done on server as well.
```sh
$ zanata project create {project_id} --project-name={project_name} --project-desc={project_description}
$ zanata version create {version_id} --project-id={project_id}
```
* Push language resources to server: please check for user `zanata.ini` and project `zanata.xml` details. *Note*: Locale aliases need to be created and maintained at server and hence keeping locale_mapping in `zanata.xml` is deprecated. Mention project type {`podir` or `gettext`} in command line or in `zanata.xml`, and indicate the same in project settings at server.
```sh
$ zanata push --help
$ zanata push [OPTIONS] 
```
* This should push all templates, translations (if flagged) onto server. Cool! translation activities for the enabled languages can now, be started. Get translations back to compile lang-packs.
```sh
$ zanata pull --help
$ zanata pull [OPTIONS]
```
* Aha, by now we should have translations done.

## Contributors
Hi! Zanata Python Client is purely written in Python. Pull Requests are always welcome. Travis checks for flake8 and tests. Just two steps {`code-review`, `QA`} to get it merged.

> Python-Zanata-Client Code Structure v1.4.0

* Configuration (Remote, Local: zanata.ini, zanata.xml, command-line)
    * `context.py`, `parseconfig.py`
* Commands - Interface and their implementation
    * `zanata.py`, `command.py`, `zanatacmd.py`, `cmdbase.py`, `pushcmd.py`, `pullcmd.py`
* REST APIs - Commands Logic to REST Resource mapping
    * `zanatalib/rest`, `resource.py`, `service.py`, `{doc,...}service.py`
* Others: utilities, converters, parsers, logger, error
    * `projectutils.py`, `csvconverter.py`, `parseconfig.py`, `logger.py`, `error.py`

This helps us where to look into the code - required for adding feature and/or bug fix.

Happy Coding !!
