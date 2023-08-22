# Enforced txn randomisation - an additional Gasper Gadget

## Motivation
*What problem is your project is solving? Why is it important and what area of the protocol will be affected?*

It would be best for the Ethereum protocol to contain MEV as MEV is a major contributor to increased L1 transaction fees (when there is congestion and contestion from MEV tactics), and is also a source of unfair transacting, such as when searchers front-run or sandwich other users. Fundamentally, searching is made possible by searchers (re)ordering and selectively in-/excluding user transactions which censors other users. The full extent of MEV strategies on Ethereum is not known, but its effects are increasingly discovered and studied.

The lowest level approach for MEV control is methodology at the block creation level that would enforce standard ordering. However, fair transaction sequencing seems impossible in corner cases (which we review); thus we propose enforced randomisation of block ordering on the consensus layer as a next best proxy. This mechanism requires additional steps to be taken during consensus with special care to prove that the propagated block includes a verifiably randomised list of transactions whilst adhering to certain non-negotiable rules such as account nonce ordering.

## Project description
*What is your proposed solution?*

We propose an addition to the Ethereum Gasper consensus mechanism for the proposer to propogate a randomly shuffled / reordered version of the proposer's selected block. This mechanism add-on would be blockchain-agnostic and can work under both a PoW or PoS world, with or without ePBS.

This proposal requires further consensus rules for (A) nodes to see and come to consensus on the proposer's initially proposed block and (B) most importantly, that consensus agree / is confident that the final propagated version of the block is due to a random shuffling outside the proposer's control.

The solution should (i) require a minimal amount of additional time which include all further rounds of node communication and (ii) requires a minimal amount of data for communication (including for example verification messages).

There are two paths forward (1) use of a proof that cannot be tampered with and is generated from an in-protocol randomisation shuffle (other nodes can rust this proof and keep the proposer honest) (2) more than one proposer is responsible in order to keep each proposer honest.

There are no other expected changes to Gasper itself.

For context we want to make sure that we understand all open proposals for ePBS as ePBS is currently the best step forward in controlling MEV. We dissect the original PBS problem and review the most viable and popular live proposals. Worthy to note, most ePBS solutions also incorporate the enablement of stateless validators and further decentralise / distribute equity amongst validators, which might be attribues we want to consider in our proposal.

## Specification
*How will you implement your solutions? Give details and more technical information on the project.*

The project will include:
- An in-depth explanation of the proposed mechanism.
- Proofs that the mechanism is logically sound.
- POC for implementation in Prysm, and Geth if necessary (currently unsure if the implementation would only require a change in consensus).

## Roadmap
*What is your proposed timeline? Outline parts of the project and insight on how much time it will take to execute them.*

**Fellowship**

**Aug 21-Sep 3**
- Read on challenges in fair transaction sequencing / ordering (list to be provided with short analyses)
- Explore in detail previous / dead proposals on transaction randomisation.
- Explore proof types for randomisation.
- Select a path for moving forward with the randomisation proposal either (1) or (2).
    - (1) a proof that cannot be tampered with that is generated from an in-protocol randomisation shuffle (other nodes can trust the proof and keep the proposer honest)
    - (2) more than one proposer is responsible in order to keep each proposer honest.
- Continue research on MEV.
- Read and dissect most recent MEV-mitigation proposals, including:
    - [State of research: increasing censorship resistance of transactions under proposer/builder separation (PBS)](https://notes.ethereum.org/@vbuterin/pbs_censorship_resistance)
    - [Unbundling PBS: Towards protocol-enforced proposer commitments (PEPC)](https://ethresear.ch/t/unbundling-pbs-towards-protocol-enforced-proposer-commitments-pepc/13879?u=barnabe) by Barnabé Monnot
    - *Review fellow cohort member's [project](https://hackmd.io/@oLeaCeNDTl-O01HPNj9_sw/r1eZd51g3n) as he is building out the specifications for PEPC.
    - [Why enshrine Proposer-Builder Separation? A viable path to ePBS](https://ethresear.ch/t/why-enshrine-proposer-builder-separation-a-viable-path-to-epbs/15710) by Mike Neuder and Justin Drake
    - [MEV burn—a simple design (a PBS add-on)](https://ethresear.ch/t/mev-burn-a-simple-design/15590) by Justin Drake
    - [SUAVE](https://writings.flashbots.net/mevm-suave-centauri-and-beyond) by Flashbots
    - Understand and spec out what the abandoned paths forward were:
    - [Forward-inclusion](https://notes.ethereum.org/@fradamt/forward-inclusion-lists) / [Proposer Broadcast lists](https://notes.ethereum.org/@fradamt/H1f3PlB5Y) `crlist`
    - [Parallelisation of block construction](https://notes.ethereum.org/@vbuterin/pbs_censorship_resistance), one of the two possible solutions proposed in Vitalik's original post

**Sep 4-17**
- Write a full proposal for an addition to the Ethereum Gasper consensus mechanism to propogate a randomly shuffled / reordered version of the proposer's committed block. This mechanism add-on is blockchain-agnostic and works under both a PoW or PoS with or without ePBS.
    - This proposal requires further consensus rules for (A) nodes to see and come to consensus on the proposer's initial committed block and (B) most importantly, that consensus agree the final propagated version of the block is due to a random shuffling outside the proposer's control.
        - The solution should (i) require a minimal amount of additional time which include all further rounds of node communication and (ii) a minimal amount of data included in the communication.
- There are no expected changes to Gasper.
- Build a theorem and related proofs.

**Sep 18-Oct 31**
- Develop a POC for Geth and Prysm.

## Possible challenges
*What are the limitations and issues you may need to overcome?*
- The solution may be too costly for on-chain implementation (in terms of run time and data for example), but these parameters should be adjustable if the crux of the gadget is intact.
- The proposal will require many further conversations with those who understand more than I do to form it well.

## Goal of the project
*What does success look like? Describe the end goal of the project, scope, state and impact for the project to be considered finished and successful.*

If the proposal results in a more formal proposal for the randomisation gadget that would be ideal. If the proposal is food for thought or a gadget that can be used by another protocol that would also be great.

## Collaborators

Many thanks for the conversations with Mario, Theo, and Bharath.

### Mentors
*Which mentors are helping you with the project?*

[Marius Van Der Wijden](https://github.com/MariusVanDerWijden)<br>
[Alex Stokes](https://github.com/ralexstokes)<br>
[Barnabé Monnot](https://github.com/barnabemonnot)

### Resources
*Provide links to repositories, PRs and other resources which constitute the project.*
- [Geth](https://github.com/ethereum/go-ethereum)
- [Prysm](https://github.com/prysmaticlabs/prysm)
