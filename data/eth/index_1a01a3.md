---
title: Nodes and clients
description: An overview of Ethereum nodes and client software, plus how to set up a node and why you should do it.
lang: en
sidebarDepth: 2
---

Ethereum is a distributed network of computers (known as nodes) running software that can verify blocks and transaction data. The software application, known as a client, must be run on your computer to turn it into an Ethereum node.

**Note: it is not possible to run an execution client on its own anymore. After [The Merge](/upgrades/merge), both execution and consensus clients must be run together in order for a user to gain access to the Ethereum network.**

## Prerequisites {#prerequisites}

You should understand the concept of a peer-to-peer network and the [basics of the EVM](/developers/docs/evm/) before diving deeper and running your own instance of an Ethereum client. Take a look at our [introduction to Ethereum](/developers/docs/intro-to-ethereum/).

If you're new to the topic of nodes, we recommend first checking out our user-friendly introduction on [running an Ethereum node](/run-a-node).

## What are nodes and clients? {#what-are-nodes-and-clients}

A "node" is any instance of Ethereum client software that is connected to other computers also running Ethereum software, forming a network. A client is an implementation of Ethereum that verifies data against the protocol rules and keeps the network secure.

Post-Merge Ethereum consists of two parts: the execution layer and the consensus layer. Both layers are run by different client software. On this page, we'll refer to them as the execution client and consensus client.

- The execution client (also known as the Execution Engine, EL client or formerly the Eth1 client) listens to new transactions broadcasted in the network, executes them in EVM, and holds the latest state and database of all current Ethereum data.
- The consensus client (also known as the Beacon Node, CL client or formerly the Eth2 client) implements the proof-of-stake consensus algorithm, which enables the network to achieve agreement based on validated data from the execution client.

Before [The Merge](/upgrades/merge/), consensus and execution layer were separate networks, with all transactions and user activity on the Ethereum happening at what is now the execution layer. One client software provided both execution environment and consensus verification of blocks produced by miners.
The consensus layer, [the Beacon Chain](/upgrades/beacon-chain/), has been running separately since December 2020. It introduced proof-of-stake and coordinated the network of validators based on data from the Ethereum network.

With the Merge, Ethereum transitions to proof-of-stake by connecting these networks. Execution and consensus clients work together to verify Ethereum's state.

