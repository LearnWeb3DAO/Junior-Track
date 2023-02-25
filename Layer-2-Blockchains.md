# What are Layer 2 blockchains?

## Layer 1 vs Layer 2

### Layer 1

A Layer-1 blockchain (also known as the parent chain or root chain) is typically a name used to describe a main blockchain network protocol such as Ethereum or Bitcoin. Layer-1 blockchains are simply the main network that a Layer-2 scaling solution attaches to in order to improve the scalability and transaction throughput of the main chain, or Layer 1. The name Layer 1 comes from its relationship with Layer-2 scaling solutions such as state channels, rollups, and plasma side chains, all of which we will get into in more detail.

Think of Layer 1 as the standard blockchain technology we've been studying so far, and Layer 2 as extensions built on top of that blockchain to add functionality.

<Quiz questionId="c6798b97-2e46-4ed8-838c-3980db60c1a8" />

### Layer 2

Layer 2 refers to a secondary framework or protocol that is built on top of an existing blockchain system. Major cryptocurrency networks such as Ethereum face transaction speed and scaling difficulties. Bitcoin and Ethereum are still not able to process thousands of transactions per second. Additionally, these layer 2 solutions usually offer much better transaction fees.

Layer 2 protocols are specifically designed to integrate with the underlying blockchain to improve the transaction throughput. They rely on the consensus mechanism and security of the main chain. Operations on layer 2 can often be performed independently of layer 1. This is why these are sometimes referred to as "off-chain" solutions. While the main chain / layer 1 can provide the security inherent to a blockchain, layer 2 can provide the speed.

<Quiz questionId="873c0ba7-f9fd-46e0-937d-4c17fb21fa86" />

Since transactions on layer 2 are happening on a different chain, a connection is periodically opened to move these transactions onto the main blockchain. This connection is sometimes called a bridge, or a channel. Therefore, a major consideration for a layer 2 solution is how transactions are validated and confirmed before being moved to the main chain.

<Quiz questionId="5359066e-c857-4bb5-b2cf-ec730044da82" />

## Layer 2 Scaling Solutions

There are a multitude of Layer 2 scaling solutions out there, or being developed, today. We will look at them broadly.

There are two main dimensions where Layer 2 scaling solutions differ from each other. The first is **transaction execution** and the second is **data availability**.

Transaction execution strategies deal with how transactions are run, where they are run, what the trust environments are, what the security and decentralization environments are, etc.

Data availability strategies deal with whether or not the Layer 2 solution makes their transaction data available on the main Layer 1 chain or not.

We will look at the following categories:

1. State Channels
2. Side Chains
3. Rollups
   - Optimistic Rollups (ORs)
   - Zero-Knowledge Rollups (ZKRs)
   - ZKR vs OR
4. Plasma
5. Validiums
6. Volitions

<Quiz questionId="b24a100b-5cde-4cd3-ab16-8e17e60fab54" />

### State Channels

