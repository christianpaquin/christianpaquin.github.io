---
layout: post
title:  "C2PA Browser Extension Validator"
permalink: /:year-:month-:day-:title.html
---

In this era of disinformation exacerbated by ever-evolving AI tools, the creation of seemingly authentic fake content can be quite dangerous, with risks ranging from harming one’s reputation to damaging society as a whole.

Fortunately, provenance technologies are emerging to fight this problem. The [Coalition for Content Provenance and Authenticity](https://c2pa.org/) (C2PA) is the leading effort allowing creators to cryptographically sign their digital assets and editors to record subsequent transformations, helping consumers to confirm their origin and authenticity while keeping an auditable history of the data. It has been adopted by leading technology providers (Microsoft, Google, Meta, Intel), camera manufacturers (Sony, Canon, Nikon), image/video editors (Adobe), generative AI systems (Copilot, OpenAI, Midjourney), and news organizations (BBC, CBC/Radio-Canada, New York Times). The C2PA is also at the forefront of the fight against election disinformation and was one of two technologies mentioned in the recent [AI Elections accord](https://www.aielectionsaccord.com/) signed at the Munich security conference.

I'm happy to announce the release of a new open-source [browser extension validation tool](https://github.com/microsoft/c2pa-extension-validator) to verify C2PA assets on a web page. The extension, currently a developer preview prototype, scans a web page for C2PA images and videos, validates them, and overlays a content credential logo to display their status. Our goals are to 1) allow people to experiment with C2PA technologies, and 2) be able to rapidly prototype new C2PA features (and ones from its sister [Creative Assertions Working Group](https://creator-assertions.github.io/)). This is very much work-in-progress and we'd love feedback and suggestions from the community, so please visit the GitHub project and try it out!

<div align='center'>
  <figure>
    <img src="img/c2pa_bing_creator_image.png" alt="C2PA-signed Bing Creator image" width="80%"><br/>
    <figcaption>A validated C2PA-signed image from the <a href="https://www.bing.com/create">Bing Creator</a>.</figcaption>
  </figure>
</div>
<br>

## Project Origin Verified Publisher Trust List

[Project Origin](https://www.originproject.info/), one of the two initiatives that unified to become the C2PA, [recently announced](https://www.iptc.org/news/iptc-c2pa-verified-news-publishers) the creation of a trust list of verified publishers to help fight disinformation in the digital news ecosystem.

Our browser extension can use the [Verified Publisher Trust List](https://www.iptc.org/origin-trust-list/) hosted by the IPTC to verify content from trusted signers. You can [watch a demonstration](https://www.youtube.com/watch?v=pFw9QHoew6U) of our extension verifying a video from the BBC who recently [started to use](https://www.bbc.com/rd/blog/2024-03-c2pa-verification-news-journalism-credentials) the provenance technology.

<div align='center'>
  <figure>
    <img src="img/origin-demo.jpeg" alt="C2PA validator using Origin Trust List demo" width="80%"><br/>
    <figcaption><a href="https://www.youtube.com/watch?v=pFw9QHoew6U">Demo</a> of our browser extension validator using the <a href="https://www.iptc.org/origin-trust-list/">Origin Verified Publisher Trust List</a> to verify a video from the BBC.</figcaption>
  </figure>
</div>
<br>

## Road ahead

It's exciting to see the growing momentum behind the C2PA and all the organizations it brought together. I believe that down the line, C2PA-protected assets will be like HTTPS-protected websites: content without certified provenance information will look very suspicious! The road to get us there will however be challenging, but I hope that these initial steps will allow us to explore and prototype the capabilities that will make this technology ubiquitous.

Onward!