# IPFS

![](https://i.imgur.com/VGb7Gkv.png)

IPFS stands for the **InterPlanetary File System**. It is a distributed, peer to peer network, for storing and sharing files, websites, applications, etc. It was originally released in 2015, and is developed by [Protocol Labs](https://protocol.ai/).

To understand what IPFS does and why it is so important, we need a bit of a history lesson around how files are stored and retrieved on the internet today.

<Quiz questionId="54458a01-92b8-4f9a-a893-51b4554a5725" />

## Data Identification and Retrieval

Imagine you wanted to buy a book about Ethereum. You ask your friends, Alice and Bob, who have been in the space for a few years about where you can find a good book about Ethereum.

They give you the following answers:

**Alice:** Go to the small bookstore on University Avenue in Waterloo near the University of Waterloo, go up to the third floor, go to the technology section, and find the 3rd book on the 4th shelf.

**Bob:** Check out _Mastering Ethereum_. It has the ISBN number 9784873118963

If your goal was to get a copy of this book, how confident would you feel you got what you wanted by following each of the instructions here?

> Thanks to [ProtoSchool](https://proto.school) for the inspiration for this example.

## Location vs Content Addressing

There are two approaches to how we can identify data. The first is **location based addressing**, and the second is **content based addressing**.

#### Location Addressing

In the above example, Alice's answer to your question is an example of location based addressing. It points us to the location where the data is stored by a certain entity (physically or digitally). Alice knew that the bookstore had that book at that specific location, and was hoping that they did not take it down or move it to a different location.

Location based addressing is how we identify data in Web2 on the centralized web. For example, I know that in the past the Bitcoin whitepaper was stored at `https://bitcoin.org/bitcoin.pdf`. If I point you to that location, it is based off a belief that they haven't moved that whitepaper elsewhere and continue to serve it at the same location. Though, it could very well host the Ethereum whitepaper on that location even though the authority hosting it is `bitcoin.org` and the file name `bitcoin.pdf` hinted towards something related to Bitcoin.

Ultimately, the contents of a file hosted on the centralized web have no relationship with their location-based address. Given a certain piece of data, there is no way for us to guess the location on the internet where it is stored. We cannot know which domain it is hosted on, who is hosting it, or the filename it is stored as.

<Quiz questionId="19f87ec4-c0f2-4b0e-8a07-42801d19d79c" />

#### Content Addressing

In the above example, Bob's answer to your question is an example of content based addressing. It provides us a unique, content-based identifier for the data, which we can use to fetch the data from a variety of sources. Given the ISBN number, you can find it at a bookstore, on Amazon, as an eBook, or wherever you want.

IPFS is building a decentralized web on content addressed data. Let's dig a bit deeper into what that means.

## Cons of Location Addressing

In the centralized web, since we cannot verify what content lives at what location, we are dependent on central authorities to label and store information as it really is. It is because of this it is quite easy for us to get tricked by bad actors. Have you ever downloaded a video or a game that turned out to be a virus?

Additionally, location addressing means that the same data can be stored at different locations with different filenames thousands or millions of times, which is mostly useless.

Have you ever saved `download.pdf` and `download(1).pdf` on your computer?

Since content does not tie into location directly, the centralized web is a mess of data that is redundantly saved multiple times under different names at different URL's, and there is no way to tell which items are the same.

<Quiz questionId="a5090868-926a-4db0-b5af-80f99625b9dd" />

## Content Addressing - Deeper Dive

On the decentralized web, we can stop relying on central authorities to label information as it is and hope the data we are downloading is not malicious.

In fact, we can all host each other's data, with a different kind of addressing (specifically, content addressing) that is more secure than location based addressing.

### Hashing

The most important piece of tech needed for content addressing is [Cryptographic Hashing](https://en.wikipedia.org/wiki/Cryptographic_hash_function). We will cover a little bit the properties of hash functions, but if you want to dig deeper into how they work and the math behind them, you can read the aforementioned link.

Hashing is a process in which a function, referred to as the **hash function**, takes an arbitrary input and produces a single, fixed-size, "hash" that represents the input uniquely.

We can represent it mathematically as follows:

```
H(a) = b
```

where `H` is the hash function, `a` is the input, and `b` is the output hash.

There are a few important properties that make hash functions super important:

1. For a given hash function, the output size of `b` will always be the same regardless of how small or big the input `a` is.
2. Output hashes are always unique. If the input changed by a single bit, the output hash would completely change. To check if two inputs are the same, you can compare their hashes. If the hashes match, the inputs must match.
   > NOTE: This is how passwords are stored in databases. Web apps which require logging in do not store your passwords directly in the database. They store a hash of the database. When someone tries to login, their password is hashed with the same hash function, and the hash is matched against the one stored in the database.
3. It is not possible to reverse a hash function. This means that given the hashing function `H` and the output hash `b`, you cannot figure out what `a` was. But, if you had `a`, you can check that it outputs `b` quite easily.

It may seem a little unintuitive at first. If output hashes are always a fixed size, but inputs can be arbitrarily large, how can we guarantee uniqueness of outputs? Take this as an exercise into looking into how secure hashing functions are and dig deeper into it.

We will not do a deep dive into hashing for now, but I will say that hashing is a probabilistic guarantee. It is not 100% guaranteed that each hash will be unique, but the probability of finding two separate inputs that output the same hash is EXTREMELY low.

<Quiz questionId="58bda410-a943-419f-a96c-2a757e760704" />

In fact, if you were able to find two inputs that give the same output hash on a modern hash function like SHA-2, congratulations! You have broken the security of that hash function. If you can study those inputs and come up with a generalized formula to do this repeatedly, you get the biggest bug bounty in the world! The entire Bitcoin network is yours :)

<Quiz questionId="df8b04b8-1b37-4498-9c76-180541e5cb37" />
<Quiz questionId="d12a975b-d880-4619-81cf-a29482866d4e" />

---

Getting back to content addressing and hashing.

Since hashes give a unique, fixed size output, regardless of the size of the input data, we can actually use the output hash as a unique identifier for the data itself. Therefore, we have a way to refer to arbitrary data by a unique ID that can be derived from the data itself by passing it through a hash function.

### Trust Implications

As mentioned before, on the centralized web we have come to trust certain central authorities with hosting most of our data. However, malicious actors can sometimes fool us about the contents of a file by spoofing the location.

On the decentralized web, we can remove that trust on central authorities, and everyone can host each other's files. Thanks to content addressing through hashing, nobody can deceive anyone about the contents of a file if we are looking up and identifying files based on their hash. Had a malicious actor changed a single bit in any file, it's hash would have changed, and we are not looking for that file.

### Peer to Peer

But where do we actually get the data from? If you have ever used Torrents, this will sound familiar, but in some sense you can also relate this to Ethereum's approach of data sharing.

Let's say you are looking for a specific file, and you have it's content hash. Instead of asking a single authority to give us that file, we ask the entire network!

![](https://i.imgur.com/MBb01oY.png)

Your request will be shared through nodes you are directly connected to, to other nodes they are connected to, and so on - until you find someone who has the content you are looking for.

You will know it is exactly the file you are looking for because it has a matching hash. Also, if that node were to go offline while serving the request, you may still be able to get a copy of that file from another node which also had that content.

## IPFS

Now that we understand content addressing, let's take another look at IPFS.

IPFS is a decentralized network of nodes for storing and sharing files. Files on IPFS are content addressed, and each file is identified based on it's content hash. This content hash is referred to as a **CID** - a `Content ID`. You will often hear the term "IPFS CID" when working with IPFS.

Anyone can run an IPFS node and upload files to that node. The node will hash those files, and make their CID's publicly known to the rest of the network. If someone else is interested in that content, they can request for the contents of that CID, and eventually the request will reach your node, and you can then serve their request.

Additionally, for important data, anyone can store a copy of certain data on their own IPFS node to improve replication in case some nodes go offline. Therefore, data on IPFS can be highly available through replication.

One last important thing to note however is that data on IPFS is immutable, which means it cannot be updated. Since files are represented by their CIDs, those files cannot be modified after being uploaded. You can upload a new, modified version, but that version would have a different CID than the original due to the uniqueness property of hash functions.

<Quiz questionId="e82a7ea4-d31a-41eb-ac84-9cf156710f96" />
<Quiz questionId="c3be88b0-0a38-4fa4-8bc9-31bc01b43387" />

## IPFS Providers

Running your own IPFS node may seem like a stretch. Similar to how not a lot of dApp builders run their own Ethereum nodes, and instead use nodes from Infura, Alchemy, or Quiknode to build their applications, you can also use node providers for IPFS.

[Pinata](https://pinata.cloud) and [Textile](https://textile.io) are some of the popular IPFS node providers. Similar to Ethereum node providers, you can create an account on these platforms to get access to an IPFS node to store and retrieve data stored on the network.

## IPFS Gateways

IPFS nodes can also optionally expose a public gateway, which allows web2 users to access files stored on IPFS.

The Protocol Labs team, along with other organizations like Cloudflare, run public IPFS Gateways.

The Protocol Labs IPFS Gateway lives at `https://ipfs.io/`.

Here's some IPFS documentation, hosted on IPFS itself. You can access it from the public gateway by visiting this link - [https://ipfs.io/ipfs/QmTkzDwWqPbnAh5YiV5VwcTLnGdwSNsNTn2aDxdXBFca7D/example](https://ipfs.io/ipfs/QmTkzDwWqPbnAh5YiV5VwcTLnGdwSNsNTn2aDxdXBFca7D/example)

It is upto node runners whether or not they want to expose a public gateway. Opening a public gateway does mean users being able to use your node to fetch and retrieve arbitrary data from the network, which can increase traffic and bandwidth costs of running your node, so you can choose not to open it if you wanted to. Providers like Pinata and Textile allow you to choose whether you want to expose a public gateway or not.

<Quiz questionId="842ad9a6-afb3-4f49-93d5-84d08dd4a134" />

## IPFS Use Cases

There are a lot of use cases for IPFS. Since it is a trustless, decentralized, redundant data store - it has a lot of implications. There is no censorship, there is no one in control of the network.

#### Building open borderless knowledge bases

Certain countries, organizations, or authorities ban or censor certain types of data from the centralized web. Thanks to the decentralized web, we can build open and borderless knowledge bases of information that cannot be censored or controlled by any central authorities.

This can be particularly useful for citizens living in highly authoritative nations or states, where access to information is not widely available on the centralized web.

#### Data marketplaces

Projects like Filecoin, which is also from Protocol Labs, extends the IPFS network to include incentives for data storers. At a fraction of the cost of hosting data in centralized cloud providers like AWS, you can pay miners of the Filecoin network to host your data. In exchange for the guarantee of data availability, you pay them in $FIL tokens, and get a decentralized and reliable data storage network for cheaper than centralized solutions.

#### NFT Metadata

Recently, decentralized NFT metadata on IPFS has been trending. In the earlier days of NFTs, NFTs had metadata being hosted on central servers. In fact, even in our Sophomore NFT Collection tutorial, we asked you to build an API endpoint to serve NFT metadata.

That is problematic because the developers of the NFT project can replace that metadata, or their service can go down, essentially messing up the entire project. The term commonly used for this is a "rugpull" - the devs pulled a rug out from the under the users.

After this happened a few times in the space, people took to using IPFS as an alternative for metadata storage, as once metadata is stored on IPFS, anyone can create a copy of the content and data on IPFS cannot really be deleted unless not even a single node no longer has a copy of that content.

#### Host large datasets

Lots of organizations have huge datasets. When I say huge, I mean like petabytes worth of data. Companies which want to build in public can rely on IPFS to decentralize the storage of this data and make data easily accessible around the world.

#### Offline Data Sharing

Since IPFS is a peer to peer protocol, it can be configured to work on a variety of transport protocols. So far we have been assuming that data on IPFS is transferred over the HTTP(S) protocol, which requires an internet connection. However, you can configure IPFS to instead work over Bluetooth, or Radio Signals, or other types of signals. This can be useful in disaster struck areas or low connectivity areas to enable data sharing within a community that lacks internet access.

<Quiz questionId="8eb68da1-736b-4b1a-bb71-d06d7da46922" />

#### Hosting Decentralized Websites

In the beginning of the article, we mentioned that IPFS can host websites. What are websites? They're just a combination of a few files - HTML, CSS, and JavaScript. You can host all of them on IPFS, and end up with a CID. You can access that CID over a public IPFS gateway and be presented to a website that runs completely on the decentralized internet.

[Random Planet Facts](https://gateway.pinata.cloud/ipfs/QmW7S5HRLkP4XtPNyT1vQSjP3eRdtZaVtF6FAPvUfduMjA) is an example website, created by the IPFS team, that runs completely on the IPFS network and is not hosted on a central cloud provider like Google or AWS.

<Quiz questionId="418c1ff1-8da1-4a86-be72-f86eccc2ce31" />

---

There are MANY more examples of IPFS use cases. This is not an exhaustive list by any means.

There are a lot more use case examples listed on Protocol Lab's website, which [you can find here](https://docs.ipfs.io/concepts/usage-ideas-examples/).

## Conclusion

As we shift away from centralized authorities towards decentralization, IPFS is really a fundamental building block of the distributed web. We hope this article inspired you a little bit and helped you understand the motivation behind decentralizing data storage.

As we move forward, we will do a practical example of using IPFS to store NFT Metadata on Ethereum.

<Quiz questionId="c78f8beb-4aab-4e6d-948c-aa0ee3b3d424" />

As always, thank you for reading and feel free to ask any questions on Discord!

<SubmitQuiz />
