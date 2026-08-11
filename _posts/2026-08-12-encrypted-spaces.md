---
layout: post
title:  "Encrypted Spaces"
permalink: /:year-:month-:day-:title.html
---

Most collaboration software asks us to make a difficult tradeoff: either work together conveniently in the cloud, or keep sensitive information under our direct control. Over the last decade, we have become accustomed to storing documents, conversations, files, and shared workspaces on centralized services that can see, process, and ultimately control access to our data.

End-to-end encryption has helped in some contexts, particularly for messaging. But collaborative applications remain challenging. It is relatively straightforward to encrypt a stream of messages between a small group of users. It is much harder to build a shared document editor, a database, a file system, or a project-management tool in which multiple users can concurrently modify state while keeping servers out of the trust boundary.

To address this problem, we recently released [Encrypted Spaces](https://github.com/encrypted-spaces/prototype), a research project exploring a different model for building collaborative applications. The project investigates architectures in which data remains encrypted and application operations can be cryptographically verified, allowing users to collaborate through infrastructure that stores and synchronizes data without being trusted with its contents.

## Why this matters

The cloud has transformed how we work together. Documents, spreadsheets, design tools, messaging platforms, and countless other applications now depend on centralized servers to coordinate shared state across users and devices. This model provides tremendous convenience, but it also creates concentration points for sensitive information. Anyone who gains access to those servers, whether through compromise, misuse, or legal compulsion, may gain access to the data stored there.

For many scenarios, this risk is acceptable; for others, it is not.

Journalists protecting confidential sources, human-rights organizations documenting abuses, healthcare workers coordinating care, researchers handling sensitive datasets, and community groups working across institutional boundaries often need stronger assurances about who can access their information. These use cases motivated a simple question:

<blockquote>
Can we build collaborative applications that retain the convenience of centralized infrastructure without requiring users to trust that infrastructure?
</blockquote>

Encrypted Spaces explores that hypothesis.

## What is an encrypted space?

An encrypted space is a shared collaboration environment with an authenticated history, dynamic membership, and a verifiable representation of application state. Servers still store data and coordinate synchronization, but clients verify that the servers behave correctly. Only authorized participants can access protected information.

From a developer's perspective, the goal is not to expose low-level cryptographic protocols. Developers should be able to work with familiar abstractions such as tables, lists, files, and text buffers while the underlying platform handles encryption, key management, proof verification, and synchronization.

The [GitHub project page](https://github.com/encrypted-spaces/prototype) provides a prototype SDK that allows developers to experiment with the concept. To illustrate the SDK's capabilities, the project offers a demo collaborative application.

<div align='center'><img src="img/encrypted_spaces_demo.png" alt="Encrypted Spaces demo app" width="75%"><br/></div><br>

## A research journey

Encrypted Spaces sits at the intersection of several research areas that have advanced dramatically in recent years. Verifiable data structures, modern group-key-management techniques, transparency systems, and increasingly practical zero-knowledge proofs can now be combined in ways that were difficult to imagine not long ago.

The project brings together researchers and practitioners from academia, industry, and civil society. Microsoft Research is one of the collaborators, with my colleague Greg Zaverucha leading our contribution; see the [project's website](https://encryptedspaces.org/) for details. To learn more, I encourage you to read the [white paper](https://encryptedspaces.org/whitepapers/encrypted-spaces.pdf).

We welcome feedback on the technology and our prototype SDK; let us know what you think!