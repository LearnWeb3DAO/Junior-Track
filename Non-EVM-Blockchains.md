# Looking at some popular non-EVM blockchains

![](https://i.imgur.com/DRLX5t7.png)

## Table of Contents

- Introduction
- Layer 1 vs Layer 2
- NEAR Protocol
- Flow
- Avalanche
- Solana

## Introduction

So far we have looked at Ethereum, and did some reading on Layer 2 solutions. However, there exist multiple other Layer 1 blockchains as well that are worth looking into. In this article, we will talk a bit about a few different popular Layer 1 blockchains.

## Layer 1 vs Layer 2

Layer 1 and Layer 2 are terms used to describe 'types' of blockchains. Layer 1 blockchains are those which are the main blockchain themselves, whereas Layer 2 blockchains are those which are overlaying networks on top of a Layer 1 blockchain.

For example, Ethereum and Bitcoin are Layer 1 blockchains, whereas Polygon, Arbitrum, Optimism, Lightning Network, etc. are examples of Layer 2 blockchains.

Layer 1 blockchains change the rules of the protocol directly in an attempt to scale or provide better usability. They can include, for example, increasing the amount of data included in each block, or accelerating the block confirmation time for each block, to increase overall throughput. Often, this comes with the cost of reduced security and/or decentralization by increasing hardware requirements of running nodes, or increasing the cost to become a validator in Proof-of-Stake systems, etc.

Example of Layer 1 blockchains include Ethereum, NEAR, Flow, Avalanche, Solana, Algorand, Bitcoin, etc.

Layer 2 blockchains refer to networks that operate on top of an underlying Layer 1 blockchain, in an attempt to improve scalability and efficiency while deriving security and decentralization benefits from the main chain. This entails shifting some work of the Layer 1 blockchain to a Layer 2 network, and only using Layer 1 transactions for settlement - e.g. executing 1000 transactions on the layer 2 network, compiling their results together, and posting a proof of validity on the layer 1 blockchain in a single transaction.

Examples of Layer 2 blockchains include Polygon\*, Optimism, Arbitrum, zkSync, Lightning Network, etc.

> `*` Polygon's PoS chain is a loose example of a Layer 2, since it is a Plasma implementation and is essentially a sidechain which also posts summarized data snapshots to the main chain. It does compile and anchor state of Polygon's chain in a compressed form on the Ethereum network periodically. But, since it doesn't provide validity proofs of transactions that were executed, trust is still placed in Polygon Network's validator set. So it doesn't derive the security benefits of Ethereum entirely, but does post state updates to Ethereum periodically, so if Polygon were compromised, it would be recoverable at the latest checkpoint from the Ethereum network.

---

Now, we will look at a few different Layer 1 blockchains and try to compare them to Ethereum.

## NEAR Protocol

![](https://i.imgur.com/QBhOJRz.png)

NEAR is a Layer-1 smart contract platform that utilizes parallel sharding storage and computation technology to scale. It also uses Proof of Stake consensus and has over 100 validators securing millions of dollars on the network.

#### What is Sharding?

The word **shard** means **a small part of a whole**. In the context of blockchains, sharding just means that instead of every single node on the network needing to process every single transaction (like Ethereum as of writing), the blockchain is split up into multiple smaller parts, and each validator only needs to worry about the transactions being executed on the shard it's a part of.

This allows for parallel computation of transactions on different shards. This allows for faster processing of transactions, and hence higher scalability.

<Quiz questionId="a30a266d-08ce-47c4-9c4f-faab1ae6a051" />

---

#### How are shards managed?

A common approach to managing all the individual shards is having a Beacon chain which acts as the 'manager' of all the shards. It is responsible for randomly assigning validators to specific shards, receiving updates from each of the shards individually and taking global snapshots of them, and processing stakes and slashing in the Proof of Stake consensus.

The bottleneck for scalability now becomes the Beacon chain, as that is responsible for the overall global state. However, if each of the nodes in the network becomes 4x faster, including nodes on the Beacon chain, each shard will be able to process 4x more transactions, and also the Beacon chain will be able to manage 4x more shards. Therefore, overall, it would lead to an increase of 16x in total transaction processing. This is called **quadratic sharding**.

<Quiz questionId="5b4aec43-3ab2-416c-90c5-0c28a379b5bb" />

#### What if there is a fork?

While shard chains + Beacon chain model is powerful, it has complexities when we talk about what happens if the chain forks. The Decision for which chain becomes the 'true' chain needs to be made on each shard individually, and the logic for the beacon chain and shard chains is different.

Due to this, NEAR follows a slightly improved model. In NEAR, the system is modeled logically as a single blockchain, where each block contains all the transactions for all the blocks, and changes the state of all shards simultaneously. So if a fork happens, the entire chain effectively ends up choosing a single, logical longest chain. However, physically, no single participant needs to download the full state of the block, and only downloads the state relevant to their shard(s).

#### How do transactions which interact with multiple shards work?

Sharding is quite useless if individual shards cannot communicate with each other, they are not any better than multiple independent blockchains communicating with each other.

Consider the following example:

1. Alice has an account that lives on shard #1
2. Bob has an account that lives on shard #2
3. Alice wants to send money to Bob
4. Validators on shard #1 cannot deposit the money to Bob
5. Validators on shard #2 cannot deduct money from Alice

How would this work?

In NEAR, if a transaction affects more than 1 shard, it is consecutively executed in each shard separately. The entire transaction is first sent to the first shard that gets affected. Once the transaction is executed, a **receipt transaction** is generated, that is passed to the next shard which is affected. If more shards are affected, more receipt transactions are generated and passed further down the line.

Each subsequent shard can verify the receipt transaction and get confirmation that the preceding shards have done their work. It then executes whatever needs to happen on that shard, and either ends the transaction or passes it further if more shards are being affected.

<Quiz questionId="9949dea3-7894-4f7b-994f-3257adf54ccf" />

---

#### Developing on NEAR

Smart contracts for the NEAR Layer 1 blockchain can be written either in **Rust** or **AssemblyScript**. Communication with blockchain nodes can be done over an HTTP API (similar to Ethereum).

![](https://i.imgur.com/iLf0kGq.png)

It is important to note that NEAR also runs an Ethereum Layer 2 blockchain called **Aurora** which uses the native ETH token as the native gas token for the layer 2 as well.

#### Comparison to Ethereum

- For developers, the primary difference is the language used to develop. While you can use Solidity for the Aurora Layer 2 network, the NEAR Protocol is more than just Aurora and doesn't support Solidity for deploying on the native NEAR blockchain.
- The native token is $NEAR, instead of $ETH, for the Layer 1 blockchain.
- Accounts on NEAR have human-readable names, instead of random letters and numbers like on Ethereum. e.g. `learnweb3.near` is a valid NEAR account.
- NEAR allows developers to upgrade their smart contracts. However, they also provide a way to permanently remove this option for a specific contract for enhanced security and truly trustless operation of smart contracts.
- Instead of Etherscan, developers use the **NEAR Explorer** to look at blocks and transactions.
- Instead of running local test nodes through Hardhat or Ganache, developers can use the `nearup` command-line tool to run a local test node.
- Gas (txn fees) on NEAR is controlled algorithmically to try and avoid greater than 50% congestion on individual shards, instead of being based on an auction model like Ethereum.
- NEAR has significantly less validators/miners than Ethereum. This is due to the high requirements to become a validator. As of writing, it costs over 1 million dollars to purchase enough $NEAR and stake them to become a validator on their network.

<Quiz questionId="956f3814-f4dd-4d92-9999-21b1bb1d5961" />

## Flow Blockchain

![](https://i.imgur.com/rohORkF.png)

Flow describes themselves as a "fast, decentralized, and developer-friendly blockchain". They are based on a multirole architecture, and designed to scale **without** sharding.

Flow is based on four foundations that make it unique and worth discussing:

- Multi-role architecture
- Resource-oriented programming
- Upgradeable smart contracts
- Easy on-ramps from fiat to crypto

Let's look at these individually and understand them.

#### Multi-role architecture

In traditional blockchains, each node is responsible for storing the entire state of the blockchain and performing all associated work. This is similar to having a single worker build an entire car.

In the physical world however, no single worker builds an entire car. A common process is **pipelining** - where tasks are handed to different people with different jobs in a sequence.

Flow applies pipelining concepts to the blockchain by separating the jobs of a validator node into four different roles:

1. Collection
2. Consensus
3. Execution
4. Verification

![](https://i.imgur.com/HMzFq8K.png)

Looking at the above figure, we can see that older blocks are further along the pipeline stage. B1 for example is in the execution state, while B2 is verifying B1 before moving onto executing B2. B3 at this time is doing consensus on it's transactions, and will eventually verify B2, and then execute B3, and so on.

Unlike sharding, which takes a horizontal approach to scaling, Flow scales vertically. This means that even though each validator node takes part in validating each transaction, they do so only at the stages of validation. They can therefore specialize for particular tasks at a particular stage of focus. This allows Flow to scale thousands of times more than current traditional blockchains.

<Quiz questionId="ab054c06-9d17-4d65-8bbd-a80a1ba095ff" />

#### Resource-Oriented Programming

Smart Contracts on Flow are developed in a programming language called **Cadence**. Cadence is defined as "the first ergonomic, resource oriented smart contract programming language".

Resource oriented programming is a new paradigm of language design, designed to be secure and easy to use. Cadence is the first high-level resource-oriented programming language. It is statically typed, like Solidity, and provides a very strong guarantee of data storage and data ownership.

Ownership of 'resources', like digital assets, is enabled directly by the programming language, instead of having to manually manage the lifetime of data.

Labelling something as a resource tells the programming language that this data represents something of value, and that all code that interacts with that data needs to follow special rules to maintain the value of the data.

1. Each resource can only exist in one place at any given time. Resources cannot be duplicated or accidentally deleted, through programming errors or through malicious code.
2. Ownership of a resource is defined by where it is stored in the code.
3. Access to methods on a resource is limited only to the owner. For example, only the owner of an NFT can initiate a function that modifies that NFT. This is built into the language directly by placing that NFT's data into the owner's storage, instead of having to create `onlyOwner` modifiers or similar workarounds.

Resource-oriented programming is a large topic to fully dive deep into here. To read and understand more about it, [you can find more information about it here](https://medium.com/dapperlabs/resource-oriented-programming-bee4d69c8f8e).

#### Upgradeable Smart Contracts and Logging

Smart contract platforms make the promise that users do not need to trust the developers, and can instead trust the code. Ethereum was designed so that smart contracts cannot be upgraded, because if code cannot be upgraded, then you do not need to trust the authors anymore after they deploy the smart contract.

Unfortunately, it is very hard to get software to be error-free the first time. [Rekt News](https://rekt.news) has a huge list of smart contracts that got hacked for millions due to tiny bugs in the original design, some of which weren't uncovered even after professional audits.

On Flow, developers can choose to deploy smart contracts to the mainnet in a "beta state". While in this state, code can be incrementally updated by the original authors. Users are warned about the contract being in the beta state when interacting with it, and can choose to wait until the code is finalized. Once the authors are confident that their code is safe, they can release control of the contract and it forever becomes non-upgradeable after that.

#### Consumer Friendly Onboarding

Flow has focused on making things easy to use for the mainstream. When interacting with a dApp on Flow, the Flow transaction format makes strong guarantees about what kinds of changes a transaction can and cannot make, therefore wallets can provide human-readable information to users to help them make informed decisions about what they are approving.

#### Comparison to Ethereum

- Flow uses Proof of Stake consensus
- Flow is a much newer ecosystem, and has a smaller community and audience, but they have a great documentation and developer tools to get started
- Flow was explicitly designed to support games and consumer applications that require high scalability to operate. This is no surprise because Flow is built by Dapper Labs - the team famous for developing CryptoKitties (one of the first NFT projects) which clogged the Ethereum blockchain, and also for developing NBA TopShot - the most popular NFT project which is officially partnered with the NBA. NBA TopShot is built on Flow.
- Execution nodes on Flow have very high staking and hardware requirements. Unlike Ethereum, Flow execution nodes are likely to be a cluster of high-end server hardware in data centers.
- Most of the applications that currently exist on Flow are made by the team behind it directly. Though more apps are currently being developed and the team is aggressively trying to onboard more external developers on the ecosystem.

<Quiz questionId="5d596551-eab5-4db4-b076-ac9f707c4728" />
<Quiz questionId="0794fe89-14b8-45dc-86f0-0642438121e3" />

## Avalanche

![](https://i.imgur.com/agzms5v.png)

Avalanche is an open source platform for building dApps and enterprise platforms in an interoperable ecosystem of blockchains. It is a smart contracts platform, which offers high throughput and near-instant transaction finality. Also, Avalanche supports Solidity, so smart contracts from Ethereum can easily be ported over to Avalanche.

Avalanche is an ecosystem, not a single blockchain. There are multiple chains that can exist on top of the Avalanche ecosystem. The most popular one is the Avalanche C-Chain, which is an EVM compatible chain and supports Solidity smart contracts. However, entirely new blockchain systems can be built in the Avalanche ecosystem with varying properties.

#### Avalanche Design Architecture

**Subnetworks:**
A subnetwork (or subnet), is a set of validators who are working to achieve consensus on a set of blockchains. Each blockchain on Avalanche is validated by one subnet, but each subnet can validate multiple blockchains. A given validator can be part of multiple subnets. If a validator does not care about a particular subnet, they do not need to join it. Also, since subnets can control who enters them, private subnets can be created which are similar to private or permissioned blockchains.

<Quiz questionId="5a32352d-c116-4c2c-aaea-841780ded8f4" />

**Virtual Machine:**
Each blockchain on Avalanche must be a virtual machine. When a new chain is created, it must specify the VM it wants to use - either something that already exists, or a new implementation by the developers. This is how the C-Chain can support the Ethereum Virtual Machine and support Solidity, while other chains on Avalanche do not.

**AVAX Token:**
All blockchains on Avalanche use the native token $AVAX to pay for operations made on the network. AVAX is also available as an ERC-20 on Ethereum, which can be bridged over to the Avalanche C-Chain.

AVAX Token has a fixed hard cap, so there can never be more than 720,000,000 AVAX tokens in existence. The monetary policy of AVAX can change over time in response to changing economic conditions.

#### Developing on Avalanche

Short of creating your own subnet, the main chain that Avalanche is used for is the C-Chain. Deploying to the C-Chain is very similar to deploying to an Ethereum Layer 2. You just need to get an RPC URL for an Avalanche node, and you can deploy your Solidity smart contracts to the C-Chain using tools you are already familiar with - Hardhat, Truffle, Remix, etc. You can also use Metamask as an Avalanche C-Chain wallet.

#### Comparison to Ethereum

- Avalanche has faster transaction finality and is cheaper to use
- Development on the C-Chain and Ethereum are pretty much the exact same
- The AVAX token is hard-capped, unlike ETH which has no hard cap
- Avalanche uses a custom Proof of Stake consensus protocol
- The Avalanche C-Chain memory pool of transactions is not public, and restricted to validators. This can be problematic as block producers have significant upside in making profit off their knowledge of what is going to be included in a transaction next, e.g. by doing arbitrage on DEX trades.
- At the time of writing, it costs upwards of $150,000 USD to stake and become a validator in Avalanche's ecosystem
- Avalanche is less decentralized than Ethereum at the time of writing

## Solana

![](https://i.imgur.com/8D0mVBm.png)

Solana has been talked about a lot recently, and has been making the rounds on news. They have seen some good days and some bad days, but for this section, we will try to go over a bit of what technology they are running under the hood.

Solana is a Proof of Stake blockchain with some additional technology. The most important part of this additional technology is something they call **Proof of History**.

#### Proof of History

Essentially, Solana's hypothesis was that one of the biggest technical challenges in designing blockchains was the lack of a shared clock. It is hard to get a distributed system of nodes to agree on the order of things due to the lack of a shared clock. Blockchains like Ethereum and Bitcoin use block numbers (or block heights) to provide an ordering to transactions, but this has the downside that a transaction doesn't finish execution until the block time has passed (~10 minutes on Bitcoin, ~15 seconds on Ethereum).

Solana takes a drastically different approach with Proof of History. Instead of having blocks and a block time, it requires validators to sign cryptographic proofs which can prove that _some_ time has passed since the last proof they provided. This is done by hashing the output of a previous hash. All data that was hashed into the proof must have existed before the proof was created. The validators then share this with all the other nodes, which can reproduce the new hash, and verify the cryptographic proof fast enough.

Most of the other optimizations are just that, optimizations to smaller aspects of the blockchain to make it work faster.

#### Leader Selection

Solana has deterministic leader selection. This means that when a transaction is created, the network already knows who the next block creator is going to be, and can just forward the transaction data to that leader directly. Unlike Bitcoin and Ethereum, there is no 'race' or 'puzzle' to solve in competition against other miners. This has it's downsides too, as deterministic leader selection implies that since leaders are known ahead of time they can be subject to DDoS attacks.

<Quiz questionId="b665a8e7-ed53-4e80-aa3a-6652de11e87e" />

#### The Compromise

Solana is really fast. Period. But what does it compromise on to achieve this speed? Apart from the downsides of deterministic leader selection, the remaining tradeoffs land up on dApp developers.

Most of the complexity Solana requires to enable fast transactions is handed down to dApp developers to deal with. If you've ever looked at Solana's documentation about how to write programs in Rust, it looks much more intimidating than looking at Solidity. The system is much more complicated and difficult to deal with, and a lot needs to be done by the end developers.

The tradeoff may be fine. The chain is really fast, but also really hard to build complicated applications on. Over time, the community will likely create abstractions and hide much of the complexity, but if you want to understand the fundamentals so you can solve weird issues when they come up, then you need to know that it is not going to be easy. The complexity is also likely the reason Solana doesn't see as many scams, not because code on Solana doesn't have vulnerabilities, but because very few devs in the world can write Solana programs let alone reverse engineer and find vulnerabilities in them.

#### Stateless Blockchain

The other biggest difference between Solana and Ethereum is that Solana smart contracts are stateless, whereas Ethereum smart contracts are stateful. By this point, you have gotten used to storing variables, arrays, mappings, etc. in Solidity code. Solana does not allow that. Contracts in Solana cannot store anything by themselves.

There is separation between code and data on Solana, and has a wide variety of implications on the system's design overall. Any data on Solana is stored in something called **accounts**. This terminology is the biggest source of confusion when coming from Ethereum, because accounts on Solana do not refer to wallets or addresses etc. Honestly, the terminology is a bit weird here.

Accounts are data stores, and accounts can be created for a pre-defined fixed size of data. To store data, rent needs to be paid for the account. Accounts can store $SOL tokens, which gets deducted over time as rent for the storage space it is using on the network.

<Quiz questionId="1d299f7b-56b6-4938-848a-562a2757831b" />

#### Transactions on Solana

On Solana, the basic unit of operations is called an **instruction**. An instruction is a single call into a program (or smart contract). One or more instructions can be bundled together serially into a **message**. A message is signed by a sender to form a **transaction**.

This is another difference from Ethereum. In Ethereum, a transaction refers to a signed call into a smart contract, and the smart contract can do whatever it wants (including calling other smart contracts). This is not possible on Solana. Again, the complexity is handed down to the developers and users. Users need to construct transactions which are a set of instructions into multiple programs, and either they all succeed or they all fail. Therefore, the developer building the dApp needs to write that code on the frontend side, instead of just having a single entry point in a program that does everything inside the program.

When passing multiple instructions in a transaction, the order matters, as that is the order they will be executed in. The dApp developers need to take special care in making sure they don't mess up the order. There is risk on the frontend side of things of creating incorrect transactions.

---

<Quiz questionId="a66cd41d-4096-489c-9408-c5bf2d6f8d87" />

Overall, Solana has it's own set of pros and cons. It's very fast, and definitely has made some good technical advancements. The cons are the developer experience is impacted, and is much harder to develop dApps on Solana that it is on Ethereum (or any EVM compatible chain).

<Quiz questionId="6a43d499-8067-4630-9240-8ba372163a71" />

## Conclusion

I hope through this article you were able to get some insight into different Layer 1's and different approaches they are taking to scale. This document should also serve as proof for how we are still quite early in the market, and likely not all of these chains will continue to gain popularity and be widely used over time as clear winners emerge.

<Quiz questionId="fda134ae-db06-42dc-8dc6-d14d04da9d4f" />

As always, feel free to post any questions on the Discord. All the best for the skill test!

<SubmitQuiz />
