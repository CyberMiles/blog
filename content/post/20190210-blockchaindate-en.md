---
title: "The next wave in blockchain data"
date: 2019-02-10T10:01:23+08:00
draft: false
tags: ["blockchain","ecommerce","data"]
categories: ["en"]
---


Last year we provided an in-depth look into blockchain storage, also known as global state. In that [article](https://medium.com/cybermiles/diving-into-ethereums-world-state-c893102030ed) I compared the two vastly different types of data in an Ethereum-compatible blockchain: permanent data and ephemeral data.


Today let’s explore the use of the permanent data, more specifically how we currently can turn raw event log data into rich information, suitable for DApps. Allow me also to detail some design characteristics of off-chain data storage projects and, in turn, point out the enormous opportunities that are on the horizon.


![](/images/20190210-blockchaindate-01.jpeg)


It is a well-known fact that it’s not practical to store entire blockchains on everyday mobile devices or even on modest home PCs (where DApps are primarily run).


It’s also understood that a blockchain’s global state is a scarce-and-precious resource, and that writing to (and reading from) a blockchain’s global state is very costly. To that end, the blockchain is designed with incentives for users to remove their smart contracts and associated stored data. This is achieved by refunding gas to those users who delete their previously deployed smart contracts, and associated data, all together.


Wait a second, you may be asking. Aren’t blockchains and smart contracts supposed to be immutable and auditable?


Well, they are. This is where the event logs come in.


Below I’ll cover blockchain event logs in great detail. But first, here is a quick refresher on the topic of blockchain storage.


>There are two vastly different types of data in Ethereum: permanent data and ephemeral data. An example of permanent data would be a transaction. Once a transaction has been fully confirmed, it is recorded in the transaction trie; it is never altered. An example of ephemeral data would be the balance of a particular Ethereum account address. The balance of an account address is stored in the state trie and is altered whenever transactions against that particular account occur. Another example of ephemeral data would be a smart contract’s variables, which are stored in the storage trie and altered by functions inside the smart contract. It makes sense that a) permanent data, like mined transactions, and b) ephemeral data, like account balances or contract variables, should be stored separately.


In addition, let me offer a refresher on why we need additional applications, over and above the blockchain’s base layer protocol.


>You may recall, from the previous article, that Bitcoin UTXOs are blind to blockchain data, ergo the Bitcoin blockchain does not actually store a user’s account balance. “The concept of a balance is created by the wallet application. The wallet calculates the user’s balance by scanning the blockchain and aggregating the value of any UTXO the wallet can spend with the keys it controls” (Antonopoulos, 2017). In a similar fashion, and in light of the costs associated with storage and processing on the Ethereum blockchain, it makes sense that we might utilize applications, which operate over and above the base layer protocol, to perform a myriad of tasks that can help users interact with smart contracts on the blockchain.


**Please note: From this point forward, all of the information in this article is in the context of Ethereum-compatible blockchains only.**


---


## Event logs

A smart contract can write permanent data to the blockchain by declaring and then emitting an event. Emitted event log data will remain intact indefinitely, even if that particular smart contract and its global state have been completely removed using the previously mentioned opcode “0xff” known as “selfdestruct.”


The cost of writing to event logs is comparatively cheaper than writing to the blockchain’s global state. For example, it costs around 40 thousand gas to write a single address and a single uint to the blockchain’s state. Alternatively, it only costs around one thousand gas to write that same single address and uint to the blockchain’s event logs.


The following section demonstrates how a smart contract developer could declare and then emit events as part of their smart contract’s day-to-day operation.


---


## Declaring events

