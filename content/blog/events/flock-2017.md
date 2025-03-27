---
title: flock 2017
type: blog
date: 2017-09-01 19:37:10 +0530
prev: blog/events/
---

"Reporting time was 7:30 pm, and it's 7:50, still you are saying it will take another 15 mins" this was my words while I was waiting and talking with cab driver. I was travelling to US for the very first time, and was little excited. We covered the distance between Pune and Mumbai Airport in decent time and managed to reach there by 12:30 midnight. Our zone was E in the plane and it was Emirates. We reached Dubai early morning. I tried to capture [Burj Khalifa](https://en.wikipedia.org/wiki/Burj_Khalifa).

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2170.JPG?raw=true" width="450"/>
</center>

And then was starting of a long journey - Dubai to Boston. It was 5-6 movies long. Had great food and drinks. And a decent 3 hrs nap. All thanks to Emirates crew and services! We landed Boston 15 past 14 hrs, in the afternoon and waited for 25-30 mins at Bus stop to Hyannis, Cape Cod. Sun was about to sign-off for the day and street lights were yet to glow when we had great MacD burger, and headed towards **The resort and conference center** - flock'17 venue!


<!--more-->

### 29th Aug '17

*Brian Exelbierd* welcome and greet all of us. *Matthew Miller* took us on a tour, how fedora did in last few releases. Its state, community engagements and future plans. Everyone was enthusiastic about the sessions to come and the registration desk was really busy!

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2196.JPG?raw=true" width="450"/>
</center>

Just after coffee break, speakers queued up and briefly advertised their sessions. And after lunch, we joined *Adam Williamson* on his talk `Packagers: working with automated test systems`.

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2205.JPG?raw=true" width="450"/>
</center>

The talk was around Python version checks, framework for automated tests execution: Taskotron, rpmdeplint which is a tool to find errors in RPM packages in the context of their dependency graph. Though rpmdeplint doesnot check 'reverse' dependencies. Rpmgrill - utility to catch koji problems. And OpenQA tests. OpenQA are designed for operating-system level automated testing. Adam's talk was quite informative about different test frameworks actively being used in fedora. After this talk, we rushed to join *Josh Berkus* on his talk about `Become a container maintainer`.

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2206.JPG?raw=true" width="450"/>
</center>

He started his session with [container guidelines](https://fedoraproject.org/wiki/Container:Guidelines). Fixing Dockerfile for registry, labels and entry point. And briefly explained container [review process](https://fedoraproject.org/wiki/Container:Review_Process). This was a do session, and we had to write Dockerfile and review others. Many developers write Dockerfile for various apps but they hardly bother to submit it to fedora registry, this session filled this gap.

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2207.JPG?raw=true" width="450"/>
</center>

In the evening we had game night and drinks. Many participated in swapping candies as well.

### 30th Aug '17

A day of containers and atomic host talks. This track opened with *Dan Walsh*'s talk on `New Container Technologies`. He covered System containers, Skopeo, Buildah, CRI-O, storage and images. A real deep-dive!

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2216.JPG?raw=true" width="450"/>
</center>

Highlights of the talk include linux, containers should be open, openshift, standard container image format, push and pull, moving images between registries, execute container image: runc runtime, PGP signing of images, system containers, standalone containers (service specific), improved storage: read-only and read-write containers, in-prod containers should be immutable, shared file systems: run same images on multiple servers, instantaneous updates, building OCI image, buildah, APIs to manage containers, [CRI-O](https://github.com/kubernetes-incubator/cri-o) effort: support multiple runtime and KPOD - mgmt tool for CRI-O. After this, we continued with `System Containers` and `Discussing Kubernetes & Origin Deployment Options` talks. During lunch we stepped to parking area and made a few clicks!

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2244.JPG?raw=true" width="450"/>
</center>

And then was the Atomic track. *Dusty Mabe* started the track with his talk `Atomic Host 101` and covered topics like Atomic Upgrades/Rollbacks, Browsing OS History, Package Layering, Live filesystem updates, Configuring Storage for Containers and Viewing Changes to your deployed files. This was also a [do session](https://dustymabe.com/2017/08/29/atomic-host-101-lab-part-0-preparation/). After this we had `Setup your own Atomic Workstation` and `Automate Building Custom Atomic Host with Ansible` talks. The evening ended in social gathering, playing games and tasty dinner all organised by flock.

### 31st Aug '17

This is the day I was waiting for! [Globalization](https://fedoraproject.org/wiki/G11N) track and my talk. Before lunch I was part of `Fedora Infrastructure: To infinity and Beyond` and `How do we restore Fedora to factory settings?` sessions. G11N track started with *Jens Petersen* and *Parag Nemade* talk about `Fedora i18n`. Where they covered some interesting topics like Langpacks auto-installation, libpinyin-2, Emoji, ibus, Glibc locales, Flatpak i18n, and others. Jens shared some ideas around Dynamic locale switching, Separation of translations (workflow and packaging) and Multilingual configuration (fallback, fonts).

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2275.JPG?raw=true" width="450"/>
</center>

And here come [my talk](https://www.youtube.com/watch?v=Ln9e3LM2Ht8) on `Introduction to Transtats`. Tool to track translation progress of packages across releases. You can find slides [here](https://speakerdeck.com/sundeep/introduction-to-transtats). This was followed by l10n talk by *Pravin Satpute*, *Alex Eng* and *Jean-Baptiste*. Alex demo'd Jenkins-Zanata plugin. Jean makes us feel what processes are required to make fedora localization community a better place. Thank you Jean!

<center>
    <img alt="" src="https://github.com/sundeep-co-in/sundeep-co-in.github.io/blob/source/source/images/flock2017/IMG_2281.JPG?raw=true" width="450"/>
</center>

We had a long discussion and managed to figure-out action items towards the same. The team went for dinner together and I must say we had a great evening. And this called for conclusion of talks and do-sessions at flock 2017.

An incredible conference: `flock @hyannis` :-)
