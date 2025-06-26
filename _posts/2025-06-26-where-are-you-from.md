---
layout: post
title:  "Where are you from?"
permalink: /:year-:month-:day-:title.html
---

"Where are you from?" is a common introductory question we ask when meeting someone. Unfortunately, we can't ask the same of a picture, video, or audio clip found online. In this era of generative AI, it has become practically impossible to distinguish digital assets captured from "real life" from those artificially created by generative systems. In the [latest *Last Week Tonight* episode](https://www.youtube.com/watch?v=TWpg1RmzAbc), John Oliver highlights the rise of "AI slop." You know an issue has gone mainstream when it makes it onto his show.

Of course, "AI or not" isn't *the* most important question. We're long past the days when media and music weren't modified by computer-assisted tools, many of which now incorporate AI by default. The more meaningful questions are: "Where is this asset from?", "How was it generated?", "By whom?", and "How was it transformed?"

Fortunately, technologies exist to help answer these questions. Content Credentials, developed by the [Coalition for Content Provenance and Authenticity (C2PA)](https://c2pa.org/), can be attached to digital assets to describe how and by whom they were created, much like a clothing label tells us about a garment's origin. For example, when a photographer takes a picture with a C2PA-compliant camera, cryptographically signed provenance data (including optional metadata like a secure timestamp or geolocation) can be embedded in the image.

<div align='center'><img src="img/c2pa_photographer.png" alt="AI generated photographer" width="75%"><br/></div><br>

Incidentally, the image above wasn't taken by a camera, it was generated using [Copilot](https://copilot.microsoft.com/). We know this because Copilot, like many generative AI systems, attaches Content Credentials to everything it creates. By inspecting the image in a [validation portal](https://contentcredentials.org/verify) or using our [browser extension](https://christianpaquin.github.io/2024-04-17-c2pa-browser-extension-validator.html), the provenance is revealed.

We're at the beginning of a major deployment phase for C2PA. Today, AI-generated content floods our digital spaces. But as C2PA adoption grows, platforms will be better equipped to determine the origin of the content they serve, allowing them to surface trusted content more prominently.

### Recent highlights

I joined the C2PA Technical Working Group nearly two years ago, and we've been working hard to bring this technology forward. The past few months have been especially active:

* We released [version 2.2](https://c2pa.org/specifications/specifications/2.2/specs/C2PA_Specification.html) of the core specification last month, incorporating feedback from implementers.
* The [Conformance Program v0.1](https://github.com/c2pa-org/conformance-public) launched two weeks ago, enabling the creation of a trusted ecosystem of hardware and software implementations.
* The IPTC continues to expand its [Verified News Publisher list](https://iptc.org/verified-news-publishers-list/), increasing trust in the news media ecosystem, just one of many verticals to come.
* Earlier this month, many of us gathered at the 3rd [Content Authenticity Summit](https://contentauthenticity.org/blog/content-authenticity-summit-2025), where numerous updates were shared and implementations demonstrated. You can watch the keynote presentations [here](https://www.youtube.com/watch?v=9rCj8y-TseE).

This momentum is encouraging, and I'm excited to see what the second half of the year brings for content provenance.

### A layered approach

Of course, C2PA is not a silver bullet. Just like a clothing label, Content Credentials can be removed, omitted (for non-compliant creators), or even faked. For instance:
* Credentials might be stripped from an asset, either deliberately or due to format incompatibilities.
* Malicious systems generating deceptive content may omit credentials altogether.
* Attackers might attempt to forge Content Credentials. While they can't successfully impersonate another party cryptographically, their fakes could still mislead users, similar to how cybersquatting (or typosquatting) domains can trick visitors. (The C2PA UX Working Group is actively developing guidance to address these issues.)

Other provenance tools, such as digital watermarks and fingerprints, can complement C2PA. They can be used to recover missing manifests or determine an asset's origin independently. (The C2PA Watermarking Working Group is focused on defining these capabilities.)

And in cases where provenance metadata isn't embedded at all, AI detectors can serve as a last line of defense to flag potentially AI-generated content. Hany Farid gave a compelling [keynote](https://www.youtube.com/watch?v=9rCj8y-TseE&t=7794s) on this topic at the CA Summit.

### Tag everything?

Longtime readers of this blog know that privacy is a major concern of mine. Adding digital signatures to all images and videos has significant privacy implications, not only for individuals, but also for organizations like newsrooms seeking to protect the identity of their journalists or sources. The [C2PA Threat Model](https://c2pa.org/specifications/specifications/2.0/security/Security_Considerations.html) addresses many of these concerns, but today's specification still has limits.

Fortunately, privacy-preserving cryptography, like zero-knowledge proofs (as we are developing in [Crescent](https://christianpaquin.github.io/2024-12-19-crescent-creds.html)), can help meet stringent threat models, even enabling *certified anonymity*. Be assured that this is on our research radar...