The following example is using [the Lity smart contract programming language](https://github.com/CyberMiles/lity). (This is just a snippet, you will see a more holistic example of this code later on.)


{{< gist tpmccallum 428df064400c60e342c17c68c3bd05eb621111d7 >}}


As you can see, to declare the event, we simply type the word “event” followed by the name of the event. Then we pass in some data types and data names (in this case, the data types of “address” and “safeuint,” which relate to the data names “endUser” and “amount” respectively).


You will notice that we have deliberately specified, in this declaration, that the “endUser” data be **indexed**. Essentially indexing a parameter allows for efficient searching later on. Up to three parameters per event declaration can be indexed.


---


## Choosing what to log

It is wise to index data, such as account addresses, because it’s likely that you will be searching for information based on a particular account address. It is not a great idea to index other types of data, such as arbitrary amounts (i.e. integers such as 1 or 10). It is completely unnecessary to include (in your event logs) any information that can be easily retrieved using pre-defined global variables or functions (ex. “block.number”). Variables like block.number are included in standard transaction receipts by default.


### Emitting events
Let us now emit the event declared above.


{{< gist tpmccallum d0a69878b0ba0afb40a861f769eaf5ce >}}

As you can see from the above code, to emit an event, you simply type “emit” followed by the name of the event (which was declared in the previous code snippet). The data to be included in the event log is passed in during a function’s execution. The order in which data is passed into the emit command must match the order of the data, as seen in the event declaration.


The following code shows the entire smart contract, thereby providing context for the above snippets.


{{< gist tpmccallum 209a9718c51ca05ff9e37080d0479dca >}}


As previously mentioned, the above example code employs the [Lity](https://github.com/CyberMiles/lity) smart contract programming language. Lity is an open-source language enhanced from Solidity, and so the Lity and [Solidity](https://github.com/ethereum/solidity) syntax is very similar.


Further to the above code example, the following video provides an in-depth look at declaring, emitting and fetching event logs. Please note that the event logs in the following video are being declared and emitted using [the Vyper programming language](https://github.com/ethereum/vyper), something which you may find this interesting.


**Note**: The results on-chain are identical regardless of which programming language the smart contracts were written in.


{{< youtube JNXl0KCcqhA >}}


---


## Developing your own smart contracts on the CyberMiles blockchain


### CyberMiles online smart contract editor

CyberMiles offers a complete development and testing environment where you can write, deploy and interact with your own smart contracts. The above Lity smart contract was written and deployed using [the CyberMiles online smart contract editor](http://europa.cybermiles.io/).


![](/images/20190210-blockchaindate-02.png)


In the above contract code, the function called “addPoints” increments the point balance for a given address. Let’s go ahead and add some points against the account address (0xca35…733c), which deployed the contract.


The contract’s addPoints function can be executed using the following syntax in the terminal of your full node.


{{< gist tpmccallum aee86cba919a873f7b3db73edb124db3 >}}


Or executed via the CyberMiles online editor.


![](/images/20190210-blockchaindate-03.png)


The overall point balance for a particular user can be obtained via the EventLogCreator.getPoints() function. After many “addPoints” function calls our tally of points for the account (0xca…733c) (at block height 1, 150, 044) is **882**.

The getPoints function can be performed with the following syntax.


{{< gist tpmccallum bd4dbb9cba1334b62d50f8421527ab90 >}}


Or via the CyberMiles online editor.


![](/images/20190210-blockchaindate-04.png)


Now, knowing that account (0xca…733c) has a grand total of 882 points is great, but this is only part of the story. Analytics are such a big part of all e-commerce ecosystems and, given the future opportunities for predictive analytics, wouldn’t it be nice to have richer information?


Consider the following richer questions:


* At what rate were the points accumulated? In other words, how many points did the account (0xca…733c) accumulate per day, or per minute?

* Is the account (0xca…733c) demonstrating sustainable growth in points?

* Did the account’s points peak and then drop off, never to regain momentum?

* What percentage of points does the account (0xca…733c) have when compared to another account (0x1472…160C)?

* Did these two accounts take turns in the lead or has one account been dominant during the entire process?

The amount of questions that can be asked are only limited by the imagination. Thankfully, the CyberMiles blockchain ecosystem provides you with the flexibility to write your own customized e-commerce applications (smart contracts and DApps), using a myriad of data types as well as mechanisms to store, log and retrieve data. There are enormous opportunities for new innovative businesses to take part in the new era of e-commerce: a decentralized global network of buyers and sellers.


The previous section covered generating event log data. The following section explores the harvesting of event log data.


---

## Harvesting event log data

Here are a couple of videos that demonstrate how event logs can be harvested. This first video demonstrates how easily smart contracts can be loaded into a custom harvester. We are using an Elasticsearch instance here.


{{< youtube 9RJMfFFTshI >}}


This second video demonstrates how quickly event logs can be harvested and indexed as JSON data, which then can be queried in a public facing RESTful API.


{{< youtube m2OY1Fn2UoQ >}}


Once harvested a typical event log will look like the following:


{{< gist tpmccallum ae1448ad012d1bd7e5bfa5ac98a1b845 >}}


### Querying the data

Querying harvested data is very straight forward. The following is an example of a curl command which can be run in any linux console


{{< gist tpmccallum a85915019654e0dab38db41dcce789df >}}


The data returned from the above query reveals that there are 1885 TokenPurchase events in the dataset.


{{< gist tpmccallum f2d1ba311c4380bc4e4c3557f8d2fb80 >}}


This article previously mentioned that, whilst possible, it is unnecessary to emit pre-defined global variables or functions (namely “block.number”). If you look closely, you can see that the block number is returned in the event log by default. Upon further inspection you also will see many more useful pieces of data, such as blockHash, transactionHash, transactionIndex. The data that was emitted explicitly by the event log can be found in the “returnValues” section of the above data.


In order to demonstrate the flexibility of querying JSON data with JSON data, the next query will return only the **blockNumber** and the **address** of the buyer who purchased tokens.


{{< gist tpmccallum fe3ee82ca1cfe906ff6672033a58e56d >}}


As you can see, in the following results, the returned data is limited only to include the blockNumber and buyer’s address (along with, of course, the “name”).


{{< gist tpmccallum 1c26e54e69074938ef784d488c646c2d >}}


### Visualizing the data

Unlike other data sources, blockchain event log data is completely normalized — due to security considerations, only the highest quality key/value pairs will ever be included in blocks. Even creators of frontend DApps have a plethora of data validation tools available. Commands like the following allow frontend developers to cleanse data inputs before blockchain transactions are even considered.


{{< gist tpmccallum 5246d1ae5b458ed3be14238f48fe225f >}}


The great news is that once we have clean normalized data at regular intervals (i.e. fine granularity, event logs within transactions that are within individual blocks), we not only can analyze the data but we also can visually plot the data using [open source graphics libraries](https://plot.ly/graphing-libraries/) (ex. plotting information such as growth rates, daily sales, price fluctuations, etc.).


![](/images/20190210-blockchaindate-05.png)


Or perhaps more adventurous visualizations like chord diagrams, which can illustrate interaction between individual account addresses or deployed smart contract addresses.


![](/images/20190210-blockchaindate-06.png)


### Blockhain event log architecture

While in traditional data analytics we might have used SQL against a relational database, such as the example below.


{{< gist tpmccallum 5d7a00e01409c3fc944e04150c0492b4b >}}


Lately, and especially due to the fact that blockchain data is schema-less, we are able to query the JSON data using JSON data, as briefly demonstrated above.


Further, to querying the backend with JSON objects, we also are able to simply pass the returned JSON objects right through into a DApps frontend. This facilitates near real-time data visualization.


![](/images/20190210-blockchaindate-07.png)


### Data analytics — in real time

JSON is easily consumed by Javascript. If your frontend is written in a different language, this is still not a problem — there are many lightweight JSON libraries, for almost every language, which can serialize and deserialize JSON objects on the fly. These days, in pretty much all situations, JSON can be passed around an application with very minimal effort. This allows a DApp to perform very quick calculations and, moreover, make new information available to the end user via the frontend.


### Data analytics: predictive analytics

Predictive analysis can assist e-commerce ventures in many areas including, but not limited to, better consumer product recommendations, enhancements and improvements in supply chain management, as well as monitoring and reducing fraudulent activities.


---


## Looking through another lens: The Graph


![](/images/20190210-blockchaindate-08.png)


>“The Graph is a decentralized protocol for indexing and querying data from blockchains, starting with Ethereum” (Thegraph.com, 2019).


TheGraph is underpinned by [GraphQL](https://github.com/facebook/graphql), an open-source query language and execution engine that originally was developed by Facebook. Let’s take a closer look at TheGraph via a quick demonstration.


So far we have demonstrated how event logs can be harvested, stored off-chain and then queried (using JSON) via a public facing RESTful API.


TheGraph provides, [amongst many other things](https://thegraph.com/), a mechanism for a DApp to directly fetch and consume only the exact amount of data that the DApp actually requires, at any given time. Here is an example of a GraphQL query for TheGraph.


{{< gist tpmccallum 11cd2daf6f725c61c9e08a8f6d5ffc70 >}}


Right off the bat, we can see (and officially confirm using [JSONLint](https://jsonlint.com/)) that TheGraph is different to a traditional RESTful Web Service, in that this GraphQL query is not written in valid JSON.


This is normal!


In fact, this GraphQL syntax is more lightweight than JSON because it does not have to specify whole key:value pairs, such as {“event”: true} and so forth.


>In case you are wondering, we are querying the Uniswap Exchange protocol on the Ethereum mainnet for these examples. Only this time via [TheGraph’s online explorer](https://thegraph.com/explorer/subgraph/graphprotocol/uniswap).


So what does the response look like from TheGraph? Here it is:


{{< gist tpmccallum c87e783da4d27ea0711c33e889db1dd5 >}}


You will be very pleased to see that **the response is, in fact, valid JSON**. You also will notice that this data is minimalistic. We have essentially asked the following question of TheGraph.


>“Considering all of the Uniswap transactions to date, please give me only the name of the very first event log which was ever emitted.”


We can build on this first query by expanding the query to ask for not only the event, but also the block number.


{{< gist tpmccallum f26178b94056834165e255ff6a801e8c >}}


The following result shows that the event was mined into block number 6629139.


{{< gist tpmccallum 6c45cd6db5dd844bf1b77d5aef739509 >}}


Just purely out of curiosity, another way that we could achieve/confirm this would be to return **all** of the event logs, ordered by block number in ascending order.


{{< gist tpmccallum 46df6ef40848393c94b801af05820602 >}}


{{< gist tpmccallum 19a24c2bf9b35235536641a9ded8c5a1 >}}


We have just taken a decent dive into blockchain event logs, so let’s take a step back for a minute and reconsider the fundamentals of what we are trying to achieve overall.


---


## A helicopter view


Armed with the knowledge that there are many, more complex, low-level data flow diagrams from projects such as EOSs demux-js and, of course, TheGraph, let’s come back out of the details and look at the helicopter view of blockchain storage and DApps.


![](/images/20190210-blockchaindate-09.png)


The purpose of the above diagram is just to solidify that lightweight, real-time data transfer is required between any off-chain storage mechanism and a particular DApp.


### A protocol vs. an architectural style

From a design perspective, let’s briefly think back to the days of the Simple Object Access **Protocol** (SOAP). Whilst SOAP facilitated communication between disparate machines it also relied on a set of pre-defined application datatypes that were essentially a permanent structure. Any changes (ex. updates to the software application or changes to the static configuration) would disrupt or completely render the previously working interoperability inoperable. Simply put, SOAP is a rigid **protocol**.


Representational State Transfer (REST), on the other hand, introduced essentially an architectural style. Systems that conformed to all of the six architectural constraints were considered RESTful. Further, Web Services that adhered to the architectural constraints were considered RESTful APIs. (You may recall our using RESTful API calls via “curl” at the beginning of this article).


Still, in design mode, while it is tempting to explore ways in which we can, say, improve JSON compression (between the storage and the DApp) and so forth, thinking like this takes us down the path of a protocol — a protocol which enforces that both sides have to agree on a pre-defined set of rules, and which forces the client side (in this case the DApp) to perform additional work (unzipping, decoding etc).


Taking more of an architectural design viewpoint, would it be more effective, in terms of flexibility and interoperability, to focus on conventions as opposed to static configuration? We must remember that smart contract developers can create their own custom event logs that can emit one to many variables of various data types. Do we want to be setting up static configuration for each contract that is deployed in the blockchain network? Is human driven static configuration sustainable? Can it be avoided altogether through the use of strong conventions and machine automation? These are all great design questions. Can you think of any more?


The next wave of blockchain architecture is rising and, right now, there are big opportunities for holistic off-chain storage architecture projects to thrive.


Considering all of the above, is it fair to say that an off-chain architecture should:

* provide a mechanism to autonomously harvest a smart contract’s event log data based purely on an ABI file and a smart contract’s address?

* automatically assign correct data field types (based solely on the smart contract’s ABI)?

* require only a minimal amount of configuration, automated schema generation as per the previous point?

* provide sufficient internal querying, filtering and logic in order to produce the most succinct responses?

* automatically/dynamically offer autocomplete syntax to the calling software?

* provide a variety of default visual frontend display portals?

* provide a library of built-in analytics (not only to explore trends, correlations and so forth, but also to generate data sets for machine learning)?

* provide a mechanism to interoperate with ubiquitous business software, file formats as well as content management and software development applications?


This is a very exciting time. We have an unprecedented amount of information, documentation and software available, as well as the appropriate decentralized infrastructure to test and deploy your projects on.


If you’d like to ask any questions or learn more about anything mentioned in this article, please leave a comment below.


Happy building!


## References


Antonopoulos, A. (2017). Mastering Bitcoin. 2nd ed. O’Reilly Media, Inc.


Thegraph.com. (2019). The Graph Docs. [online] Available at: [https://thegraph.com/docs/introduction#what-the-graph-is](https://thegraph.com/docs/introduction#what-the-graph-is) [Accessed 6 Feb. 2019].