Modular design with various software pieces working together is called [encapsulated complexity](https://vitalik.ca/general/2022/02/28/complexity.html). This approach makes it easier to execute The Merge seamlessly and enables the reuse of individual clients, for example, in the [layer 2 ecosystem](/layer-2/).

![Coupled execution and consensus clients](./eth1eth2client.png)
Simplified diagram of a coupled execution and consensus client.

### Client diversity {#client-diversity}

Both [execution clients](/developers/docs/nodes-and-clients/#execution-clients) and [consensus clients](/developers/docs/nodes-and-clients/#consensus-clients) exist in a variety of programming languages developed by different teams.

Multiple client implementations can make the network stronger by reducing its dependency on a single codebase. The ideal goal is to achieve diversity without any client dominating the network, thereby eliminating a potential single point of failure.
The variety of languages also invites a broader developer community and allows them to create integrations in their preferred language.

Learn more about [client diversity](/developers/docs/nodes-and-clients/client-diversity/).

What these implementations have in common is they all follow a single specification. Specifications dictate how the Ethereum network and blockchain functions. Every technical detail is defined and specifications can be found as:

- Originally, the [Ethereum Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf)
- [Execution specs](https://github.com/ethereum/execution-specs/)
- [Consensus specs](https://github.com/ethereum/consensus-specs)
- [EIPs](https://eips.ethereum.org/) implemented in various [network upgrades](/history/)

### Tracking nodes in the network {#network-overview}

Multiple trackers offer a real-time overview of nodes in the Ethereum network. Note that due to the nature of decentralized networks, these crawlers can only provide a limited view of the network and might report different results.

- [Map of nodes](https://etherscan.io/nodetracker) by Etherscan
- [Ethernodes](https://ethernodes.org/) by Bitfly
- [Ethereum Node Crawler](https://crawler.ethereum.org/)
- [Nodewatch](https://www.nodewatch.io/) by Chainsafe, crawling consensus nodes

## Node types {#node-types}

If you want to [run your own node](/developers/docs/nodes-and-clients/run-a-node/), you should understand that there are different types of node that consume data differently. In fact, clients can run three different types of nodes: light, full and archive. There are also options of different sync strategies which enable faster synchronization time. Synchronization refers to how quickly it can get the most up-to-date information on Ethereum's state.

### Full node {#full-node}

- Stores full blockchain data (although this is periodically pruned so a full node does not store all state data back to genesis)
- Participates in block validation, verifies all blocks and states.
- All states can be derived from a full node (although very old states are reconstructed from requests made to archive nodes).
- Serves the network and provides data on request.

### Light node {#light-node}

Instead of downloading every block, light nodes download block headers. These headers only contain summary information about the contents of the blocks. Any other information required by the light node gets requested from a full node. The light node can then independently verify the data they receive against the state roots in the block headers. Light nodes enable users to participate in the Ethereum network without the powerful hardware or high bandwidth required to run full nodes. Eventually, light nodes might run on mobile phones or embedded devices. The light nodes do not participate in consensus (i.e. they cannot be miners/validators), but they can access the Ethereum blockchain with the same functionality and security guarantees as a full node.

The execution client Geth includes a [light sync](https://github.com/ethereum/devp2p/blob/master/caps/les.md) option. However, a light Geth node relies upon full nodes serving light node data. Few full nodes opt to serve light node data, meaning light nodes often fail to find peers.

Light clients are an area of active development for Ethereum and we expect to see new light clients for the consensus layer and execution layer soon.
There are also potential routes to providing light client data over the [gossip network](https://www.ethportal.net/). This is advantageous because the gossip network could support a network of light nodes without requiring full nodes to serve requests.

Ethereum does not support a large population of light nodes yet, but light node support is an area expected to develop rapidly in the near future. In particular, clients like [Nimbus](https://nimbus.team/), [Helios](https://github.com/a16z/helios), and [LodeStar](https://lodestar.chainsafe.io/) are currently heavily focused on light nodes.

### Archive node {#archive-node}

- Stores everything kept in the full node and builds an archive of historical states. It is needed if you want to query something like an account balance at block #4,000,000, or simply and reliably test your own transactions set without mining them using tracing.
- This data represents units of terabytes, which makes archive nodes less attractive for average users but can be handy for services like block explorers, wallet vendors, and chain analytics.

Syncing clients in any mode other than archive will result in pruned blockchain data. This means, there is no archive of all historical states but the full node is able to build them on demand.

## Why should I run an Ethereum node? {#why-should-i-run-an-ethereum-node}

Running a node allows you to directly, trustlessly and privately use Ethereum while supporting the network by keeping it more robust and decentralized.

### Benefits to you {#benefits-to-you}

Running your own node enables you to use Ethereum in a private, self-sufficient and trustless manner. You don't need to trust the network because you can verify the data yourself with your client. "Don't trust, verify" is a popular blockchain mantra.

- Your node verifies all the transactions and blocks against consensus rules by itself. This means you don’t have to rely on any other nodes in the network or fully trust them.
- You can use an Ethereum wallet with your own node. You can use dapps more securely and privately because you won't have to leak your addresses and balances to random nodes. Everything can be checked with your own client. [MetaMask](https://metamask.io), [Frame](https://frame.sh/), and [many other wallets](/wallets/find-wallet/) offer RPC-importing, allowing them to use your node.
- You can run and self-host other services which depend on data from Ethereum. For example, this might be a Beacon Chain validator, software like layer 2, infrastructure, block explorers, payment processors, etc.
- You can provide your own custom [RPC endpoints](https://ethereum.org/en/developers/docs/apis/json-rpc/). Publicly for the community or even privately hosted Ethereum endpoint enables people to use your node and avoid big centralized providers.
- You can connect to your node using **Inter-process Communications (IPC)** or rewrite the node to load your program as a plugin. This grants low latency, which helps a lot, e.g. when processing a lot of data using web3 libraries or when you need to replace your transactions as fast as possible (i.e. frontrunning).
- You can directly stake ETH to secure the network and earn rewards. See [solo staking](https://ethereum.org/en/staking/solo/) to get started.

![How you access Ethereum via your application and nodes](./nodes.png)

### Network benefits {#network-benefits}

A diverse set of nodes is important for Ethereum’s health, security and operational resiliency.

- Full nodes enforce the consensus rules so they can’t be tricked into accepting blocks that don't follow them. This provides extra security in the network because if all the nodes were light nodes, which don't do full verification, validators could attack the network.
- In case of an attack which overcomes the crypto-economic defenses of [proof-of-stake](/developers/docs/consensus-mechanisms/pos/#what-is-pos), a social recovery can be performed by full nodes choosing to follow the honest chain.
- More nodes in the network result in a more diverse and robust network, the ultimate goal of decentralization, which enables a censorship-resistant and reliable system.
- They provide access to blockchain data for lightweight clients that depend on it. In high peaks of usage, there need to be enough full nodes to help light nodes sync. Light nodes don't store the whole blockchain, instead they verify data via the [state roots in block headers](/developers/docs/blocks/#block-anatomy). They can request more information from blocks if they need it.

If you run a full node, the whole Ethereum network benefits from it.

## Running your own node {#running-your-own-node}

Interested in running your own Ethereum client?

For a beginner-friendly introduction visit our [run a node](/run-a-node) page to learn more.

If you're more of a technical user, dive into more details and options on how to [spin up your own node](/developers/docs/nodes-and-clients/run-a-node/).

## Alternatives {#alternatives}

Setting up your own node can cost you time and resources but you don’t always need to run your own instance. In this case, you can use a third party API provider like [Infura](https://infura.io), [Alchemy](https://alchemyapi.io), [Chainstack](https://chainstack.com), [QuikNode](https://www.quiknode.io), [Tenderly](https://tenderly.co/web3-gateway), or [Blast](https://blastapi.io/). Alternatively, [ArchiveNode](https://archivenode.io/) is a community-funded Archive node that hopes to bring archive data on the Ethereum blockchain to independent developers who otherwise couldn't afford it. For an overview of using these services, check out [nodes as a service](/developers/docs/nodes-and-clients/nodes-as-a-service/).

If somebody runs an Ethereum node with a public API in your community, you can point your light wallets (like MetaMask) to a community node [via Custom RPC](https://metamask.zendesk.com/hc/en-us/articles/360015290012-Using-a-Local-Node) and gain more privacy than with some random trusted third party.

On the other hand, if you run a client, you can share it with your friends who might need it.

## Execution clients (formerly 'Eth1 clients') {#execution-clients}

The Ethereum community maintains multiple open-source execution clients (previously known as 'Eth1 clients', or just 'Ethereum clients'), developed by different teams using different programming languages. This makes the network stronger and more [diverse](/developers/docs/nodes-and-clients/client-diversity/). The ideal goal is to achieve diversity without any client dominating to reduce any single points of failure.

This table summarizes the different clients. All of them pass [client tests](https://github.com/ethereum/tests) and are actively maintained to stay updated with network upgrades.

| Client                                          | Language | Operating systems     | Networks                           | Sync strategies                    | State pruning   |
| ----------------------------------------------- | -------- | --------------------- | ---------------------------------- | ---------------------------------- | --------------- |
| [Geth](https://geth.ethereum.org/)              | Go       | Linux, Windows, macOS | Mainnet, Sepolia, Goerli           | Snap, Full                         | Archive, Pruned |
| [Nethermind](http://nethermind.io/)             | C#, .NET | Linux, Windows, macOS | Mainnet, Sepolia, Goerli, and more | Snap (without serving), Fast, Full | Archive, Pruned |
| [Besu](https://besu.hyperledger.org/en/stable/) | Java     | Linux, Windows, macOS | Mainnet, Sepolia, Goerli, and more | Snap, Fast, Full                   | Archive, Pruned |
| [Erigon](https://github.com/ledgerwatch/erigon) | Go       | Linux, Windows, macOS | Mainnet, Sepolia, Goerli, and more | Full                               | Archive, Pruned |
| [Akula](https://akula.app)                      | Rust     | Linux                 | Mainnet, Sepolia, Goerli           | Full                               | Archive, Pruned |

**Note that OpenEthereum [has been deprecated](https://medium.com/openethereum/gnosis-joins-erigon-formerly-turbo-geth-to-release-next-gen-ethereum-client-c6708dd06dd) and is no longer being maintained.** Use another client implementation!

For more on supported networks, read up on [Ethereum networks](/developers/docs/networks/).

Each client has unique use cases and advantages, so you should choose one based on your own preferences. Diversity allows implementations to be focused on different features and user audiences. You may want to choose a client based on features, support, programming language, or licences.

### Besu {#besu}

Hyperledger Besu is an enterprise-grade Ethereum client for public and permissioned networks. It runs all of the Ethereum Mainnet features, from tracing to GraphQL, has extensive monitoring and is supported by ConsenSys, both in open community channels and through commercial SLAs for enterprises. It is written in Java and is Apache 2.0 licensed.

Besu's extensive [documentation](https://besu.hyperledger.org/en/stable/) will guide you through all details on its features and setups.

### Erigon {#erigon}

Erigon, formerly known as Turbo‐Geth, started as a fork of Go Ethereum oriented toward speed and disk‐space efficiency. Erigon is a completely re-architected implementation of Ethereum, currently written in Go but with implementations in other languages under development, e.g. [Akula](https://medium.com/@vorot93/meet-akula-the-fastest-ethereum-implementation-ever-built-58eaca244c39). Erigon's goal is to provide a faster, more modular, and more optimized implementation of Ethereum. It can perform a full archive node sync using around 2TB of disk space, in under 3 days.

### Go Ethereum {#geth}

Go Ethereum (Geth for short) is one of the original implementations of the Ethereum protocol. Currently, it is the most widespread client with the biggest user base and variety of tooling for users and developers. It is written in Go, fully open source and licensed under the GNU LGPL v3.

Learn more about Geth in its [documentation](https://geth.ethereum.org/docs/).

### Nethermind {#nethermind}

Nethermind is an Ethereum implementation created with the C# .NET tech stack, licensed with LGPL-3.0, running on all major platforms including ARM. It offers great performance with:

- an optimized virtual machine
- state access
- networking and rich features like Prometheus/Grafana dashboards, seq enterprise logging support, JSON RPC tracing, and analytics plugins.

Nethermind also has [detailed documentation](https://docs.nethermind.io), strong dev support, an online community and 24/7 support available for premium users.

## Consensus clients (formerly 'Eth2' clients) {#consensus-clients}

There are multiple consensus clients (previously known as 'Eth2' clients) to support the [consensus upgrades](/upgrades/beacon-chain/). They are running the Beacon Chain and will provide a proof-of-stake consensus mechanism to execution clients after [The Merge](/upgrades/merge/).

[View consensus clients](/upgrades/get-involved/#clients).

| Client                                                        | Language   | Operating systems     | Networks                                                          |
| ------------------------------------------------------------- | ---------- | --------------------- | ----------------------------------------------------------------- |
| [Lighthouse](https://lighthouse.sigmaprime.io/)               | Rust       | Linux, Windows, macOS | Beacon Chain, Goerli, Pyrmont, Sepolia, Ropsten, and more         |
| [Lodestar](https://lodestar.chainsafe.io/)                    | TypeScript | Linux, Windows, macOS | Beacon Chain, Goerli, Sepolia, Ropsten, and more                  |
| [Nimbus](https://nimbus.team/)                                | Nim        | Linux, Windows, macOS | Beacon Chain, Goerli, Sepolia, Ropsten, and more                  |
| [Prysm](https://docs.prylabs.network/docs/getting-started/)   | Go         | Linux, Windows, macOS | Beacon Chain, Gnosis, Goerli, Pyrmont, Sepolia, Ropsten, and more |
| [Teku](https://consensys.net/knowledge-base/ethereum-2/teku/) | Java       | Linux, Windows, macOS | Beacon Chain, Gnosis, Goerli, Sepolia, Ropsten, and more          |

### Lighthouse {#lighthouse}

Lighthouse is a consensus client implementation written in Rust under the Apache-2.0 license. It is maintained by Sigma Prime and has been stable and production-ready since Beacon Chain genesis. It is relied upon by various enterprises, staking pools and individuals. It aims to be secure, performant and interoperable in a wide range of environments, from desktop PCs to sophisticated automated deployments.

Documentation can be found in [Lighthouse Book](https://lighthouse-book.sigmaprime.io/)

### Lodestar {#lodestar}

Lodestar is a production-ready consensus client implementation written in Typescript under the LGPL-3.0 license. It is maintained by ChainSafe Systems and is the newest of the consensus clients for solo-stakers, developers and researchers. Lodestar consists of a beacon node and validator client powered by JavaScript implementations of Ethereum protocols. Lodestar aims to improve Ethereum usability with light clients, expand accessibility to a larger group of developers and further contribute to ecosystem diversity.

More information can be found on our [Lodestar website](https://lodestar.chainsafe.io/)

### Nimbus {#nimbus}

Nimbus is a consensus client implementation written in Nim under the Apache-2.0 license. It is a production-ready client in use by solo-stakers and staking pools. Nimbus is designed for resource efficiency, making it easy to run on resource-restricted devices and enterprise infrastructure with equal ease, without compromising stability or reward performance. A lighter resource footprint means the client has a greater margin of safety when the network is under stress.

Implemented by Trinity. Works like fast sync but also downloads the data needed to execute latest blocks, which allows you to query the chain within the first few minutes from starting.

- Syncs state first and enables you to query RPC in a few minutes.
- Still in development and not fully reliable, background sync is slowed down and RPC responses might fail.

Learn more in [Nimbus docs](https://nimbus.guide/)

### Prysm {#prysm}

Prysm is a full-featured, open source consensus client written in Go under the GPL-3.0 license. It features an optional webapp UI and prioritizes user experience, documentation, and configurability for both stake-at-home and institutional users.

Visit [Prysm docs](https://docs.prylabs.network/docs/getting-started/) to learn more.

### Teku {#teku}

Teku is one of the original Beacon Chain genesis clients. Alongside the usual goals (security, robustness, stability, usability, performance), Teku specifically aims to comply fully with all the various consensus client standards.

Teku offers very flexible deployment options. The beacon node and validator client can be run together as a single process, which is extremely convenient for solo stakers, or nodes can be run separately for sophisticated staking operations. In addition, Teku is fully interoperable with [Web3Signer](https://github.com/ConsenSys/web3signer/) for signing key security and slashing protection.

Teku is written in Java and is Apache 2.0 licensed. It is developed by the Protocols team at ConsenSys that is also responsible for Besu and Web3Signer. Learn more in [Teku docs](https://docs.teku.consensys.net/en/latest/).

## Synchronization modes {#sync-modes}

To follow and verify current data in the network, the Ethereum client needs to sync with the latest network state. This is done by downloading data from peers, cryptographically verifying their integrity, and building a local blockchain database.

Synchronization modes represent different approaches to this process with various trade-offs. Clients also vary in their implementation of sync algorithms. Always refer to the official documentation of your chosen client for specifics on implementation.

### Execution layer sync modes {#execution-layer-sync-modes}

#### Full sync {#full-sync}

Full sync downloads all blocks (including headers, transactions, and receipts) and generates the state of the blockchain incrementally by executing every block from genesis.

- Minimizes trust and offers the highest security by verifying every transaction.
- With an increasing number of transactions, it can take days to weeks to process all transactions.

#### Fast sync {#fast-sync}

Fast sync downloads all blocks (including headers, transactions, and receipts), verifies all headers, downloads the state and verifies it against the headers.

- Relies on the security of the consensus mechanism.
- Synchronization takes only a few hours.

#### Light sync {#light-sync}

Light client mode downloads all block headers, block data, and verifies some randomly. Only syncs tip of the chain from the trusted checkpoint.

- Gets only the latest state while relying on trust in developers and consensus mechanism.
- Client ready to use with current network state in a few minutes.

**NB** Light sync does not yet work with proof-of-stake Ethereum - new versions of light sync should ship soon!

[More on Light clients](https://www.parity.io/blog/what-is-a-light-client/)

#### Snap sync {#snap-sync}

Snap sync is the latest approach to syncing a client, pioneered by the Geth team. Using dynamic snapshots served by peers retrieves all the account and storage data without downloading intermediate trie nodes and then reconstructs the Merkle trie locally.

- Fastest sync strategy, currently default in Ethereum mainnet
- Saves a lot of disk usage and network bandwidth without sacrificing security

[More on snap sync](https://github.com/ethereum/devp2p/blob/master/caps/snap.md)

| Client       | Disk size (fast sync) | Disk size (full archive) |
| ------------ | --------------------- | ------------------------ |
| Geth         | 400GB+                | 6TB+                     |
| OpenEthereum | 280GB+                | 6TB+                     |
| Nethermind   | 500GB+                | 12TB+                    |
| Besu         | 750GB+                | 5TB+                     |
| Erigon       | N/A                   | 1TB+                     |

#### Optimistic sync {#optimistic-sync}

Optimistic sync is a post-merge synchronization strategy designed to be opt-in and backwards compatible, allowing execution nodes to sync via established methods. The execution engine can _optimistically_ import beacon blocks without fully verifying them, find the latest head, and then start syncing the chain with the above methods. Then, after the execution client has caught up, it will inform the consensus client of the validity of the transactions in the Beacon Chain.

[More on optimistic sync](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md)

#### Checkpoint sync {#checkpoint-sync}

Checkpoint sync, also known as weak subjectivity sync, creates a superior user experience for syncing Beacon Node. It's based on assumptions of [weak subjectivity](/developers/docs/consensus-mechanisms/pos/weak-subjectivity/) which enables syncing Beacon Chain from a recent weak subjectivity checkpoint instead of genesis. Checkpoint sync makes the initial sync time significantly faster with similar trust assumptions as syncing from [genesis](/glossary/#genesis-block).

In practice, this means your node connects to a remote service to download recent finalized states and continues verifying data from that point. Third party providing the data is trusted and should be picked carefully.

More on [checkpoint sync](https://notes.ethereum.org/@djrtwo/ws-sync-in-practice)

## Further reading {#further-reading}

There is a lot of information about Ethereum clients on the internet. Here are few resources that might be helpful.

- [Ethereum 101 - Part 2 - Understanding Nodes](https://kauri.io/ethereum-101-part-2-understanding-nodes/48d5098292fd4f11b251d1b1814f0bba/a) _– Wil Barnes, 13 February 2019_
- [Running Ethereum Full Nodes: A Guide for the Barely Motivated](https://medium.com/@JustinMLeroux/running-ethereum-full-nodes-a-guide-for-the-barely-motivated-a8a13e7a0d31) _– Justin Leroux, 7 November 2019_

## Related topics {#related-topics}

- [Blocks](/developers/docs/blocks/)
- [Networks](/developers/docs/networks/)

## Related tutorials {#related-tutorials}

- [Turn your Raspberry Pi 4 into a validator node just by flashing the MicroSD card – Installation guide](/developers/tutorials/run-node-raspberry-pi/) _– Flash your Raspberry Pi 4, plug in an ethernet cable, connect the SSD disk and power up the device to turn the Raspberry Pi 4 into a full Ethereum node running the execution layer (Mainnet) and / or the consensus layer (Beacon Chain / validator)._
