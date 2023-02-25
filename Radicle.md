# Radicle - a decentralized alternative to GitHub

![](https://i.imgur.com/0tcXnVK.png)

> NOTE: When we say `Git`, we do not mean `GitHub`. GitHub is a centralized platform, and Git is a protocol. You can use Git and not use GitHub, instead storing your code on platforms such as GitLab or BitBucket.

<Quiz questionId="7f8eae58-a790-42c0-a5f7-0bb13600149d" />

---

Collaboration on code these days mostly happens through GitHub. There are alternatives like GitLab and BitBucket, but GitHub is by far the most used Git platform.

However, using GitHub is not a free lunch. GitHub is owned by Microsoft, and with centralization comes tradeoffs. Since you're hosting your code and content on GitHub's platform, that means they could censor you if they wanted to. Let's look at an example:

## youtube-dl

youtube-dl is a free and open source download manager for video and audio from YouTube, and over 1,000 other websites. It is one of the most starred projects on GitHub, with over 100,000 stars.

In October 2020, GitHub took down the youtube-dl repository, along with various other public forks of the project, based on a request from the Recording Industry Association of America (RIAA).

This spurred a lot of controversy, and highlighted how open-source software and code could be taken down due to it being hosted on a centralized platform. Even though GitHub publicly reinstated that repository in November 2020 due to public backlash, it was still an indicator of how much power they have over your code.

## Banned Accounts

Since GitHub is a US corporation, they have to play according to the US government. Being a centralized platform means GitHub can ban whoever they want from the platform and prevent them from participating in open source through the world's largest open source platform.

Due to pressure from the US government, GitHub has currently banned all Iranian, Syrian, and Crimean accounts. This is heavily misaligned with the values and mission of building a free and open community.

---

In the spirit of decentralization and removing control from intermediaries, Radicle emerged as a decentralized code collaboration tool. It provides similar functionality to Git, without the centralized platform risk.

## Why Radicle?

> Open source runs the world

You may have heard this before. Free and public code has made it significantly easier and cheaper to build software, and innovation in the industry is growing exponentially due to it.

Code collaboration platforms like GitHub have undoubtedly played a huge hand, and have changed forever the way developers write and maintain software. However, they are centralized, and everything you do on these platforms are locked in and exist solely on those platforms.

As an alternative set out to achieve the true goals, Radicle is built on the following principles:

1. Radicle must prioritize user freedom. This means users have the freedom to run, copy, distribute, study, change, and improve the software.
2. Radicle must be accessible and uncensorable.
3. Radicle must be user-friendly.
4. Radicle must be offline-first, and does not require internet connectivity to function.
5. Radicle must not compromise on security, and secure every aspect of the system with cryptographic signatures to ensure security in a decentralized system.

<Quiz questionId="3e49ab44-8f52-44ac-83a8-0627feb265ce" />

## Radicle Link

The Radicle Network is built on a peer-to-peer protocol called Radicle Link. Radicle Link extends the Git protocol, and adds gossip messaging to find peers interested in the same data in a decentralized way.

The gossip protocol is a decentralized communication protocol, where all nodes randomly talk to other nodes and request information or share information they want to, and by forwarding knowledge to other nodes, the 'gossip' eventually makes way to nodes who want what you have, or have what you want.

![](https://i.imgur.com/Il5mDZe.png)

<Quiz questionId="b2aac702-45c9-499a-9886-71682d441b2c" />

Very similar to how IPFS shares data, participants in the Radicle Network share and spread data they are interested in by keeping a local copy and sharing it. Since it is an extension of the Git protocol, it maintains the efficiency of Git's data transfer protocol through the peer to peer network.

In Radicle, repositories are called **projects**, which are replicated and shared by **peers**. If you've ever used torrents, the word 'peers' may sound familiar. Essentially they are people or organizations either looking for data they are interested in, or making data they already have publicly available for other peers.

<Quiz questionId="1bb39d2a-152a-4a61-815b-f6d3a037d2f3" />
<Quiz questionId="3b3e8f56-4af0-4564-bc1c-b66ad09cf89c" />

If you want to go even deeper into how exactly Radicle Link works, [you can find a more in-depth documentation on the specification here](https://docs.radicle.xyz/understanding-radicle/how-it-works)

## Radicle vs GitHub

While they are built to solve the same problems, albeit with different approaches, the way to use Radicle is somewhat different than using GitHub.

1. Radicle is completely open source and built entirely on open protocols, there is no centralized aspect anywhere.
2. Radicle relies on peer-to-peer communication instead of a client-server architecture like GitHub
3. Since it relies on peer-to-peer communication, and it is unreasonable to expect a single peer to download every single repository/project that exists, Radicle is **not** global by default. Instead, peers can track other peers and projects and that determines what content they can see and interact with.
4. Radicle is a community-owned network, not a company. Governance of the network happens through a DAO by ownership of the $RAD token that lives on Ethereum.

<Quiz questionId="1f804acb-20d9-452f-8b73-8ab4c07f05b5" />

## Using Radicle

Unlike Git, Radicle projects do not have a single canonical view (i.e. a master/main branch). Instead, Radicle projects have multiple **upstreams**, i.e. different forks of the code, maintained by the code maintainers and contributors.

To fetch and receive changes from contributors, you have to add them as **remotes** to your project. This automatically tracks them, and you can subscribe to the new code updates they make on their upstream.

To actually get started with using Radicle, the primary way is to download the **Radicle Upstream** desktop client. It is an open source client which acts as your gateway to the Radicle Network.

> Unfortunately, the Radicle Upstream Desktop Client currently does not ship for Windows. It is only available for Linux and macOS.

<Quiz questionId="2aab2417-9f06-46ce-b65f-dc2b699f2cc6" />

#### Creating Projects

The basic workflow for creating projects on Radicle is quite similar to how GitHub would work:

1. Create a new repository/Import an existing repository
   1.1 Your project will be assigned a unique **Radicle ID**
2. Share your Radicle ID with others to let them view your repository
3. Make changes, commit code, and push changes using the typical `git push` command (but you're pushing to Radicle now, not GitHub)

#### Viewing Open-Source Projects

To look at open source projects hosted on Radicle, you need access to the project's Radicle ID.

Using the Desktop client, you can search for the Radicle ID. This will send out an information request message on the Gossip protocol, and once you find a peer who can share that information with you, you will be able to view the project.

#### Contributing to Open-Source Projects

To contribute to an open source project, you can fork a project and make changes to it as you normally would.

Then, the original repository can track your changes if they add your fork as a remote of the repository.

That will let them automatically track your changes and generate patches which can be merged into the original repo from your fork automatically.

<Quiz questionId="44b3c19f-e729-4a39-a5d4-f146a01336c2" />

## Radicle ❤️ Ethereum

Radicle has an optional integration with Ethereum. Users can opt-in to the integration which would allow them to have unique global names for your profiles and organizations by using ENS, own decentralized organizations on Radicle by having members be linked to an Ethereum wallet, and be able to accept contributions in crypto for your open source projects.

Additionally, Radicle also has the $RAD token on Ethereum, which is the governance token for the Radicle DAO. This token provides voting power in the DAO which controls the Radicle Network.

<Quiz questionId="80050ea9-2bba-4de0-bd6a-8f8178a2d858"/>
<Quiz questionId="2124e7ec-3185-4f19-ae1e-18757a0f178c" />

## Coming Up

Hopefully this article gives a conceptual idea of what Radicle is, how it is different from GitHub, why it is important, and how it works.

In the coming level, we will do a practical introduction of Radicle and go through the process of actually setting up repositories on Radicle, and sharing them with others.

> NOTE: The practical Radicle level has been slightly delayed as we wait for the Radicle team to ship a cross-platform CLI client which would also work on Windows. If you are, however, a Linux or macOS user, we highly suggest you download the Radicle Upstream client and try to create a project on Radicle.

<SubmitQuiz />
