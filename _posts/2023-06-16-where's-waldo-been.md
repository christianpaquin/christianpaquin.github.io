---
layout: post
title:  "Where's Waldo been?"
permalink: /:year-:month-:day-:title.html
---

<div style="float: left; margin-right: 20px; margin-bottom: 20px;">
  <img src="img/waldo.jpg" alt="Waldo" title="Waldo">
</div>

Let’s imagine an identity-themed game of [Where’s Waldo](https://waldo.candlewick.com/). The goal is to figure out where Waldo used an identity document, say his driver’s license.[^1] The credential, issued by the DMV (which I'll also call the issuer), contains his name, his date of birth, and his address. Now let’s say that Waldo presents his credential at a casino to prove he is over 18.[^2]

Now, can the player – let’s call him Odlaw, Waldo’s infamous nemesis – figure out that Waldo visited this location after the fact? We assume Odlaw is very powerful, and has access to the DMV’s and the casino’s internal systems (e.g., logs, employees, etc.) Can he track the usage of Waldo’s identity document? 

<div style="float: right; margin-left: 20px; margin-bottom: 20px;">
  <img src="img/odlaw.jpg" alt="Odlaw" title="Odlaw, Waldo's nemesis">
</div>

In real life[^3] it would be hard for Odlaw to learn where Waldo presented his driver’s license. Given that the casino clerk is not “plugged-in the Matrix” and likely doesn’t have photographic memory, the simple enter-or-not decision they make after glancing at the driver’s license protects Waldo’s privacy fairly well. (I deliberately ignore here other information sources such as surveillance cameras, ATM usage, etc.)

Now, let’s transpose this game online. Waldo now obtains an electronic version of his driver’s license and presents it at an online casino.

If the credential (a.k.a. the identity token) is obtained using a federated protocol such as OpenID Connect or OAuth, then it is retrieved on-demand from the issuer who then learns where the user is coming from,[^4] therefore allowing Odlaw to trivially win the game.

To make the game more difficult, Waldo should therefore obtain a credential that can be presented to any web site without disclosing the destination to the issuer. Let’s say this happens, then Waldo can present his e-license to enter the online casino; Odlaw must now work a bit harder to figure out where Waldo has been. Since the online casino’s system has a long and perfect memory (vs. the human checking the physical ID), it will remember (in its logs) not only Waldo’s date of birth, but also his name and address. To find Waldo, Odlaw simply needs to get access to the casino’s audit log.

Ok, ok, wait, why would Odlaw do that? Well, we’re playing a game here, so it’s Odlaw’s goal to find Waldo. It may feel like a weird game for now, but keep reading…

What if Waldo could only show his date of birth without also disclosing his name and address; would that solve the issue? Well, it would certainly help. There are different strategies to achieve this, the most practical would be to use a mechanism to disclose only the attributes (a.k.a. claims) requested by the web site. The [ISO mobile Driver License (mDL)](https://www.iso.org/standard/69084.html) standard allows just that, by using a hash-based selective disclosure mechanism.[^5] Using such a credential, Waldo would only disclose his date of birth, therefore reducing the disclosed information and making it harder to find him. A birth date is however more information than strictly needed by the casino; they only care that their visitors are over 18. Inspecting the casino’s logs and learning the disclosed birth dates would give Odlaw an unfair statistical advantage in the game.

It’s easy to fix this by having the issuer include an over-18 binary attribute in the credential, allowing users to only disclose its true/false value.[^6] Ok, if Waldo uses this new credential, the only thing Odlaw would learn by studying the logs is that the visitor is over-18, without knowing which credential owner presented it.

<div style="float: left; margin-right: 20px; margin-bottom: 20px;">
  <img src="img/spyglass-fingerprints.png" alt="fingerprints">
</div>

This is not the end of the game, however; although the disclosed attribute value is close to anonymous, its cryptographic container is not! Indeed, the credential is signed by the issuer, and the digital signature is a unique number that acts as a digital fingerprint that can identify Waldo. To figure out where’s Waldo, Odlaw simply needs to correlate the signature between the issuer's logs and the casino’s.

Ok, let’s take some time to justify the game. Who is Odlaw, and why would he want to track and trace Waldo across the web? Well, perhaps Odlaw is a data broker or ad platform, amassing large quantities of user data, analyzing leaked logs, having web signals on many sites, or having access to insiders at the DMV or the casino (data which can later be sold to various governments, as [was confirmed in the US](https://www.wsj.com/articles/u-s-spy-agencies-buy-vast-quantities-of-americans-personal-data-report-says-f47ec3ad) this week). Perhaps he is an oppressive government trying to track a specific (set of) dissident user(s), to block (censor) their access or retaliate against them. Maybe Odlaw is a private investigator or a paparazzo, trying to dig dirt on politicians or celebrities. The web as we know it was built on a business model relying on tracking user activities through all sorts of mechanisms, some of which are now being addressed through technical means (e.g., elimination of 3rd party cookies) and legislation (e.g., GDPR). As more digital identity and authentication systems are being built (some of which in response to the [threat of AI](./2023-03-22-auth-all-the-things.md)), using inescapably traceable cryptography would result in a dangerous privacy decrease.

How can Waldo prevent this unfair searching capability? The issue is that a unique value (the signature) is applied on the credential by the issuer and is shown as-is by Waldo to the casino. Fortunately, cryptography isn’t necessarily a source of woe for Waldo; it also gives us the tools to break the linkage between the issuance and presentation of the credential. There are two main techniques to do so: Waldo can either randomize the issuer’s signature before presenting it to the casino or convince the casino that his credential is correctly signed without disclosing the signature itself.[^7] The former technique involves so-called blind signatures (as used in [U-Prove](https://microsoft.com/uprove)), and the latter involves zero-knowledge proofs (as in [BBS](https://github.com/decentralized-identity/bbs-signature)).[^8] Using these privacy-preserving signature mechanisms, Waldo can receive a credential with various attributes, and minimally disclose[^9] an anonymous over-18 claim without fear of being tracked. Indeed, by inspecting the logs of the issuer and the casino, Odlaw can’t figure out if Waldo has been here.

Are we done? Is the "Where’s Waldo been" game impossible to win now? Perhaps in theory, but not in practice. There are many signals that could still be used to track Waldo, e.g., timing correlations between issuance and presentation, or transport layer leakage (e.g., IP address tracing). These must be addressed separately, and we concentrate here on data privacy. There are many subtleties that we must consider to clean up all correlatable information in the credential. For example, long-lived identity credentials have an expiration date, which if precise enough could be used as a unique fingerprint to identity its owner. Credentials should therefore encode validity periods using statistically hiding ranges or buckets, to obfuscate to whom they belong. Privacy-minded deployers must therefore check all the credential’s metadata for information that could (help) identify the user (revocation information, validity periods, usage restrictions, etc.)

Ok, surely now, we must be done. If Waldo has a privacy-protecting credential, supporting minimal disclosure allowing him to only prove he’s over 18 in an unlinkable manner, and makes sure to leave time between credential issuance and presentation, and obfuscate his IP address and other fingerprintable signals by using TOR, surely Odlaw can’t win the "Where’s Waldo been" game.

Weeeeell… the game isn’t called where’s Wilma or where’s Wenda, it’s called where’s Waldo. If Odlaw (again, this could be the issuer, an insider, a hacker, a 3rd party, the destination website, or any combination of these) is reeeally motivated to figure out where _Waldo_ is going, they could try to tag him in some fashion.[^10]

What if the issuer orders the attributes differently in Waldo’s credential, e.g., everybody has a `[name,address,DoB,over-18]` credential but Waldo receives a `[name,address,over-18,DoB]` one? Easy then to spot the needle in the log stack. What if Waldo contains a capitalized “True” value for is over-18 attribute, while all other adults have a “true” value? To minimize the risk of leaking such information, the attribute schema should be made public by the issuer as part of its parameters, and Waldo’s client should check the proper formatting of his credential and values before presenting it.

<div align="center">
  <img src="img/waldo-haystack.png" alt="Waldo in a logstack" title="Waldo in a logstack" width="25%">
</div>

What if the issuer uses a different set of parameters (and signing key) for Waldo than for other users? As if it would sign everyone’s credential using its “blue” pen but would use its “red” one for Waldo's. It would again be trivial to spot Waldo in the web logs. This is a difficult problem to tackle, and key (and parameter) transparency techniques ([like in PKI](https://en.wikipedia.org/wiki/Certificate_Transparency)) can help Waldo protect his privacy.

I hope you have come to appreciate that deploying privacy-protecting identity credentials is a challenging effort. Even when using proper cryptography, the identity framework must offer sufficient protections to minimize data leakage. This is what we aimed to do when designing the [U-Prove JSON Framework (UPJF)](2023-04-03-uprove-json-framework-overview.md), where the token’s public key and signature are randomized, attribute schemas are specified in the public issuer parameters, and expiration dates are specified in privacy protecting “buckets.” The [User-centric Web Attestation](./2023-05-30-user-centric-web-attestations.md) framework further constrains the UPJF by forcing issuers to publish the attribute schema (indexed set of valid claim values) as part of their public parameters.

In an identity ecosystem where privacy-protecting cryptography is used, and where audit mechanisms are in place to make sure issuers are well-behaved, the "Where's Waldo been" game becomes very difficult to win for Odlaw, which in turn is a win for Waldo and all internet users.

<div align='right'>
  <img src="img/woof.png" alt="Woof the dog" title="Woof the dog" width="20%">
</div>
# Footnotes

[^1]: This post has a north American bias, using the name Waldo (vs. the original Wally) and a driver's license as an example identity document; I’ll leave it as an exercise to the reader to mentally localize the examples (I’m for example replacing Waldo with Charlie, from my cherished childhood “Où est Charlie?” books).

[^2]: Let’s say the casino is in Washington state where the gambling age is 18, which matches many non-US jurisdictions.

[^3]: Not to say that online activities aren’t an integral part of life, but you know what I mean…

[^4]: Very often, the destination is encoded in the token to prevent an attacker from stealing and reusing it. Even if not explicitly stated in the token itself, the web issuer must redirect the user’s browser/app to the destination, therefore learning which site is visited.

[^5]: Instead of encoding the attributes directly, the issuer encodes a salted hash digest allowing the user to disclose the attributes of their choice by attaching their pre-image values. The OAuth [SD-JWT](https://github.com/oauth-wg/oauth-selective-disclosure-jwt) specification provides a similar mechanism for general JSON Web Tokens.

[^6]: Some advance cryptographic techniques, e.g., zero-knowledge proofs allow users to prove that their birth date is such that they are over-18 without disclosing it, but these are quite complex and it’s unclear if we'll see them deployed at scale anytime soon. Even if it were practical to do so, it is way simpler for this use case to encode a couple of useful Boolean values (e.g., over-13, over-18, over-21, over-65) rather then using the more complicated approach. In security, simplicity is your friend…

[^7]: Identity credentials often encode a public key which is also a unique number that must be dealt with using these same techniques. I omit it for simplicity here.

[^8]: These two classes of schemes have different pros and cons; I'll write more about them in a follow-up post. Other simpler mechanisms exist to issue unlinkable "membership" credentials without attributes, such as [privacy pass](https://datatracker.ietf.org/wg/privacypass/about/), or this [recent proposal](https://eprint.iacr.org/2022/1622) by my esteemed MSR colleagues.

[^9]: I use the term minimal disclosure to describe the ability to only present the required information, nothing less, nothing more. Sometimes, the minimum required is your full identity (e.g., if you board a plane), sometimes, it could simply be an over-18 proof (like in our example). The term selective disclosure is often used in systems where only a subset of the attributes are revealed. Selective disclosure is however not always minimal: if for example the signature is linkable, or if you are disclosing an attribute that reveals more information than required (reusing again our birth date vs. a simple over-18 claim example). More advanced zero-knowledge cryptographic techniques allow us to prove _properties_ of undisclosed attributes, e.g., that my undisclosed date of birth is such that I’m over 18, that my name doesn't appear on a block list, or that my state of residence is one of those allowing online gambling.

[^10]: Of course, the reader can imagine that the attacker could target a group of “Waldos”.

