---
title: Zanata-Python-Client v1.5.0
type: blog
date: 2016-04-20 10:25:08 +0530
prev: blog/i18n_l10n/
---

This release makes z-p-c little more stable with some major bugfixes. Feature wise we have `translation files mapping rules` added.

> Translation Files Mapping Rules

Customize the way translation files are found when pushing, as well as the location they will be saved to when pulling.

```
<!-- example rules definition in zanata.xml -->
<rules>
  <rule pattern="**/pot/*.pot">{locale}/{path}/{filename}.po</rule>
  <rule pattern="**/po/*.pot">{path}/{locale_with_underscore}.po</rule>
</rules>
```

<!--more-->


In the example above, `pattern` identifies a source file, and the contents of the rule element specify how translation files will be stored.

The `pattern` attribute is a **glob** matching pattern to your source document file(s). You can define more than one rule and apply each rule to a specific set of source documents using different patterns. The first matched rule will be applied to the file.


Please note pattern value will be tested against file path relative to project root, not `src-dir`.

`pattern` is optional. If not specified, the rule will be applied to all source documents in your project. The actual rule consists of literal path and placeholders/variables.

Supported placeholders/variables are:

* `{path}` is the path between source document root (what you define as src-dir option) and the final file.
* `{filename}` the source document name without leading path and extension.
* `{locale}` the locale for the translation file. If you use "map-from" argument in your locale mapping, this will be the map-from value.
* `{locale_with_underscore}` same as above except all hyphens '-' will be replaced with underscores '_'. This is typically used in properties and gettext projects.
* `{extension}` the source document file extension

For example, if you have the following file structure (where `{projectRoot}` is the root directory of your project and contains zanata.xml):

```
{projectRoot}/
              templates/messages/kdeedu/kalzium.pot
              templates/messages/kdeedu/artikulate.pot
              de-DE/messages/kdeedu/kalzium.po
              de-DE/messages/keeedu/artikulate.po
              ...
              zanata.xml
```

Here we have two source documents (with `pot` extension) and two translation documents (with `po` extension) for the locale `de-DE`.

You can then use below configuration:

```
<src-dir>templates</src-dir>
<trans-dir>.</trans-dir>
<rules>
    <rule pattern="**/*.pot">{locale}/{path}/{filename}.po</rule>
</rules>
```

Explanation: Since you have defined `<src-dir>` as templates, the source document `templates/messages/kdeedu/kalzium.pot` will have its path extracted relative to `{projectRoot}/templates`, which gives the relative path `messages/kdeedu/kalzium.pot`. The relative path then will be partitioned into several tokens to form the following variables:

```
{path}                          = 'messages/kdeedu/'
{filename}                      = 'kalzium'
{locale}                        = 'de-DE'
{locale_with_underscore}        = 'de_DE'
{extension}                     = 'pot'
```

NOTE the relative path `messages/kdeedu/kalzium.pot` will be the document's unique identifier inside Zanata. If you change src-dir setting later, e.g. to "**.**", which results in a change of the relative path to `templates/messages/kdeedu/kalzium.pot`, pushing again will create a new document with the new path as its unique identifier, and the old document will be considered obsolete and will not be visible to anyone. The old document's translations will not be copied to the new document automatically, but they will appear as **Translation Memory** matches. This can be confusing and frustrating for translators.

As the rule is defined as `{locale}/{path}/{filename}.po`, for locale `de-DE`,

source file kalzium.pot's translation file will be written to or read from `{projectRoot}/de-DE/messages/kdeedu/kalzium.po`.
source file artikulate.pot's translation file will be written to or read from `{projectRoot}/de-DE/messages/kdeedu/artikulate.po`

You can also replace `{locale}` with `{locale_with_underscore}` if you want all your locales to use underscore instead of hyphen. e.g. **de-DE** will become **de_DE** which results in translation files written to or read from `{projectRoot}/de_DE/messages/kdeedu/kalzium.po`

The mapping rules configuration is optional in `zanata.xml`. If not specified, standard rules are applied according to your project type.

* gettext: `{path}/{locale_with_underscore}.po`
* podir: `{locale}/{path}/{filename}.po`
* properties: `{path}/{filename}_{locale_with_underscore}.{extension}`
* utf8properties: `{path}/{filename}_{locale_with_underscore}.{extension}`
* xliff: `{path}/{filename}_{locale_with_underscore}.{extension}`
* xml: `{path}/{filename}_{locale_with_underscore}.{extension}`
* file: `{locale}/{path}/{filename}.{extension}`

Feel free to give it a try and file bugs if you feel any.

Happy Translation!
