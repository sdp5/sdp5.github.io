---
title: Langpacks Auto-installation
type: blog
date: 2019-07-31 17:00:21 +0530
prev: blog/desktop/
sidebar:
  open: true
---

Fedora Linux Operating System has strong language support. Underneath, there are a lot of components that facilitate `display`, `input` and `storage` of language specific data. Users can install Fedora in their native language and continue using it, or they can easily switch their workstation from one language to another. For the latter case, the steps are:

* Open “GNOME Control Center”
* Select “Region & Language” tab.
* Select language from modal box and logoff-login the session.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io-jekyll/blob/source/source/images/autoinstall-langpacks/switch-lang.png?raw=true" width="450"/>
</center>

Here, the user has selected Japanese language. As soon as the user logs into the new session, Japanese `locale data` would be triggered and applications would appear in Japanese.  Language data available in langpacks (*language packs*) include fonts, input methods, translations etc.

To make installation of langpacks convenient, `langpack meta packages` have been created. Once a meta package is installed, the required langpacks for installed applications are downloaded and installed too. For Japanese, the installation command would be:

```
[localhost] $ sudo dnf install langpacks-ja
```

Earlier, this installation was a manual step. After long discussion, `GNOME-Software` was identified to be an ideal setup to have langpacks auto-installation functionality.

Now, with the [implementation](https://gitlab.gnome.org/GNOME/gnome-software/merge_requests/257), users need not worry about installation of langpacks as it gets installed automatically in the background as soon as GNOME-Software starts looking for updates the first time a user logs into the desktop in that language.

Because this is a `background task`, no notifications are popped, however here is how a debug message is logged.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io-jekyll/blob/source/source/images/autoinstall-langpacks/langpacks-ja.png?raw=true" width="450"/>
</center>

If the user uninstalls the language meta package, this feature will not automatically reinstall it. Users can still reinstall the meta package manually if they wish to use it again.

I really hope this feature to be available in Fedora 31. And make the lives of Fedora native language users easier. :)
