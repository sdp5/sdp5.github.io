---
title: Transtats - Getting Started
type: blog
date: 2019-02-06 11:49:23 +0530
prev: blog/i18n_l10n/
---

Helps in tracking translation completeness of your packages. These packages may participate in product releases, so a few Transtats UI group statistics to create at-a-glance translation progress picture - *for the release*! It has tools to detect translation deficiency in ready-to-ship packages.

## Getting Started
Transtats Fedora instance is running at https://transtats.fedoraproject.org and `fedora-30` release is added.

<!--more-->

##### First step is to add your package in Transtats.
* Click on `Fedora Login` button and login into the system using your Fedora Credentials.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/1-fedora-login.png?raw=true?raw=true" width="350"/>
</center>

* After logged-in, click on `Packages` link. (*This will take you to packages list page*)

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/2-packages-link.png?raw=true?raw=true" width="350"/>
</center>

* Click on `Add Package` button to add your package in Transtats.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/3-add-package.png?raw=true?raw=true" width="350"/>
</center>

* Enter `Package Name` and `Upstream URL`. Select `Translation Platform` and check `Fedora`.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/4-package-form.png?raw=true?raw=true" width="500"/>
</center>

* Finally, click on `Add Package` button to validate and register the package with Transtats.
* Go back to list packages page and locate newly added package. Click on package name.
    * This will take you to the package details page. (*Use dropdown to navigate between packages*)

##### Secondly, sync your package with Translation Platform, Build System and Upstream.
* Sync the package with `Translation Platform`; this will generate `Branch Mapping` also.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/5-sync-platform.png?raw=true?raw=true" width="400"/>
</center>

* Once branch mapping is created, sync the package with `Koji` build system.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/6-sync-buildsys.png?raw=true?raw=true" width="400"/>
</center>

* This will take you to the syncdownstream YML job; uncheck *Dry Run* and run the job!

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/7-job-run.png?raw=true?raw=true" width="500"/>
</center>

* Go back to package's detail page and locate statistics from `koji` build system.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/8-locate-stats.png?raw=true?raw=true" width="350"/>
</center>

* Click on `Statistics Diff` to update diff and know - which languages need attention.
    * This is based on branch mapping generated of the respective package.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/9-stats-diff.png?raw=true?raw=true" width="400"/>
</center>

* This will indicate languages in which the package is out of sync.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/10-out-of-sync.png?raw=true?raw=true" width="350"/>
</center>

##### Finally, update releases and packages summary.
* Go to `Dashboard` (*landing page*) and click to refresh summary.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/11-release-summary.png?raw=true?raw=true" width="500"/>
</center>

* Similary for packages - click on `Packages - Translation Completeness` tab and refresh summary.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/ts-get-started/12-package-summary.png?raw=true?raw=true" width="500"/>
</center>

## Limitations
As of now following are the limitations (*shall be eliminated in coming Transtats releases*):

* Packages could not auto sync (with Platform, Build System etc.) and need manual effort.
* Branch mapping and Statistics Diff need to be updated manually for each package.
* Releases and Packages summary also need manual effort to get updated.

#### Sync Order
1. Sync package with Translation Platform, Build System and Upstream.
2. Update Statistics Diff and then, Releases & Packages Summaries.

#### Jobs
While running job uncheck `Dry Run` only when the package is added in Transtats - to save stats.
`Sync Package Build System` job can be dry run for any package and any build tag (*including rawhide*).

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/ts-get-started/13-yml-job.png?raw=true?raw=true" width="400"/>
</center>

* Job log URL will be generated for dry runs too, and can be shared.

## More resources
* https://transtats.fedoraproject.org/quick-start
* http://docs.transtats.org/en/latest/

## Issues
* Please report issues / feature ideas here: https://github.com/transtats/transtats/issues/new
* Join `#transtats` channel in IRC freenode. Or, drop queries at `#fedora-g11n` channel.

## References
* https://speakerdeck.com/sundeep/using-transtats-to-track-translation-string-change
* https://www.youtube.com/watch?v=8q9cg-wsrUg

Thanks!
