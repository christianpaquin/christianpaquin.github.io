---
layout: post
title:  "User-centric Web Attestations"
permalink: /:year-:month-:day-:title.html
---

Over the course of our life, we receive many attestations that we carry and display proudly. I framed my university diploma in my study; professionals (doctors, psychologists, etc.) display theirs in their offices. I also carry many attestations in my wallet (membership cards, employment badge, etc.) that I can easily show to anyone.

Online attestations also exist, but unlike the physical ones, they are mostly tied to one environment, making it difficult to present across system boundaries. For example, some social media sites display badges for verified users, gamers can display achievements in their profiles, NFT (especially the [soul-bound ones](https://www.microsoft.com/en-us/research/publication/decentralized-society-finding-web3s-soul/)) can represent memberships or entitlements.

<blockquote>
What if you could display attestations on the web from whomever, wherever you wanted? What if we could build a portable blue check mark?
</blockquote>

To explore this idea, we released a proof-of-concept [User-centric Web Attestations (UWA)](https://github.com/microsoft/web-attestation-sample/) framework, with which users can obtain certified, cryptographic attestations from various issuers, and attach them to any web property they control (e.g., a personal blog, a social media profile, etc.). The project contains a sample issuer Express server and an Edge/Chrome browser extension that can create and verify web attestations.

# Strong privacy

There is a [strong push](./2023-03-22-auth-all-the-things.html) for verified user information in this era of generative AI, but to avoid further eroding the already fragile online privacy and autonomy of users, we designed the UWA framework to support the strongest privacy threat model.

It is often desirable to attach attestations to your “real life identity”: e.g., attach your academic achievements to your resumé, your employment history to your LinkedIn profile, etc. Other times, you might prefer to link them to a pseudonymous identity not linkable to your real-life one. Using the UWA framework, anyone trusting the issuer can verify the attestations, but no one (including the issuer itself) can link their issuance to their presence on a web page. In other words: I can get attestations that state “I’m a member of Community XYZ,”  “I’m over-18,” or simply “I’m a human,” and that’s the only information one could infer from them without being able to link it back to the actual person to whom it was issued (even if a malicious insider actively tries to track usage of UWA it issues).

We achieve this strong privacy property by encoding attestations using [U-Prove tokens](https://microsoft.com/uprove).

# System overview

The following diagram gives an overview of a web attestation life cycle:

<div align='center'>
  <img src="img/UWA_arch.svg" alt="UWA architecture" title="UWA architecture diagram"><br/>
</div>

1. An Issuer sets up its public parameters and publishes them in a well-known location. These specify the contents of the U-Prove tokens, which can contain an application-specific label to make the attestations more informative. Users and Verifiers must obtain the Issuer parameters before creating or verifying web attestations.
2. The User obtains U-Prove tokens from an Issuer. Authentication to the Issuer is application-specific (e.g., an Issuer might issue attestations to its members, or provide a notary validation service for paying customers). U-Prove tokens are stored in the web browser extension.
3. When visiting a website, the User can create a web attestation from an issued token using the web browser extension (encoded either as a string or a QR code), and attach it to the site. The U-Prove token is then deleted from the browser extension to prevent linkability with newly created attestations (new tokens will be automatically obtained from the Issuer if they expire or if they are running out.)
4. Other users visiting the same website can verify attached web attestations from trusted Issuers using the web browser extension. Unknown Issuers can be added to the trusted list by the User. Invalid attestations (for example: forged, or copied from a different site) are marked as such; malformed ones are simply ignored.

# An example

I created a web attestation to attach to this blog post issued by the project's [sample issuer](https://github.com/microsoft/web-attestation-sample/tree/main/sample-issuer).

```
uwa://eyJhbGciOiJVUDI1NiJ9.eyJzY29wZSI6Imh0dHBzOi8vY2hyaXN0aWFucGFxdWluLmdpdGh1Yi5pby8yMDIzLTA1LTMwLXVzZXItY2VudHJpYy13ZWItYXR0ZXN0YXRpb25zLmh0bWwiLCJ0aW1lc3RhbXAiOjE2ODUxMzQzMzg3Mjl9.eyJ1cHQiOnsiVUlEUCI6IlVXemxCU3VyRkl3ZnVxVy0xaFdYa2VPcGNaQzlvYjNITlRKOUdvM2hUN1EiLCJoIjoiQlBhZVI2VFB2dV9YU2tLMHNoZnR0dVdXV3Z5MVBfQVRkV0txalNGZTJqSVJPM3JQSm1pSnkxZXJWMll6VFNwZEVuLWYtN1BoR0ZSMzNvd29LWmtBelVjIiwiVEkiOiJleUpwYzNNaU9pSm9kSFJ3Y3pvdkwzSmhkeTVuYVhSb2RXSjFjMlZ5WTI5dWRHVnVkQzVqYjIwdmJXbGpjbTl6YjJaMEwzZGxZaTFoZEhSbGMzUmhkR2x2YmkxellXMXdiR1V2YldGcGJpOXpZVzF3YkdVdGFYTnpkV1Z5TDNOaGJYQnNaU0lzSW1WNGNDSTZNakF3TURRc0lteGliQ0k2TVgwIiwiUEkiOiIiLCJzWnAiOiJCTm5IUHg2QllJZ1BrTlE0VVBrV0VndW5veWxIck5XR2hkM2R2TW93bkk0MlZ3QXRlS1FsSzdXMFpBREtSRHhsanV0aElYRy10Z2REcWxWTlFKenpTbHMiLCJzQ3AiOiJMUXhadGNxTVZiYnk1dEZwYVU1WlBabFFqMUhDOWpYSnJjSlNCZzl6bEF3Iiwic1JwIjoiU2t6Qng3dEdnYmdOOUtpbFJTNlJYWHNRb3RNN3FpR0tQT1l4SjFDbXBsSSJ9LCJwcCI6eyJhIjoiZGV0NDVpU3gwTUtLZ3czdU94Q2pYdV9VOGFkV2tzQ2kwUjdpckJZbUpKWSIsInIiOlsia3lRbmVJcnBGZGZHTTJIQ2VnVTloQnlKeVFDWHRMWXNPSHBxLXdoaGpkYyJdfX0
```

This just looks like an opaque URI[^1], but if you installed the [browser extension](https://github.com/microsoft/web-attestation-sample/tree/main/browser-extension), it recognizes the UWA string, verifies the web attestation (valid issuer and scope), and displays a verified badge: <img src="img/uwa_blue_checkmark.svg" alt="UWA blue checkmark" style="height: 2ex;">.

The browser extension can also generate and verify a UWA encoded as QR code, which are useful for environment limiting the number of characters a user can provide (e.g., in social media bio or posts).

<div align='center'>
  <img src="img/uwa_qr.png" alt="UWA QR code" width="20%"><br/>
</div>

This web attestation doesn't attest to much; it just shows that I was able to set up the sample issuer, obtain demo tokens, and create a UWA for my blog post. This, however, demonstrates the mechanics of issuing and presenting such attestations. The project's README page walks through a [deployment example](https://github.com/microsoft/web-attestation-sample#deployment-example) describing how Alice can obtain U-Prove tokens from the `https://commun.ity` website and attach a corresponding membership attestation to her `https://soc.ial/@pr1v4cy` social profile, and how another user attach a humanity attestation to their page.

# Concluding thoughts

The UWA framework provides a simple, yet powerful mechanism, allowing users to openly share attestations of any type across boundaries, while preserving any degree of privacy they desire.  There are many subteleties and pitfalls when designing privacy-preserving systems; the project's [README](https://github.com/microsoft/web-attestation-sample/blob/main/README.md) and the [UWA specification](https://github.com/microsoft/web-attestation-sample/blob/main/doc/uwa-spec.md) contain important details to consider for a real deployment and discuss further extensions that would improve the system.

I'm curious to see what you think, so go try the project, and give us feedback.

# Footnotes

[^1]: The UWA string is is composed of a `uwa://` prefix followed by a JSON Web Siganture (JWS) encoding a [U-Prove token presentation](https://christianpaquin.github.io/2023-04-03-uprove-json-framework-overview.html). You can try [base64url-decoding](https://www.base64decode.org/) the three dot-separated part of the JWS to peak inside.