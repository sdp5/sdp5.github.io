---
title: Choosing a major keyboard layout got easier in anaconda
type: blog
date: 2020-09-25 13:06:02 +0530
prev: blog/desktop/
sidebar:
  open: true
---

[Anaconda](https://fedoraproject.org/wiki/Anaconda) is the default installer choice of linux distros like [fedora](https://getfedora.org/), [Red Hat Enterprise Linux](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) and others. It allows the user to set languages, keyboards, time & date, storage, networking, software packages and accounts. During installation, the target computer’s hardware is identified, configured and appropriate file systems are created.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/anaconda-keyboards/anaconda.png?raw=true" width="450"/>
</center>

Users can select keyboard layouts for their language (and script) using the Keyboard tab. They can choose from a menu of options. Locating desired layout in this long list could seem tedious. Moreover, it can be hard to search for the US layout when using the wrong default layout.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/anaconda-keyboards/anaconda-keyboard-layouts.png?raw=true" width="450"/>
</center>

An attempt was made to determine major keyboard layouts and place them first in the keyboard layouts selection list. Hence lesser scroll for the users.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/anaconda-keyboards/anaconda-keyboard-layouts-separator.png?raw=true" width="450"/>
</center>

[anaconda-34.5-1](https://github.com/rhinstaller/anaconda/commit/429b4c1eec2b8d00654c7141bfb3fd580729f76e) gracefully includes this proposal, making a significant improvement when selecting keyboards.

happy hacking!