![](https://i.imgur.com/jBbWJoT.png)

State channels were the first widespread scaling approach for blockchains. State channels are used when two or more users want to do a bunch of transactions in a trusted setting without paying gas every single time.

A simple example of this is a tic tac toe game on the blockchain, with betting.

Let's say Alice and Bob want to put 10 ETH down to play a game of tic tac toe. Also, let's say Alice and Bob trust each other. We might design a smart contract such that each move is recorded on the blockchain, and, at the end of the game, either Alice or Bob receive their winnings. But this would be an expensive game, since each move would cost either Alice or Bob gas fees. This can be solved with state channels.

Instead of designing a smart contract to support tic tac toe move by move, we design a smart contract that allows opening and closing a state channel. Once opened, the state channel will have the default start state of the game, and Alice and Bob can makes moves on it without paying gas fees, because the opened state channel will be off-chain. Every move will not be written to the main chain. When the channel is closed, the end state of the game is written to the main chain, along with processing the winnings from the game. So, instead of writing every move to the blockchain, only the start and end of the game is written to the blockchain, thereby saving massive amounts of transaction fees. The number of moves in tic tac toe might be low, but imagine a game of chess, or go, and you'll see how much money this can save.

In applications that involve money/crypto transfers, sometimes these are called **payment channels**.

For payments, another example of a good use of state channels would be getting paid royalties for music. For example, let's say that an artist wants to receive monthly payouts for their songs being listened to on music services, such as Spotify. The artist would like to get paid in ETH for this, but they do not really want to do the work for setting this up. A company could provide such a service to that artist. If the artist received an ETH transfer to their wallet for each and every single song listen, the transfer fees would far outweigh the profits from the listens. Using a state channel, the company could open a channel at the start of the month, collect royalties on behalf of the artist for the month, and then close the channel at the end of the month, sending them a lump sum of ETH for all royalties, only paying a transfer fee once that month. This saves on transfer fees while maintaining security.

State channels still utilize blockchain methodology such as making each state change cryptographically secure.

In addition to potential gains to speed and lower fees, there are some other benefits to state channels. Since the beginning and end of the game is written to the main chain, but in between states are not, it's possible to have more privacy. The state channel was just between Alice and Bob, but since each move was not published to the blockchain, each move is not public.

This approach is not without its challenges as well. The core assumption under which State Channels are useful is that participants of the state channel trust each other to perform these transactions off-chain. Since to open the channel, both Alice and Bob had to put down 10 ETH. That money is locked into the smart contract until the state channel is closed. However, if Alice goes offline on her move, Bob will never get his money back. Therefore, if Bob does not trust Alice, a state channel is not the best approach since either player can lock up the smart contract.

[State Channels on ethereum.org](https://ethereum.org/en/developers/docs/scaling/state-channels)

<Quiz questionId="962d9275-8b3a-4e0e-830a-441ec264b2b8" />

### Side Chains

![](https://i.imgur.com/aAPEmI7.png)

A side chain is an independent EVM-compatible blockchain which runs in parallel to a main blockchain, and has a channel to Layer 1. A side chain has its own validators and consensus method of adding blocks. Side chains accumulate transactions quickly and cheaply and summarize them to the main chain via a bridge or channel.

Since side chains are based on the EVM, you can think of them as mini ethereum blockchains. Side chains come with all the benefits of an EVM, just as writing smart contracts in solidity, and interact with the chain using web3 APIs.

The drawbacks of side chains are that they can be more centralized. For example, if their consensus protocol uses a less secure or less decentralized approach in order to have higher transaction throughput and those nodes collude to commit fraud.

Important to note that, unlike other solutions below, side chains are _technically_ not layer 2 because they do not use the security of the main chain, but are often referred to as such.

#### Further Reading

[Side chains on ethereum.org](https://ethereum.org/en/developers/docs/scaling/sidechains)

<Quiz questionId="f97bf1bf-34bc-42bd-a6d7-ca0756ee4682" />

### Rollups

Rollups are solutions that perform transaction execution on Layer 2 but post transaction data onto Layer 1, in a bundled or summarized form. Think of rollups as a "squash and merge" operation. By moving computation off chain, they free up more space on-chain. Onchain data availability is crucial, since it allows Ethereum to double check the integrity of rollup transactions. Unlike state channels, the transaction data that rollups post on the main chain can be verified to be either correct or incorrect, and rollup execution need not happen in a trustful environment.

Rollups work by deploying a set of smart contracts on Layer 1 that are responsible for deposits, withdraws, and verifying proofs. Proofs are the main distinction between different types of rollups. In general, there are two kinds of rollups: Optimistic Rollups and Zero-Knowledge Rollups.

#### Optimistic Rollups

![](https://i.imgur.com/9YNcWTy.png)

In optimistic rollups, batches of transaction data are posted to the main chain and presumed to be **valid by default** (hence the name optimistic) but can be challenged by other users.

Theoretically, anyone can challenge them by submitting a claim, also known as fraud proof, to prove that a batch committed to the chain contained invalid state transitions. If the fraud proof is valid, these invalid state transitions would be rolled back. If nobody challenges the transaction, it will be committed to the main chain. To give users enough time to challenge transactions, there is a long wait time between a transaction being posted to it being committed on the main chain, typically a few days but as long as a week. During this time, you cannot withdraw your funds to the main chain.

It's important to note that when a challenge happens, the main chain can always verify the authenticity of transactions in the potential block. But these verifications take work and therefore fees.

What's to stop bad actors from spamming a rollup network with bad transactions, or from spamming the network with fraud proof verifications? And where does the money come from for Layer 1 to verify transactions if challenged? To answer this question, we introduce 3 players in this space:

1. `asserter` - the proposer attempting to post a proof of transactions on the main chain, thereby asserting their validity
2. `challenger` - the user trying to prove that the proof posted by the `asserter` is fraudulent
3. `verifier` - a smart contract on the main chain that verifies the proof and checks it's validity

An `asserter` has to provide a bond to propose a block of transactions, usually in the form of some ETH.
A `challenger` also has to provide a bond (usually ETH) to make a challenge.
The `verifier` will verify the transaction(s) on the main chain.

If the `asserter` is found to be fraudulent, they lose some of their bond. The `verifier` gets some of the asserter's bond for processing the verification, and the `challenger` gets another portion of the asserter's bond as a reward for finding the fraud.

If the `asserter` is found to **NOT** be fraudulent, the `challenger` loses some of their bond. The `verifier` gets some of the challenger's bond for processing the verification as before, and this time the `asserter` gets some of the challenger's bond as a reward for their trouble.

#### ZK Rollups

ZK stands for "Zero Knowledge" and it's a method by which one party (the prover) can prove to another party (the verifier) that a given statement is true while the prover avoids conveying any additional information apart from the fact that the statement is indeed true. More on [Zero Knowledge Proofs](https://en.wikipedia.org/wiki/Zero-knowledge_proof).

In this rollup, there are no individuals doing the verification. Instead, everyone who proposes a new set of rolled up transactions to be added to the main chain constructs a zero knowledge proof for it. This can be automatically verified by the smart contract that controls adding transactions to the main chain. Therefore, in contrast to Optimistic Rollups, ZK Rollups do not have the `challenger` role and every proof posted on the main chain is verified at the time of posting.

In particular, the proposer constructs a certain kind of zero knowledge proof, called a [zk-SNARK](https://en.wikipedia.org/wiki/Non-interactive_zero-knowledge_proof), which is a `non-interactive zero knowledge proof`, meaning that this proof requires no interaction between the prover and the verifier.

For a deeper dive in `zk-SNARK`s, refer to David Wong's series, [The Missing Explanation of zk-SNARKS](https://www.cryptologie.net/article/507/the-missing-explanation-of-zk-snarks-part-1/)

#### ZK vs Optimistic

At first glance, ZK rollups seem better in every way than optimistic rollups. After all, transactions can be verified automatically, without the need for challengers, and the asserters prove their own transactions before submitting them. Furthermore they pay gas to process the proof in the smart contract so only they lose if they submit an invalid proof.

So, why have optimistic rollups at all?

The problem with ZK rollups is that it is difficult to construct these proofs, mathematically speaking. Every use case requires research time to find a matching cryptographic proof, which can take a long time to find.

Furthermore, ZK proofs are often complex and therefore expensive to verify. The more operations the smart contract contains, the more expensive it is to run. In addition, smart contracts are also limited in space, so the proof must run in less than a certain number of operations.

<Quiz questionId="7abc77a4-1abb-4413-9fff-366bfa75f55c" />

### Plasma

![](https://i.imgur.com/EWYDWKJ.png)

Plasma is a framework for building scalable, layer 2 applications. Plasma uses a lot of the above ideas in its applications. The building blocks of plasma are off-chain executions, state commitments, and entry/exits to the main chain. A plasma chain is a separate, child blockchain that is anchored to the main Ethereum chain. Plasma chains use various fraud proofs to arbitrate disputes, just like optimistic rollups. Like side chains, plasma chains have their own consensus algorithm and create blocks of transactions. At fixed intervals, a compressed representation of each block is committed to a smart contract on Ethereum. [Merkle trees](https://en.wikipedia.org/wiki/Merkle_tree) enable creation of a limitless stack of these chains that can work to offload bandwidth from the parent chains (including Mainnet). Plasma chains do as much work as possible off-chain. The implementation of Plasma gives the ability of hundreds of side chain transactions to be processed offline with only a single hash of the side chain block being added to the Ethereum blockchain.

Plasma chains only interact with the main chain to commit their state, or to facilitate entry and exit. Since most implementations of plasma are an entirely different blockchain, it must facilitate entering and exiting the chain from the main chain, which is facilitated by smart contracts. Actually, one of the big downsides with plasma networks is that it's more difficult to withdraw assets from it to the main chain. Withdrawals are delayed by several days to allow for challenges, as with optimistic rollups. For fungible assets this can be mitigated by liquidity providers, but there is an associated capital cost. This is because assets on plasma networks are not exactly the same as the assets on the main chain. For example, you do not hold ETH on plasma, you usually hold wETH (wrapped ETH), which has a 1:1 value to ETH.

Plasma implementations also rely on validators to watch the network and ensure security. There is also a waiting period when funds are withdrawn, to allow for challenges.
As with side chain, the benefits of plasma are higher throughput and lower cost per transaction. It's excellent for transactions between two users. However, the downsides of plasma are that it does not support computation as complex as the main chain allows, though many solutions are working on ways to address this.

To use Plasma, you can integrate one of several projects that have implemented Plasma, for example [Polygon](https://polygon.technology), which was formally called Matic.

To dive deeper into Plasma, check out the [Plasma Whitepaper](http://plasma.io/plasma.pdf).

Some other useful docs about plasma are [Learn Plasma](https://www.learnplasma.org/en) and [Plasma Docs on ethereum.org](https://ethereum.org/en/developers/docs/scaling/plasma)

<Quiz questionId="b6a57af1-a718-4716-9c58-db2147e29415" />

### Data-Availability

So far we have been talking about the different approaches to transaction execution taken by various scaling solutions, and the trust and security environments in which those transactions are run.

However, another dimension in which Layer 2's have varying tradeoffs is data availability. On main chains, we are used to having all data being posted publicly on the blockchain. This, however, carries along with it significant privacy issues.

For example, if a professional trading firm was trading massive amounts on money on a DEX, they likely do not want their trading strategies to be made public, otherwise it's not useful to them anymore.

Layer 2 solutions fall in one of these data-availability situations:
![](https://i.imgur.com/TWmjV4H.png)

> NOTE: On-chain and off-chain in this context particularly refers to the MAIN Layer 1 chain. On-chain means data is available on the Layer 1 main chain, and off-chain means it is NOT available on the Layer 1 main chain, though it _may_ be available on the Layer 2 chain.

Both rollup techniques store data on-chain by posting verifiable proofs on the main chain, but ZKR's use validity proofs which are verified upon posting, whereas OR's rely on `challengers` to catch fraudulent proofs.

On the other hand, Plasma chains store most data off the main chain, and only need to touch main-chain usually when there is a dispute. At best, they would post snapshots to the main chain without a proof attached. So if you trust the Plasma chain, then only does the snapshot help. Similar to OR's, the snapshots are considered to be valid by default unless a dispute is raised.

We will now look at Validiums and Volitions.

#### Validium Chains

Validium works very similar to ZK rollups except data is stored off the main chain. Since transaction data is not published on the main chain, this introduces new trust assumptions as users must trust an operator to make data available when it is needed. This is typically achieved through a committee of known entities who stake their business reputation on being reliable data providers. If an L2 node operator stops servicing withdrawal requests, this committee will make its copy of the data publicly available.

It is important to note that Validium is a type of data-availability situation, and does not concern itself with how transactions are executed. Typically you can use a Validium approach with ZKR based transaction execution.

[Validium on ethereum.org](https://ethereum.org/en/developers/docs/scaling/validium)

#### Volition Chains

Volition chains are those which are the birthchild of Rollups and Validium data-availability approaches. Essentially, they allow for a hybrid data-availability scenario where users can decide what they want on the main chain and what they do not want on the main chain.

For example, a large trading firm may not want every single trade it makes to be available publicly on the main chain, but at the end of the week require a proof to be posted on the main-chain to inherit it's security benefits. So they can store data off-chain for each individual transaction, but bring it on-chain for the overall week's profit/loss and balance changes.

Again, similar to Validiums, this is just a data-availability situation, and does not concern itself with how the transactions are executed. Typically, you can use a Volition approach with ZKR's.

<Quiz questionId="192d1af2-5e34-401c-8699-b37c0fbce647" />

### Examples of Layer 2

#### Immutable X

[Immutable X](https://www.immutable.com) is a Layer 2 scaling solution for NFTs on Ethereum. It allows developers to build marketplaces, games, apps, and more. While Ethereum handles only about 15 transactions per second and suffers from high-gas fees, an Layer 2 solution like `Immutable X` handles 9,000 transactions per second with zero gas fees. It leverages Ethereum’s well-developed security, connections, and ecosystem to help developers.

#### Polygon (formerly Matic)

[Polygon](https://polygon.technology) is a plasma-based, EVM-compatible, layer 2 scaling solution that makes use of proof-of-stake, side chains, and more. It is one of the most popular Ethereum L2 solutions out there today. For brief introductions to it, you should read [New to Polygon?](https://docs.polygon.technology/docs/home/new-to-polygon) and [Polygon Architecture](https://docs.polygon.technology/docs/home/architecture/polygon-architecture).

We will have more practical examples of using Polygon in future modules.

#### Arbitrum

[Arbitrum](https://offchainlabs.com/) is an optimistic rollup solution for general purpose smart contracts on Ethereum. It is EVM-compatible, and developers can easily port their existing Solidity code to Arbitrum.

#### Optimism

[Optimism](https://www.optimism.io/) is also an optimistic rollup solution for general purpose smart contracts on Ethereum. It is also EVM-compatible, and develoeprs can port their existing Solidity code to Optimism.

Arbitrum and Optimism differ in some low-level nitty-gritty technical details, but on the surface, they both operate within the same category.

#### zkSync

[zkSync](https://zksync.io/) is a ZK-Rollup solution for Ethereum. Since generalized ZK-Rollups are _extremely_ hard, currently zkSync supports ETH and ERC20 token transfers, NFTs, atomic swaps, and orderbook based DEX's.

and many more... This is not an exhaustive list, and there are many more Layer 2 solutions out there competing for attention and market share, differing in technical details on low-level implementations. You can probably find hundreds if you look online.

<Quiz questionId="51fb4879-2b17-4fe0-a529-5c52a6c6340b" />

### Recommended Watch

[Ethereum Layer 2 Scaling Explained](https://youtu.be/BgCgauWVTs0)

[Rollups Explained](https://youtu.be/7pWxCklcNsU)

[Overview of different Scaling Solutions](https://www.youtube.com/watch?v=9pJjtEeq-N4)

<Quiz questionId="926ee9f0-64ca-4770-af83-595e9840cc1b" />

## The ETH network upgrade and Layer 2's

As we get closer to the ETH network upgrade (previously referred to as Eth2), a common question to ask is "will layer 2's still be relevant?"

The short answer is yes.

The long answer requires us looking at what the ETH upgrade is bringing to the table, and why it benefits Layer 2's.

Currently, ETH operates as a single, distributed 'world computer'. All transactions are executed serially and there is a single shared state that is maintained amongst all Ethereum nodes. This has it's advantages, but the disadvantage is that when a lot of users want to transact parallely, gas fees can spike which can make transactions very expensive. Recently, the demand for Ethereum dApps has led to a relatively high gas fees. You can argue security and decentralization benefits over cost all day, but it is still expensive for an average newcomer to use.

Layer 2 solutions, like rollups, help alleviate the cost by doing transaction execution off the main chain. Instead, they do the computation on the layer 2, and then post a cryptographic proof back to the main chain that can be verified by anyone. Since anyone can check whether or not the proof that was posted is valid or invalid, they can feel comfortable in knowing if the transaction was executed properly or improperly. Due to this property, rollups inherit the security benefits of the main chain without needing to pay gas fees for computation and execution on the main chain.

With the ETH upgrade, ETH is focused on enabling data sharding. Essentially this means that the single state of the network will be broken down into smaller sets of data called shards, and the goal is to reduce storage costs by making storage more accessible to the network. The consequence of doing this is that when rollups post proofs back to the main chain, it will be cheaper for the rollup to do so.

Gas costs on rollup chains are currently limited by gas costs of storage on the main chain. Even though the computation on layer 2's is very cheap, posting proofs to the main chain still requires paying gas on Ethereum to store the proof. If storage costs on Ethereum go down, then cost of using rollup chains also goes down as proofs can be posted for cheaper. This will incentivize more people to use Layer 2 chains, and direct demand of Ethereum data storage will go down, which will further decrease storage costs on Ethereum even more, and so on.

Therefore, the ETH upgrade will actually help secure Layer 2 solutions gain adoption and become even cheaper to use.

---

<Quiz questionId="4f505d9a-4863-4057-a40c-266cbdf85bac" />

## Contributors

<SubmitQuiz />
