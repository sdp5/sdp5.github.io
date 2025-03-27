---
title: Making your TV smart with rpi
type: blog
date: 2020-08-17 12:16:12 +0530
prev: blog/desktop/
sidebar:
  open: true
---

[Raspberry Pi](https://www.raspberrypi.org/) has gained popularity in a very less time for obvious reasons. Let's take a look how we can turn our TVs smart with the tiny yet powerful board. Quickly check you have following:

* Raspberry Pi board
* Power cable (Micro USB/USB Type-C)
* SD Card (min 8 GB)
* Ethernet cable
* Keyboard, Mouse
* HDMI Cable, TV or Monitor

Get your laptop. Hit the [downloads](https://www.raspberrypi.org/downloads/) page! Make `Raspberry Pi Imager` work on your system. Basically we will be exploring [kodi](https://kodi.tv/), however, lets use [OSMC](https://osmc.tv/) which wraps it inside! Download OSMC image respective to the board you're using from [Disk Images](https://osmc.tv/download/). Now, burn that onto your SD Card using the Raspberry Pi Imager. Plug SD card and other peripherals to your board as well as turn the power on. Here is your OS preparing ...

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/kodi/osmc-boot.jpg?raw=true" width="450"/>
</center>

OSMC (*open source media center*) is debian based OS, hence all `apt` here!<br/>With small settings check/enable `ssh` and try [connecting](https://osmc.tv/wiki/general/accessing-the-command-line/). Look at sources in `/etc/apt/sources.list.d/`. The UI must have astonished you by now.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/kodi/osmc-menu.jpeg?raw=true" width="450"/>
</center>

I'm sure, you cannot wait to configure `YouTube` and/or `Amazon Prime` in your setup. Let's roll. `YouTube` video add-on is available in *Kodi Add-on repository* so, its just adding/enabling that from navigating to `Home > Add-ons > Install from repository > Video Add-ons`.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/kodi/youtube-install.png?raw=true" width="450"/>
</center>

Though you've enabled YouTube; you may encounter an error `accessNotConfigured`. However, the fix is simple. Navigate to [Google's Developers Console](https://console.developers.google.com/) and register a new youtube project for generating `API Key`, `API ID` and `API Secret`. Fill them. Now, it should be working.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/kodi/youtube-api.png?raw=true" width="450"/>
</center>

For Netflix and Amazon Prime, get onto your `ssh console` and follow this amazing [blog post](https://makingstuffwork.net/technology/install-netflix-amazon-prime-video-plex-raspberry-pi-kodi-osmc/). Source repo is [here](https://github.com/Sandmann79/xbmc). ARMv6 processors may not support these formats because of [libwidevine](https://github.com/emilsvennesson/script.module.inputstreamhelper/issues/23). Enjoy your add-ons a bit. It has a plenty of options across `programs`, `music`, `pictures` and `vidoes`. Enable a few more to test the performance!

Next is to get rid of keyboard/mouse. Look none other than [Yatse](https://yatse.tv/). That's awesome. Just [play](https://play.google.com/store/apps/details?id=org.leetzone.android.yatsewidgetfree&hl=en_IN) it on your mobile device. Remember to keep both your mobile and Raspberry Pi on the same network. `Yatse` will discover your kodi automatically to let you interact seamlessly.

<center>
    <img alt="" src="https://github.com/sdp5/sdp5.github.io/blob/source/source/images/kodi/yaste.png?raw=true" width="450"/>
</center>

[Kodi](https://github.com/xbmc) is a great community with awesome people.
