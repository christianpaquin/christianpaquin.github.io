---
layout: post
title:  "Crescent Credentials"
permalink: /:year-:month-:day-:title.html
---

Providing strong privacy for identity credentials is becoming an important goal. Recent frameworks such as the
[Selective Disclosure for JSON Web Tokens (SD-JWT)](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-selective-disclosure-jwt-13) and
[mobile Driver’s License (mDL)](https://www.aamva.org/topics/mobile-driver-license) support selective disclosure of attributes to
prevent data over-sharing. This is an important capability, but a critical one that is hard to achieve is missing from both: unlinkability.

An unlinkable (a.k.a. untraceable or untrackable) credential is one that can be issued to a user and presented to a verifier without any data correlations
between these two actions.[^1]
In particular, nothing in the credential's construct (e.g., serial numbers, cryptographic values such as public keys and signatures, etc.) could be (mis-)used
as a correlation handle (other than the attributes themselves, which of course could identify a user if disclosed). To achieve this feature, you not only need
to sanitize the always-disclosed data to avoid linkable values (e.g., serial numbers, GUIDs, validity periods, etc.), but you also need to use special
cryptography to avoid creating inescapably linkabable values (e.g., the issuer signature which acts as a unique fingerprint). I've
described this in details in the [Where's Waldo Been post](https://christianpaquin.github.io/2023-06-16-where's-waldo-been.html).

<div align='center'><img src="img/crescent_unlinkability.png" alt="Unlinkability" width="60%"><br/></div><br>

There are two general strategies to achieve unlinkability in an identity system. The first one is to use a cryptographic signature scheme that supports unlinkability,
such as blind signatures (e.g., [U-Prove](https://microsoft.com/uprove)) or proofs-of-knowledge
(e.g., [BBS](https://identity.foundation/bbs-signature/draft-irtf-cfrg-bbs-signatures.html)).[^2] One challenge with this approach is that it requires major changes
to current identity issuance systems: new algorithms need to be standardized, implemented, and integrated into various platforms. BBS is currently being standardized
by the IETF, but even when that concludes, there will be a lot of integration work required to match the ubiquity of a RSA or an ECDSA, and it will be difficult to 
ask for an identity ecosystem overhaul when the industry is already preparing for the [post-quantum cryptographic](https://christianpaquin.github.io/2024-04-12-recent-highlights-on-the-quantum-safe-journey.html) transition. 

The second strategy is to present conventional, existing credentials in an unlinkable way using zero-knowledge proofs. We recently released
[Crescent](https://github.com/microsoft/crescent-credentials), a cryptographic library that achieves exactly that. Zero-knowledge proofs are
cryptographic building blocks with seemingly magical properties, allowing a prover to convince a verifier of the veracity of certain facts about some data
without disclosing the data and without the verifier learning anything more than the statements being proven (the verifier gains "zero" extra knowledge). These primitives were introduced close
to 40 years ago, and have only recently become efficient enough for use in practice. Crescent introduces two important properties, the ability to:
1. share the large zero-knowledge parameters among all issuers using the same credential schema and signing algorithm (e.g., you would only need one set for the [AAMVA](https://www.aamva.org/) mDL ecosystem); and
2. offload the expensive zero-knowledge user calculations to a "prepare" stage that only needs to run once asynchronously
per credential; subsequent presentations and verifications are then very efficient.

Crescent currently supports two types of credentials: JSON Web Tokens (JWT) and mobile Driver's Licenses (mDL); more are on our roadmap (such as X.509).
To learn more about Crescent, consult our [technical paper](https://eprint.iacr.org/2024/2013).

We've created a sample to illustrate the capabilities and practicality of the system.

For simplicity, the sample defines its own issuance and presentation protocol, but it is easy to imagine how this could be integrated
into higher level identify framework (e.g., OpenID/OAuth, Verifiable Credentials, mDL ecosystem); some of our future work will
focus on these integrations.

You can find details about the sample [here](https://github.com/microsoft/crescent-credentials/blob/main/sample/README.md),
but in summary:

<div align='center'><img src="img/crescent_sample_arch.png" alt="Crescent sample application" width="75%"><br/></div><br>

1. A Crescent Service has pre-generated the zero-knowledge parameters to create and verify zero-knowledge proofs
from JWTs and mDLs.
2. The user has a pre-imported (mock up) mDL.
3. The user obtains a proof-of-employment JWT from her employer Contoso.
4. These credentials are stored in the browser extension Crescent wallet that communicates with a local Client Helper whose role is to handle the heavy computation and storage.
5. The user presents an employment proof using her JWT to a mental health clinic Fabrikam. 
6. The user presents an over-18 proof using her mDL to a social network.

You can see these components in action in this [demo video](https://www.youtube.com/watch?v=3KJeUhiL_cU).

<div align='center'><a href="https://www.youtube.com/watch?v=3KJeUhiL_cU">
<img src="img/crescent_youtube.png" alt="Crescent demo on YouTube" width="40%"><br/>
</a></div><br>

It's exciting to see the rapid development in zero-knowledge proof research, which brings the technology closer to deployment in
real-life systems. We continue working on each part of the system, from the low-level cryptographic building blocks
to the integration in the identity layer. Stay tuned...

# Footnotes

[^1]: Technically, an observer (which could even be the issuer, the verifier(s), or both in collusion!) looking at the issuance and presentation messages should not be able to tell which user presented which credential. Some timing (e.g., close issuance and presentation time) or network (e.g, IP address) metadata, or presented attributes could, of course, leak some information; these can be addressed using various privacy-maximizing strategies.

[^2]: I've compared both approaches in [this blog post](https://christianpaquin.github.io/2023-07-13-of-u-prove-and-bbs.html).

