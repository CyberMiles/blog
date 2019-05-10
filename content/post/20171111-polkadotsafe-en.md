---
title: "I accidentally killed it (and evaporated $300 million)"
date: 2017-11-11T10:01:23+08:00
draft: false
tags: ["hacks","smart contract","Ethereum","Wallet","blockchain"]
categories: ["en"]
---

Github user devops199 said on Twitter “I accidentally killed it”. Before anyone can blink an eye, his “accident” has “killed” Ethereum accounts holding over [$300M](https://www.vice.com/en_us/article/ywbqmg/parity-multi-signature-wallet-vulnerability-300-million-hard-fork) in crypto assets. On average, each of his 4 words cost $75M.

![](/images/20171111-polkadotsafe-01.png)

One of those Ethereum accounts belongs to the Polkadot project, and it held close to $90M before it was “killed”.

![](/images/20171111-polkadotsafe-02.png)

Someone asked devops199 after the incident: Why did you do it? His response? “I am just a beginner poking around the system!” In another word, he did not know what he was doing. A newbie experiment cost people $300M.

![](/images/20171111-polkadotsafe-03.png)

The fact that this is even possible shows us the sad and amateurish state of Ethereum Smart Contract development. It desperately need help. Ironically, the old-hand enterprise developers know how to do this the right way — if only the “cool kids” and “programming bros” can listen.

### How is it possible to make money disappear?

In the world of cryptocurrencies, you can have one or more “wallets” to hold your money in the distributed ledger known as the blockchain. Traditionally, each wallet is simply a pair of public and private keys. Those keys are created in pairs. You can create a pair at any time using software on your own computer.

The public key is like the wallet’s account number. You can display and advertise it to the world. People can send cryptocurrencies into your wallet through its public key. For example, here is the public key for one of my Ethereum wallets. You are welcome to send some ETHs or other ERC20 tokens to this address if you enjoyed this article!

0xD69b30FAdf81882253329Ab0149131c67602eEd1

However, in order to spend the money in the wallet (ie to send some to another wallet), you must know the private key and use this private key to digitally sign any spending request. But,

**Question**: what happens if the wallet owner forgets or lost her private key?

**Answer**: In that case, no one can spend or touch the fund in that wallet. The money has simply evaporated (or lost, or frozen, depending on your favorite metaphor).

> Lost coins only make everyone else’s coins worth slightly more. Think of it as a donation to everyone. — Satoshi Nakamoto

### Multi-signature wallets

The public / private key wallets are useful for individuals, but very hard to use for companies or projects. The private key should never be shared between multiple people. It should only be known by one person — the private key is just a piece of string data. If it is shared once, it can be shared many times without detection. However, it is also dangerous to have one person control company wallets that contain millions of dollars worth of cryptocurrencies. Besides the obvious risk of embezzlement, there is a risk for physical violence (i.e. abduction) against the private key holder.

A clever solution to this problem is to leverage Ethereum support for accounts based on Smart Contracts. In this case, the Ethereum wallet is not directly associated with a public / private key pair. Instead, it’s access is controlled by a piece of software residing on the blockchain, known as a Smart Contract. The Smart Contract could require multiple private keys at the same time before it can execute commands to access funds in the wallet.

So far so good! It is obvious that many companies and projects have great needs for multi-signature wallets. Parity developed this [Smart Contract](https://etherscan.io/address/0x863df6bfa4469f3ead0be8f9f2aae51c91a907b4#code) and puts it on the Ethereum public blockchain for anyone to use as a library function. In another word, you can also easily write your own multi-signature wallet Smart Contract by simply having your Smart Contract calling key functions in the Parity Smart Contract. Parity itself is using this Smart Contract in this fashion in its own high profile ICO projects such as the Polkadot.

### Features become bugs

However, as we recall, Parity wrote its multi-signature Smart Contract as a regular standalone Smart Contract. It was not intended nor declared as a library function. Because of that, it has two “features” that should never exist on a library function.

First, it allows for an “owner” of itself. A properly declared library function should not have an owner as it is used by many. To make matter worse, the function to set owner is not protected and can be called by essentially anyone on the Internet.

Second, as the case for standalone Smart Contracts, the owner can delete the Smart Contract it owns via the kill function.

Now, we know that devops199 poked around random Smart Contracts on the Ethereum public blockchain. He just called the public functions to set himself as owner and then killed the contract to see what happens. He stumbled on the Parity Smart Contract, killed it, and now rendered all Smart Contracts wallets that relied on the Parity Smart Contract inoperable. For now, there is no way to access the funds in those accounts since the Smart Contracts can no longer verify the multi-signature requests without the Parity library functions devops199 killed.

### A new hope

Now, isn’t all software system created by humans? Why can’t we just rollback the change or reinstall the Parity Smart Contract as a library to solve this problem? Well, that is precisely the kind of operations the blockchain is designed to prevent! In a decentralized world, there is no “we” to make this change.

> A common used analogy in the real world is a bank hack. Say, someone hacked a bank and stole money by making electronic transfers to himself. The bank must ask the police to investigate and convict this person in order to get their funds back. The bank cannot just “revert the database change” simply because it is feasible to do so.

A central idea behind the blockchain Smart Contracts is “code is law”. It takes the fragile and unreliable human out of the decision-making process. What’s written in the software code is faithfully and irreversibly executed by the blockchain. One person’s bug is another person’s feature. In a decentralized world, without a central authority, who to judge what is a bug and what is a feature. The software code as they are written is the only objective representation of any agreement.

Having said that, there are two potential remedies for the Parity bug (we will call it a bug since that’s what most people agree on at this time). Neither of them is ideal.

First, there is a year-old issue on Ethereum development roadmap called [EIP 156: Reclaiming of ether in common classes of stuck accounts](https://github.com/ethereum/EIPs/issues/156). Since the Parity bug causes the fund to stuck in accounts, this appears to offer some hope. However, a close reading of the proposed specification shows that EIP is intended to address simpler stuck account problems. But perhaps we can expand the scope of this EIP to resolve the Parity bug as well? There is currently a very active debate in the comments section of this issue on Github. The arguments range from political to technical. Check it out if you are interested.

Second, the majority of the community can always create a hard fork to recover the funds for the frozen accounts. This is not the first high profile hack on the Ethereum blockchain. In 2016, Ethereum suffered from the infamous [DAO hack](https://medium.com/@pullnews/understanding-the-dao-hack-for-journalists-2312dd43e993) and lost tens of millions of dollars worth of ETH. That incident ended with a hard fork of the Ethereum blockchain, which allows the majority of the community to come to an agreement to revert the ledger and recover the fund. However, today the Ethereum network has grown much bigger. The consensus and technical risks of a hard fork have all increased significantly. It is highly doubtful whether the community will support another hard fork when similar software bugs will for sure occur many times in the future.

### What do we learn here?

Parity is one of the best-known software developers in the Ethereum ecosystem. It creates core infrastructure software and tools used by many in the community. It also implements blockchain pilot projects for banks. To see such obvious bugs from Parity is very disappointing.

The Smart Contract library that caused this problem is open source. Linus Torvalds once said: “given enough eyeballs, all bugs are shallow”. Open source software achieves security through transparency. But the hundreds of organizations that relied in the Parity Smart Contract library to handle critical financial assets have apparently never audited the code. That is a huge failure for the community as a whole.

However, an even deeper problem is why Ethereum is plagued by bugs and hacks? The Turning complete Ethereum Virtual Machine to execute Smart Contracts sounded great, but it is very hard to program. Without high level tools and frameworks, developers often compare writing Ethereum Smart Contracts to writing assembly language code. As years of experiences in enterprise computing have taught us, secure and high quality software is produced by sophisticated tools, frameworks, and reusable software components. Enterprise stack of frameworks and tools are “bloated” for very good reasons! In contrast, the current Ethereum virtual machine and its software stack appear to be very amateurish.

[A code quality review](https://vessenes.com/ethereum-contracts-are-going-to-be-candy-for-hackers/) on deployed Ethereum smart contracts reveals 100 “serious” bugs per 1000 lines of code. In contrast, the Microsoft’s released codebase contains only 0.5 serious bugs per 1000 lines of code, and the NASA code contains 0 obvious bugs. The code quality for Ethereum smart contracts us very low.

I believe that blockchain technology can only be adopted by enterprises after we solve the problems of poor developer productivity and software quality. As a community, we must innovate on making Smart Contracts easier to create and making it easier to write high quality / secure code. Our project, [CyberMiles](https://www.cybermiles.io/en-us/), aims to solve this problem by bringing the full stack of enterprise software frameworks to blockchain nodes and have a smart contract language heavily optimized for e-commerce applications. We believe Ethereum’s one-size-fit-all is not the future of public blockchain networks. I see a future of several specialized blockchains, each optimized for a special type of smart contracts (i.e., CyberMiles aims for e-commerce optimization), and those specialized block chains will form an Internet of blockchains through Cosmos or ironically even Polkadot.
