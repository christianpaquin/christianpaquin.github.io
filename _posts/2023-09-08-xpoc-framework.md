---
layout: post
title:  "Introducing the Cross-Platform Origin of Content (XPOC) framework"
permalink: /:year-:month-:day-:title.html
---

It is quite challenging today to verify the origin of online content. Content creators (individuals or organizations) with well-known websites typically provide a way to discover their various associated accounts on (social) media platforms (e.g., YouTube, X/Twitter, Facebook, Instagram, etc.), most commonly using the well-known logo icons.

<div align='center'>
  <img src="img/social_logos.png" alt="social account logos" width="20%"><br/>
</div>
<br>

Manual validation of such information can be complicated and assumes that you know the creator's website. Moreover, a lot of content can also be created as a collaboration of multiple collaborators (e.g., a podcast, a video interview, a conference panel, etc.) and the resulting media could be posted under the account of one of the creators or a host.

In this era of disinformation, made worse by ever-evolving AI tools, the creation of seemingly-authentic fake accounts and content can be quite dangerous, with risks ranging from damaging one's reputation to having society-wide impact. AI detection tools are playing (and losing!) a cat and mouse game against rapid technological developments. Many media hosting platforms have account and content validation processes with various levels of quality, but these systems are disconnected and can't rely on a standardized verification mechanism that could be shared among them.

To address this issue, we've created the [Cross-Platform Origin of Content (XPOC) framework](https://github.com/microsoft/xpoc-framework), allowing content creators 1) to publish an authoritative manifest of their accounts and approved content across various platforms, and 2) to tag said accounts and contents with special XPOC URI linking back to their manifest. This allows validation tools to automatically determine the origin of the protected content.

<div align='center'>
  <img src="img/xpoc_manifest.PNG" alt="XPOC manifest example" width="80%"><br/>
  <figcaption>XPOC manifest for christianpaquin.github.io</figcaption>
</div>
<br>


This framework would be useful to politicians, celebrities, companies, government entities and many other stakeholders to provide a signal of authenticity for the accounts they control and the content they created or approved.

The XPOC framework was designed to be simple to implement and deploy, allowing content creators to publish manifests and tag protected content without the explicit collaboration of the media platforms; in turn, platforms implementing the framework would improve their user's experience and content origin validation.

The [GitHub project](https://github.com/microsoft/xpoc-framework) contains the framework specification, a TypeScript reference library, and some sample implementations for XPOC manifest editing tools and XPOC URI validation tools.

## System Overview

I'll illustrate the XPOC framework using myself as an example. My main website is [christianpaquin.github.io](https://christianpaquin.github.io); it hosts this very blog. This is the central point of my professional persona, where you can find links to some of the accounts I control on other platforms (e.g., my X/Twitter handle and my GitHub account). I created a XPOC manifest to list all of them in a standardized and discoverable manner. Moreover, I added links to content I created or participated in that has been posted by accounts I don't control (e.g., this [panel on societal resilience](https://www.youtube.com/watch?v=pzi76VdmD9g) posted on the MSR YouTube channel or this [post-quantum crypto presentation](https://www.youtube.com/watch?v=IqCw19bKE6c) I gave a DEF CON 27). You can take a look at the [xpoc-manifest.json](https://christianpaquin.github.io/xpoc-manifest.json) file hosted on this web site.

<div align='center'>
  <img src="img/christianpaquin.github.io_xpoc-manifest.jpg" alt="XPOC manifest for christianpaquin.github.io" width="80%"><br/>
  <figcaption>XPOC manifest for christianpaquin.github.io</figcaption>
</div>
<br>

It is important to note that this manifest only contains accounts and content I associate with my MSR persona; I have other personal accounts and content I didn't list here (but could be present in a personal webpage's manifest).

If someone or some system would like to find all the accounts and content associated with me, they could retrieve and inspect this file. Humans would prefer more user-friendly discovery tools, such as the sample XPOC [viewer portal](https://github.com/microsoft/xpoc-framework/tree/main/samples/client-side-html) from our GitHub repository.

<div align='center'>
  <img src="img/xpoc-viewer.jpg" alt="XPOC viewer" width="80%"><br/>
  <figcaption>Sample XPOC manifest viewer</figcaption>
</div>
<br>

An important feature of the framework is the ability to link these accounts and content back to my main website, to let visitors know who is behind my social handles and account names. I did this by adding the `xpoc://christianpaquin.github.io!` XPOC URI to my account pages and content. The `xpoc://` prefix and terminating character `!` help parsing tools to find the URI in the page's HTML; they would then extract the base URL `christianpaquin.github.io` and fetch the corresponding manifest at `https://christianpaquin.github.io/xpoc-manifest.json`. 

For example, I've put my XPOC URI in my [X/Twitter bio](https://twitter.com/chpaquin), and in this (test) [YouTube video description](https://www.youtube.com/watch?v=hDd3t7y1asU), allowing validation tools to find my manifest.

Our GitHub repository contains a sample [browser extension](https://github.com/microsoft/xpoc-framework/tree/main/samples/browser-extension) that can be used to verify these URIs, allowing people to verify that these accounts and content are actual from me and not someone pretending to be.

<div align='center'>
  <img src="img/xpoc-browser-extension-twitter.jpg" alt="Validation of X/Twitter XPOC URI" width="80%"><br/>
  <figcaption>Validation of X/Twitter XPOC URI</figcaption>
</div>
<br>

<div align='center'>
  <img src="img/xpoc-browser-extension-youtube.jpg" alt="Validation of YouTube XPOC URI" width="80%"><br/>
  <figcaption>Validation of X/Twitter XPOC URI</figcaption>
</div>
<br>

# The road ahead

Fighting disinformation and fake content is a challenging task, and we are only at the beginning of the battle.

The XPOC framework won't magically solve the issue, but it can help address a slice of the problem. If content creators (especially those targeted by fake content) start publishing manifests of their content, then verifiers (fact checkers, journalists, hosting platforms) would be better equipped to confirm their origin, and this would reduce the window of damage opportunity of the malicious content.

Early XPOC adopters falling victim to some attacks could help educate verifiers, letting them know that they should have detected the fake since they weren't listed in their manifest. These early incidents would help turn the adoption wheel.

As adoption grows, it will become more difficult to target creators who are using the XPOC framework, and attackers would likely focus on those who don't. Similarly to what happened during the HTTP-to-HTTPS transition, early adopters benefited from stronger security, and those late to the party had their unprotected site flagged as insecure or even blocked. I can imagine the same situation happening if creators start using XPOC: the last minority of unprotected accounts would look suspicious, and might go through stronger account and content review processes on the hosting platform.

Let us know what you think about the XPOC framework, how it could be used and improved.

Onward!