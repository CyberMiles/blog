---
title: "CyberMiles 2019/04 MainNet software update"
date: 2019-04-04T15:30:23+08:00
draft: false
tags: ["mainnet","update"] 
categories: ["en"] 
---

![](/images/20190404-mainnetupdate-02.png)

Following an on-chain governance vote on April 1st 2019, the CyberMiles public blockchain validators (super nodes) coordinated a successful software upgrade on April 4th 2019 to improve the usability and stability of the blockchain. After a brief service interruption to restart the validator nodes with upgraded software, all services have now resumed. 

The highlights from this upgrade are as follows. 

### Application developers

We brought two exciting features to the CyberMiles MainNet. They aim to improve developer and end user experience for DApps. The first is secure random numbers, which is critical for many types of applications including gaming and e-commerce. See more here:

<https://www.litylang.org/rand/>

The second key feature is the ability for smart contract owners to pay for “gas fees” allowing much easier onboarding of DApp users. See more here:

<https://www.litylang.org/gas/>

We will soon support “free” DApps in CMT Wallet as well. Stay tuned!

### Validators

For validators, we have simplified the award computation formula. We removed the nonlinear factor that rewards and punishes validators based on short term stake gains / losses. This nonlinear factor was the cause of wild validator ranking fluctuations in the past several weeks. 

We have also made significant improvements to the efficiency of the code. The node software should now run much faster and consume less resources. 

### Delegators

Delegators can now continue to earn staking awards during the 7 day unstaking period. The removal of the above mentioned “new stake” factor should also create more stable income streams for delegators. 


