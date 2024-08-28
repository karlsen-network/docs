# Karlsen Documentation

[This repo is a work in progress]

To connect with the Karlsen community, join the Discord [here](https://discord.gg/ZPZRvgMJDT).

[![Join the Karlsen Discord Server](https://img.shields.io/discord/1169939685280337930.svg?label=&logo=discord&logoColor=ffffff)](https://discord.gg/ZPZRvgMJDT)

## Overview

Karlsen is a fork of [Kaspa](https://github.com/kaspanet/kaspad)
introducing a GPU-centric fork as a solution to the dominance of ASIC
mining farms, aiming to empower small-scale miners and enhance
decentralization. We focus on bridging the gap between blockchain
technology, decentralized finance and the real world of payment systems
and traditional finance.

With Kaspa, our approach is one of friendly cohabitation. We operate
under the same protocol, enjoy similar advantages, and face
unidentified issues. Any significant improvements that need to be made
in the primary codebase will be shared back.

## Small Scale Miners

The Karlsen Network team believes in decentralization and small-scale
miners. We will ensure long-term GPU-friendly mining.

### Hashing Function

We initially started with `kHeavyHash` and `blake3` modifications
on-top. This algorithm is called `KarlsenHashv1`. However `kHeavyHash`
and `blake3` are not future proof in ASIC resistence. Therefore we've
launched already our `testnet-1` with [FishHash](https://github.com/iron-fish/fish-hash/blob/main/FishHash.pdf).
It is the worlds first implementation of FishHash with Golang in a
1bps blockchain.

`KarlsenHashv1` is currently used in [mainnet](https://github.com/karlsen-network/karlsend/releases/tag/v1.1.0)
and can be mined using the following miners maintained by the Karlsen
developers:


## Table of Contents

1. [Getting Started](Getting%20Started/README.md)
   * [Rust Full Node Installation](Getting%20Started/Rust%20Full%20Node%20Installation.md)
