---
layout: post
title:  "Recent highlights on the quantum safe journey"
permalink: /:year-:month-:day-:title.html
---

NIST's [5th PQC Standardization Conference](https://csrc.nist.gov/Events/2024/fifth-pqc-standardization-conference) just concluded. It was a great event and always is a great opportunity to (re-)connect with my academic and industry colleagues. A lot has happened since the first iteration of the conference. In fact, things have accelerated greatly since the release of the [first PQC FIPS drafts](https://csrc.nist.gov/pubs/fips/203/ipd) last August.

Here are a few personal highlights:

* Microsoft established its [Quantum Safe Program](https://www.microsoft.com/en-us/security/blog/2023/11/01/starting-your-journey-to-become-quantum-safe/), helping customers on their quantum safe journey.
* The [Open Quantum Safe](https://openquantumsafe.org/) project has joined the Linux Foundation's [PQC Alliance](https://pqca.org/); this will allow the project to extend its activities and provide components that are better-suited for real-life deployments. It's been humbling to see the impact of OQS, a project I joined since its early days; it has been used by almost everyone test PQC algorithms and prototype their integration into protocols and various systems. In fact, if a vendor is offering a PQC solution today; it likely uses OQS.
* NIST released the first draft reports of the [NCCoE PQC Migration](https://www.nccoe.nist.gov/crypto-agility-considerations-migrating-post-quantum-cryptographic-algorithms) project. I have the pleasure of leading the project's Interoperability and Performance workstream, and our [report](https://www.nccoe.nist.gov/sites/default/files/2023-12/pqc-migration-nist-sp-1800-38c-preliminary-draft.pdf) describes results of testing quantum-safe algorithms in TLS, SSH, QUIC, X.509, and HSMs, using various software and hardware components from my esteemed collaborators.
* In January, the White House held a [PQC roundtable meeting](https://www.whitehouse.gov/omb/briefing-room/2024/02/12/readout-of-white-house-roundtable-on-protecting-our-nations-data-and-networks-from-future-cybersecurity-threats/) I had the honor of attending. It's good to see PQC leadership at the highest level to help move the transition forward.
* Just last week, my colleagues announced an [important milestone](https://cloudblogs.microsoft.com/quantum/2024/04/03/how-microsoft-and-quantinuum-achieved-reliable-quantum-computing/) in building a reliable quantum topological qubit, bringing us one step closer to realizing the quantum vision, and reminding us that the transition work is very important.

## Once upon a time

I've been on a quantum journey for a long time. Back when my fellow Canadian Bryan Adams's "Have You Ever Really Loved a Woman?" was topping the charts, I started to study with Gilles Brassard (the co-inventor of [Quantum Key Distribution (QKD)](https://en.wikipedia.org/wiki/BB84) and [Quantum Teleportation](https://en.wikipedia.org/wiki/Quantum_teleportation)) at the University of Montreal. The whole lab was then focussed on this crazy new idea of using quantum mechanics to build computing models. I too caught the quantum bug and ended up specializing in quantum cryptography, designing ways to use quantum error correcting codes to extend QKD distances. During that time, Peter Shor and Lov Grover published their famous algorithms. I left the academic world right before Y2K and started a career working on a more "practical" side of cryptography engineering.

Decades later, Quantumania is in full force. I'm excited by the possibilities of the quantum revolution, a dream that is becoming reality, and I'm proud of the hard preparatory work our industry is doing to keep our systems safe.

What a journey. Onward!
