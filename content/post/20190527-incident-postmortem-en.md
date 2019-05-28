---
title: "Service Disruption Incident Postmortem"
date: 2019-05-27T17:07:23+08:00
draft: false
tags: ["disruption","incident"]
categories: ["en"]
---

> tl;dr If your CyberMiles node stopped at block height 1724747, please stop it (CTRL-C works), upgrade software to [v0.1.8-beta-hotfix](https://github.com/CyberMiles/travis/releases/tag/v0.1.8-beta-hotfix) and restart. Or, you could also start [from a snapshot](https://travis.readthedocs.io/en/latest/connect-mainnet.html#snapshot) or [from genesis](https://travis.readthedocs.io/en/latest/connect-mainnet.html#sync-from-genesis). If you are a validator node, please make sure that you backup your config files and use those config files when you restart.

On May 26th 2019, the CyberMiles public blockchain suffered a technical failure that paused blockchain operation for 17 hours. During that time, no transaction was accepted or recorded on the blockchain. **There was no data loss nor transaction rollback.** 

As a decentralized public blockchain, CyberMiles is maintained by 24 validators across the globe. Any software change must be approved and then implemented by 2/3 of validators in order to take effect. With a diverse group of independent validators, the CyberMiles blockchain downtime, while regrettable, serves the purpose of maintaining security and integrity of transactions through consensus. Here is what happened. 

At around 5am EST (US Eastern Time) on May 26th 2019, a user submitted a transaction to create a smart contract on the CyberMiles blockchain. The transaction was included in block height 1724748. When the transaction is executed by the CyberMiles Virtual Machine (CVM) on each blockchain node, it triggered a bug that crashed the CVM. Specifically, when the user requested to execute a transaction without paying the gas fee, the CVM's [“free gas”](https://www.litylang.org/gas/) rules check for the transaction's target address. However, in the case of a smart contract creation transaction, the target address is null. That causes the software to throw an infamous null pointer exception, crashing the CVM. 

![](/images/20190527-incident-postmortem-01.PNG)

All CyberMiles nodes stopped at block height 1724747, unable to process block height 1724748. The problem was reported and escalated to CyberMiles core developers. The CVM developers based in Taipei immediately worked on the problem and produced [a software patch](https://github.com/second-state/lityvm/commit/557cc4935d94d6e1d6b947143788838ca98908f9). The blockchain engineering team in Beijing and USA then tested it and released a new software version [v0.1.8-beta-hotfix](https://github.com/CyberMiles/travis/releases/tag/v0.1.8-beta-hotfix). The core developers were ready to notify validators to review, approve, and implement v0.1.8-beta-hotfix.

![](/images/20190527-incident-postmortem-02.png)

However, that was when a second problem surfaced. Since the null pointer exception crashed the CVM, the virtual machine shuts down abnormally. The operations team based in Beijing, Los Angeles, and Austin discovered that some nodes could have corrupted database files after this crash. So, a straightforward restart with the new binary application is now out of the question. 

![](/images/20190527-incident-postmortem-03.PNG)

Out of abundant precaution, the engineering team decided to recommend all nodes to restart from an uncorrupted snapshot at block height 1724748 following CyberMiles' [snapshot fast sync procedure](https://travis.readthedocs.io/en/latest/connect-mainnet.html#option-2-binary-from-snapshot).

An uncorrupted node with all validator signatures on block height 1724748 is found in the community. It has a data size of 18GB after compression. The data snapshot from this node is then reviewed, verified, and validated, before it is made [available to the community](https://s3-us-west-2.amazonaws.com/travis-ss-bucket/mainnet/travis_ss_mainnet_1558862782_1724747.tar) again. 

At 5pm EST, the first validator nodes in North America came online with the update software v0.1.8-beta-hotfix. However, without the quorum of 17 active validators (that is 2/3 of 24 at block height 1724748), the blockchain cannot produce the next block. The quorum is reached at 10pm EST when Asian validators woke up on Monday morning. The blockchain services, such as [CMT Wallet](https://www.cybermiles.io/en-us/blockchain-infrastructure/cmt-wallet/), [CMT Cube](https://www.cybermiles.io/en-us/cmt/cmt-cube/), and [CMT Tracking](https://www.cmttracking.io/), are fully restored soon after. 

Key lessons

* The CyberMiles development team must increase software testing coverage, especially on original and innovative features. 
* Incident response coordination between core developers and validator operators could be improved. In this case, the root cause was identified and patched quickly, but coordination across validators took a very long time.
* DPoS blockchains like CyberMiles could suffer [service disruptions](https://www.trustnodes.com/2018/06/16/eos-stops-functioning-network) without forking or security breach. 
* A diverse group of validators improve blockchain security but could prolong service disruption by making it difficult to reach a quorum. 

