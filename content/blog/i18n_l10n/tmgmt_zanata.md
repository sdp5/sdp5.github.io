---
title: Working with TMGMT Zanata
type: blog
date: 2016-02-25 11:53:55 +0530
prev: blog/i18n_l10n/
---

TMGMT Zanata is a plugin for Drupal's Translation Management Module TMGMT. The plugin allows TMGMT to send content to a configured Zanata project for translation, and can download translations as they are ready.

Zanata is a web-based system for translators, content creators and developers to manage localization projects.

1. Installation
2. Set Up
3. Configuration
4. Translation Jobs

## Installation

TMGMT Zanata requires the Translation Management Tool module (TMGMT) to be enabled. And to use Translation Management (Node Translation), the following modules must be installed:

* Entity API
* Views
* Chaos Tools (Required for Views)
* Views Bulk Operations
* Rules
* Content Translation (enable only, part of core)
* Locale (enable only, part of core)


TMGMT also requires some other modules to be enabled:

* Translation Management UI
* Content Source User Interface
* Export / Import File

This will automatically trigger the activation of Translation Management Core, Content Source and Translation Management Field modules.

TMGMT Zanata Installation is similar to any Drupal module.

Installed and enabled TMGMT with Zanata Plugin will look something similar to screenshot below:

![Drupal TMGMT Module](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/1_tmgmt.png)

TMGMT Zanata plugin appears as `Zanata translator` and if enabled, can be used as one of the available translators to TMGMT module.

## Set Up

### Enable translation for content types

To make each content type translatable, the publishing options for the content type must be set appropriately. To do so:

1. In the administration menu, navigate to "Structure" > "Content types".
2. Find the content type you wish to make translatable and open its edit page.
3. Change to the "Publishing options" tab
4. Under "Multilingual support, select "Enabled, with translation".
5. Click "Save content type"

![Content Type](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/2_content_type.png)

### Add languages

To allow translation, your site must have one or more languages enabled, in addition to the default language. To enable a language:

1. In the administration menu, navigate to "Configuration" > "Regional and language" > "Languages".
2. Open the "Add language" page
3. Select an existing language or enter details for a custom language.
4. Click "Add language" or "Add custom language"

![Enable Languages](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/3_languages.png)

### Enable the TMGMT Zanata plugin module

Once the code for the plugin module has been installed, enable the module. To do so, open "Modules" in the Administration menu, scroll to the "Translation Management" section, and ensure that "Zanata translator" is checked. Also ensure that TMGMT and its dependencies are checked. Click "Save configuration" to finalize the change.

### Cron

Set up cron to automatically fetch new translations from Zanata server every time it runs.
There are two separate ways to run Drupal's cron tasks:

#### Automated Cron

The automated cron system is compatible with all systems because it doesn't actually involve the system's cron daemon. It works by checking at the end of each Drupal request to see when cron last ran and, if it has been too long, processing the cron tasks as part of that request. The two down-sides are
cron tasks will only run when Drupal is processing requests, and
the 'weight' (processing and memory) of running the cron tasks will be added to some arbitrary unknown page request, which may slow down those requests, which may slow down those requests, and has the potential to exceed memory limits on a complex site.

![Cron](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/4_cron.png)

#### External Cron

Create a cron job, or use some other external (to Drupal) method of triggering its cron tasks, such as an external cron job service like EasyCron or Cronless. This is the more reliable because it will always run on schedule. Note that if you create a cron job, you may want to disable the "automated cron" system entirely.

Easiest way to setup external cron it to add following in system crontab:

``0 * * * * wget -O - -q -t 1 http://{your_drupal_server}/cron.php``

This would run every hour. Replace {your_drupal_server} with the url of your server.


## Configuration

Before the module can be used, it must be configured with some details about Zanata - user and project. These are set on the translator configuration page.

If you do not yet have a Zanata username or API key, or you have not yet created a project or project-version on Zanata for your website, use the following instructions to do so before you continue:

* To get a Zanata account, see Signing Up.
* For instructions on finding your API key, see "User Configuration" on the Configure the Client help page.
    * User settings are accessed through the Dashboard.
    * Note that you do not need to create any config files for this plugin.
