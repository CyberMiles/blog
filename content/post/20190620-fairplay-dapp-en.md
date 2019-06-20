---
title: "FairPlay: a new type of DApp"
date: 2019-06-20T15:30:23+08:00
draft: false
tags: ["e-commerce","dapp","smart contract","search engine"] 
categories: ["en"] 
---

*Credits: FairPlay is developed by Second State, an open source enterprise smart contract platform, and deployed on the CyberMiles public blockchain.*

### What is FairPlay

[FairPlay](https://www.fairplaydapp.com/) uses smart contracts to conduct automated prize draws that are fair and transparent. It allows anyone to create and participate in product giveaways and e-commerce marketing campaigns. Since its launch, FairPlay has routinely reached a DAU (Daily Active Users) of 1000 (according to Google Analytics), with each giveaway receiving hundreds of on-chain transactions, making it one of the most popular DApps of all public blockchains.

{{< youtube lQ6dKy_Q42s >}}

The user does not need any special software (ie a crypto wallet) to load the FairPlay web app, and view active and past giveaways. The FairPlay web application is simply a collection of HTML and JavaScript files. Any user can start a web server on her own computer and serve those files locally.

![](/images/20190620-fairplay-dapp-01.png)

When the user needs to make a smart contract transaction, such as creating a new giveaway or participating in an existing giveaway, the web page directs the user to open the CMT Wallet to digitally sign and complete the operation. 

![](/images/20190620-fairplay-dapp-02.png)

*Note: Secure random numbers are crucial for applications like FairPlay. FairPlay is deployed on the CyberMiles public blockchain to take advantage of the Lity Virtual Machine, which supports secure random number generation as a native operation. The Lity Virtual Machine is an open source Ethereum compatible VM developed by Second State.*

From the surface, FairPlay is a DApp in front of blockchain smart contracts. However, under the hood, FairPlay has a modular architecture that is easy to develop and maintain. Key to this architecture is a smart contract search engine. 

### A modular architecture 

Most of today’s DApps rely on a single monolithic smart contract to serve as the “backend”. The smart contract manages all application users and states. Even for systems that consist of multiple contracts, there is typically a registry or manager contract that provides aggregated information about the system.

However, a large smart contract is difficult to write and maintain. It tends to be error-prone, and nearly impossible to fix when an error or issue is discovered, exacerbating the security problems that had plagued DApps today. The registry contract is also constrained by the limitations of today’s smart contract programming languages and virtual machines. It cannot support complex data query operations. 

With the FairPlay DApp, we took a different approach. The DApp consists of many giveaway events, but each event is its own smart contract. When someone creates a new giveaway, she deploys a new instance of the FairPlay smart contract. When an event ends, its smart contract instance is discarded. That allows us to continuously improve the FairPlay contract to add features and fix bugs, as each future giveaway event uses a new smart contract. However, a key challenge in this approach is how the DApp organizes all those smart contracts created by different addresses at different times, and make the information inside all those contracts available in a unified UI. Enter the search engine.

### The smart contract search engine

The FairPlay DApp home screen is the search engine. It allows users to find giveaways containing specific keywords or tags, as well as the user’s previously participated giveaways. The search engine indexes information from all FairPlay contracts deployed on the blockchain.

![](/images/20190620-fairplay-dapp-03.png)

The FairPlay DApp is powered by the open source smart contract search engine developed by Second State. It is decentralized — anyone can create a search engine-based DApp in front of all deployed FairPlay smart contracts. Each search engine-based DApp could use different algorithms and queries to surface and promote FairPlay giveaways tailored to its users and audience. 

![](/images/20190620-fairplay-dapp-04.png)

The search engine-based DApps do not require monolithic smart contracts. Instead, each smart contract in the DApp is designed to complete a limited set of specific business transactions. All related smart contracts are aggregated in the search engine. This pattern is much closer to the design goal of smart contracts. 

### Solving the on-boarding problem

A critical challenge that hinders DApp adoption today is the requirement of crypto wallets. It is nearly impossible to onboard users if they are asked to download and install an unfamiliar mobile app just to try a DApp. Furthermore, many DApps require the user to purchase cryptocurrencies, which often require the user to register for exchanges and purchase Bitcoin using bank transfers first, just to pay for a few cents of “gas fees” to complete a transaction. No wonder that there are very few DApp users.

A wallet account is needed for a DApp user to make transactions against the smart contracts. However, as we have seen from the FairPlay DApp, much of the DApp operations consist of searching, browsing, and viewing information. The user only occasionally needs to make transactions. 

The FairPlay DApp allows users to access the search engine and detailed state information from each smart contract without any wallet account. When the user tries to create and send a transaction against the smart contract, the web app asks the user to open the CMT Wallet to complete the transaction. At the point, the user has the option to download the CMT Wallet mobile app if she does not already have it installed. 

Of course, experienced users could always open the DApp directly from CMT Wallet’s DApp browser. It provides the best user experience as the transactions are signed and sent seamlessly. 

The FairPlay DApp runs on the CyberMiles blockchain, which supports multiple gas fee strategies. For example, the CyberMiles blockchain waives gas fees for casual users and allows contract creators to pay gas fees on behalf of end users. Together with UI optimizations in the CMT wallet, FairPlay users typically would not see gas fee prompts when using the DApp. That dramatically increases the onboarding success rate for transactions. 

### Conclusions

The FairPlay DApp is designed to improve DApp usability through innovative technologies such as the smart contract search engine, easy on-boarding of web applications, and gas free transactions. Get in touch if you have ideas on how to further improve it!

