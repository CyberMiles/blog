---
title: "Building a safer crypto token"
date: 2018-04-25T10:01:23+08:00
draft: false
tags: ["security","overflow"]
categories: ["en"]
---


Another day, another billion-dollar Ethereum hack. This time the hack evaporated more than two billion USD worth of market value due to a bug in the smart contract written in Solidity. On the CyberMiles blockchain, while we are fully backward compatible with Ethereum, we make this type of bug virtually impossible to write. Here’s what you need to know.


---


The Beauty Chain (BEC) was a high-profile cryptocurrency in China. Its stated goal is to be “a truly decentralized and beauty-themed ecosystem.” It started trading on OKEX on Feb. 23, 2018 and [spiked 4,000 percent on the first day of trading](https://news.8btc.com/bec-spiked-4000-on-first-trading-day-another-pump-and-dump-scheme). From its peak market cap of around 70 billion USD, it has gradually come down to around two billion USD as of April 22, when its trading value suddenly dropped to zero. (OKEX subsequently [suspended trading of BECs](https://news.8btc.com/okex-suspend-bec-trading-due-to-irreversible-bug-in-smart-contract).)


![](/images/20180425-safetoken-02.jpeg)

Of course, BEC is not alone in this. Another widely traded cryptocurrency, SmartMesh (SMT), which had a market cap of more than 300 million USD (for total supply of tokens), suffered a similar fate. SmartMesh claims to be the “token-based next generation Internet protocol,” but while SMT has a smaller market cap than BEC, it is much more widely traded and probably caused more damage in terms of real loss. (Huobi suspended trading of SMT.)


### What happened?

It was discovered that hackers were able to exploit the BEC/SMT smart contracts and [issue vigintillions (value with 63 zeros) of tokens to themselves](https://etherscan.io/tx/0xad89ff16fd1ebe3a0a7cf4ed282302c06626c1af33221ebe0d3a470aba4a660f). It has resulted in [a security alert](https://medium.com/@peckshield/alert-new-batchoverflow-bug-in-multiple-erc20-smart-contracts-cve-2018-10299-511067db6536) since apparently multiple ERC 20 token contracts are susceptible to this problem.


![](/images/20180425-safetoken-03.png)


The hacked transaction and account balances on BEC and SMT smart contracts. Can you count the zeros?

---


### How did this happen?

Technically, there is a bug in the ERC 20 smart contract that issues and manages the crypto token. Let’s use BEC as an example. (SMT is very similar.) The developer added a method called 'batchTransfer()' to the contract. The method is intended to facilitate token transfer to multiple parties at once (i.e. a batch).


![](/images/20180425-safetoken-04.png)
Image credit from the CVE-2018–10299 security alert


However, the developer made a crucial mistake in the following line of code:
'uint256 amount = uint256(cnt) * _value'


The 'cnt' here is the number of recipients to transfer the tokens to, the '_value' is the number of BECs each recipient should receive, and the 'amount' is the computed amount the contract should withdraw from the sender's account for the transfer.


Now, what if someone calls this 'batchTransfer' function with a 'cnt * _value' so large that the product exceeds the capability of the computer to keep track of 256 bit integer ('uint256')? It will cause an infamous “buffer overflow,” and cause the 'amount' to compute to zero. Notice that both 'cnt' and '_value' are legitimate 'unit256' numbers, but the 'amount' is now zero. Hence you can transfer '_value' number of BECs to anyone without withdrawing anything from the sender's account. So a sender with 1 BEC can now send trillions (in fact, vigintillions) of BECs to other recipients. [Thanks to the Alex O’s explanation in the comments section!]


### How to prevent it?


The way to prevent this is to use the SafeMath library function when doing integer math. SafeMath detects and addresses the “integer overflow” problem. Interestingly, the BEC/SMT contract developers did use SafeMath, except in that one instance!


Why? One might guess that in the gold rush of ICOs, the developer might just copied and pasted some ERC 20 contract code from the Internet without fully understanding their implications.


### How could CyberMiles prevent this?


CyberMiles is improving the whole stack of the Ethereum Virtual Machine (EVM) from the compiler to the virtual machine runtime. One of the important safeguards we put into the place is to enforce the use of SafeMath in token contracts, such as ERC 20, ERC 223, ERC 721, and ERC 884. It works as follows.


**Compiler optimization**: If the compiler detects the source code to be compliant with one of the above token specifications, it will throw an error on any “naked” integer operation not wrapped around in SafeMath.


Of course this behavior can be overridden with a compiler flag to ignore such errors. But the enforcement of SafeMath is turned on by default, ensuring the safety of token contracts.


**Runtime optimization**: At the virtual machine runtime, we automatically will analyze integer assignment operations in real-time and stop the contract execution with an error when overflow occurs.


### The moral of the story

Try to understand the code you got from the Internet, especially if it handles money!


Deploy your token smart contracts on the CyberMiles blockchain for better safety, advanced language features, and low fees (MainNet coming online in Q3 2018).


Or at least use the CyberMiles Lity/Solidity compiler to avoid common errors.


Learn more at cybermiles.io.


Or at least use the CyberMiles Lity/Solidity compiler to avoid common errors.

Learn more at <cybermiles.io>.