* For help creating a project, see Project Creation.
* For help creating a version, see Version Creation.

To open the translator configuration page and enter configuration:

1. Open the Administration menu
2. Navigate to "Configuration" -> "Regional and language" -> "Translation Management Translators"
3. You will see a list of translator plugins that have been added to TMGMT. Look for "Zanata translator" in the list.
4. If you see an auto-created Zanata translator, open the configuration page by clicking "edit".
5. If you do not see an auto-created Zanata translator, create one. To create a Zanata translator, click "Add translator", enter an appropriate label such as "Zanata translator", and in "Translator plugin" select "Zanata translator". When you select "Zanata translator" its configuration page will be displayed below.
6. If you do not wish to perform translation review in Drupal, for example if review will be done in Zanata, ensure that "Auto accept finished translations" is checked. This setting can be changed later.
7. Ensure that the Zanata server URL shows the correct Zanata server on which you have an account and have created a project for your website. To find the full URL, open the Zanata server and click the Zanata logo to open the homepage. The URL shown in your browser address bar is the server URL, but do not include a / at the end. For example, the homepage for the main Zanata server is at "http://translate.zanata.org/zanata/" so the URL for this server is "http://translate.zanata.org/zanata".
8. Enter your username, your API key, your project ID and your project-version ID. Be careful to use your Zanata username rather than your display name, and the Project ID rather than the Project Name. See the links above for directions that may help to find this information.
9. Click "Save translator" to complete the configuration.
10. You are now able to create translation jobs that use Zanata translator.

![Plugin Configuration](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/5_settings.png)

Feature, `Translation Periodic Check` depends upon Cron setup and it fetches new translations from Zanata server every time cron runs.

## Translation Jobs

TMGMT Zanata is a plugin for the Translation Management Tool (TMGMT), so most of the directions for creating and interacting with translation jobs can be taken directly from TMGMT's documentation. Let’s see translation of an article using TMGMT Zanata.

* In your website, navigate to article's page (you want to translate) and click on `Translate`. If `Translate` is not appearing please see configurations and complete the steps.

![Page](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/6_page.png)

* Clicking on `Translate` would take you to this page. Here, you can select language and request translation. Translation job would be created as soon as you request translation.

![Select Language](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/7_select_lang.png)

* Upon request, translation job would be added to cart. Here we can select translator for the added job and submit it. Job items and target language should be verified. As soon as we submit or checkout, page’s content would be pushed to server for translation.

![Submit Job](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/8_submit.png)

* After submission we receive confirmation which looks something like:

![Submit Confirmation](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/9_confirmation.png)

* In Zanata, we can navigate to the document uploaded.
    * As mentioned in Zanata Translator Plugin Settings, select Project and Version.
    * Select the language for which job is created.
    * Here, we can see list of documents uploaded for the given language.
    * Select the document which belongs to submitted job.
        * Please refer submission confirmation notes to identify document name, as it may differ than the article’s heading.
    * This would take you to Zanata translation page. Here, you can translate the article.
      Upon translation of content, mark complete to make them available for pull.

![Zanata](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/10_zanata.png)

* If `Auto accept finished translations` and `Translation Periodic Check` are checked in translators settings, new translations delta are fetched every time cron runs and article will be published automatically once it reach 100% translated. We can fetch new translations and/or re-submit it to translator for active translation jobs.

![Manage Job](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/11_manage.png)

* Once translations are fetched from Zanata, it would be available for review. This step can be skipped by checking `Auto accept finished translations`. Accepted translations look like the screen below. Acceptance of translation makes a translation job finished.

![Fetch Translation](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/12_fetch.png)

* As soon as a translation job finishes, TMGMT publishes it and a navigation is added to the page which takes user to translated version of the article of intended language.

![Publish Translation](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/13_publish.png)

* Summary of translation jobs looks similar to below screen.

![Job Summary](https://raw.githubusercontent.com/sundeep-co-in/sundeep-co-in.github.io/source/source/images/tmgmt_zanata/14_summary.png)

### References

* https://www.drupal.org/project/tmgmt_zanata
* https://www.drupal.org/node/1490024
* https://www.drupal.org/cron
* https://www.drupal.org/node/1547632

Happy Website Translation!!

