\====================================================================

**Authors**: Ximin Luo

**Created**: 2020-05-08

\====================================================================

====================
Parachain networking
====================

TODO: this document will discuss the networking interface between parachains
and the relay chain.

Background
==========

- Collators need to send PoV block information to their parachain validators.

- Ids of parachain validators are on-chain and public knowledge

- Collators are in general unauthenticated

  - In future, the parachain validation function may supply an interface that
    allows the parachain to restrict its collator membership set, this will
    take a while to develop.

- The task is to distribute PoV block data from various entities to parachain
  validators. Some of these entities may be malicious, some may be actual
  collators.

- Blocks are unauthenticated during distribution, however after distribution
  once we've received it fully, we can run the validation function on it to
  authenticate it.

  In the meantime, we have to buffer potentially bad data from malicious nodes,
  but we can remember this information and prefer peers with good history.

  - In future, the parachain validation function may supply an interface that
    allows the parachain to specify a way to pre-validate a block, e.g. that it
    must be signed by specific keys.

Current thoughts
================

- Block data may be split, so that we can receive different pieces from
  multiple peers at once. This can be done e.g. by a merkle tree, with the root
  hash being the identifier of the block. (As per the previous point, this root
  hash would be unauthenticated during distribution).

- Parachain validators should be connected to each other, to help with
  distribution.
