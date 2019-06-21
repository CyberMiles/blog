---
title: "FairPlay：一种新型DApp"
date: 2019-06-21T15:30:23+08:00
draft: false
tags: ["e-commerce","dapp","smart contract","search engine"] 
categories: ["cn"] 
---

*Credits: FairPlay is developed by Second State, an open source enterprise smart contract platform, and deployed on the CyberMiles public blockchain.*

### 什么是FairPlay

[FairPlay](https://www.fairplaydapp.com/) 使用智能合约进行公平透明的自动抽奖。 它允许任何人创建并参与抽奖等电商营销活动。 根据Google Analystics，自推出以来，FairPlay 的DAU（每日活跃用户）经常达到1000。FairPlay已经成为所有公链中最受欢迎的DApp之一。

用户不需要特意下载加密货币钱包，就可以加载FairPlay 网页（www.fairplaydapp.com）并查看正在进行中和已经结束的抽奖活动。 FairPlay 网页只是HTML 和JavaScript 文件的集合。 任何用户都可以在自己的计算机上启动Web 服务器并在本地提供这些文件。

{{< youtube lQ6dKy_Q42s >}}

![](/images/20190620-fairplay-dapp-01.png)

当用户需要进行智能合约交易时，例如创建新的抽奖或参加现有抽奖时，网页将提示用户打开CMT Wallet 以输入密码（进行数字签名）并完成操作。
![](/images/20190620-fairplay-dapp-02.png)

*注：安全随机数对像FairPlay这样的应用是至关重要的。 FairPlay部署在CyberMiles公链上，利用Lity虚拟机，它支持生成安全随机数作为本机操作。

从表面上看，FairPlay是区块链智能合约前端的DApp。 然而，在普通用户看不到的地方，FairPlay具有易于开发和维护的模块化架构。

这种架构的关键就是智能合约搜索引擎。

### 模块化架构

今天大多数DApp 依靠单一庞大的复杂智能合约作为“后端”，并由这种智能合约管理所有应用程序的用户和状态。 即使是由多个合约组成的系统，通常也需要注册表或合约管理器，以提供有关系统的汇总信息。


但是，大型智能合约通常很难编写和维护。 大型合约往往容易出错，在发现错误或问题时几乎无法进行修复，从而加剧了今天困扰DApp 的安全问题。 用户注册的相关合约也受到当今智能合约编程语言和虚拟机的限制，不支持复杂的数据查询操作。

FairPlay DApp，使用了一种不同的解决方式。 该DApp由许多抽奖活动组成，但每个抽奖都对应着一个独立的的智能合约。 当有人创建新的抽奖时，她/他就部署了一个新的FairPlay智能合约。 当抽奖活动结束时，这个智能合约实例也就被废弃。 这能够让开发人员不断地改进FairPlay 智能合约，添加新的功能，修复错误，因为接下来的每个抽奖活动都用到一个新的智能合约。 但是，这种方式面临着一个关键挑战就是DApp 如何管理组织不同地址在不同时间创建的智能合约，并且能够在统一的UI中提供所有的合约信息。 

这就需要智能合约搜索引擎。

智能合约搜索引擎

FairPlay DApp的主页就是搜索引擎。 搜索引擎允许用户通过关键字或标签查找特定的抽奖活动，以及用户之前参与过的抽奖活动。 搜索引擎索引了所有部署在区块链上的FairPlay 合约信息。

![](/images/20190620-fairplay-dapp-03.png)

使用了开源的智能合约搜索引擎，FairPlay DApp是去中心化的，因为任何人都可以搜索部署在FairPlay上的智能合约，同时也可以创建基于搜索引擎的DApp。 每个基于搜索引擎的DApp都可以使用不同的算法和查询逻辑，展示和推广针对特定用户和受众量身定制的FairPlay抽奖活动。

![](/images/20190620-fairplay-dapp-04.png)

于搜索引擎的DApps不需要复杂的大型智能合约。 相反，DApp中的每个智能合约都旨在完成一组有限的特定业务交易。 所有相关的智能合约都会汇总在搜索引擎中，这种模式更接近智能合约的设计目标。

### 解决用户引导问题

阻碍DApp采用的一个关键问题是需要加密钱包才能使用DApp。 如果要求用户下载并安装一个不熟悉的移动应用程序来尝试DApp，那么几乎不可能有用户愿意尝试。 此外，许多DApp 需要用户拥有加密货币才能体验相关服务，这通常要求用户先去交易所注册，然后使用银行转账来购买比特币。但回到DApp，用户只需支付几美分的“gas fee”就可以完成交易。 DApp用户很少一点也不奇怪。

DApp用户需要建立钱包帐户才能对智能合约进行交易。 但是，正如我们在FairPlay DApp中看到的那样，用户在DApp 中的大部分操作是搜索，浏览和查看信息，发生交易只是偶尔的行为。

FairPlay DApp允许用户在没有任何钱包账户的情况下访问搜索引擎，并查询每个合约的详细状态信息。 当用户尝试创建智能合约和发送交易时，Web应用程序会要求用户打开CMT Wallet 以完成交易。 此时，如果用户尚未安装加密钱包，则可以下载CMT Wallet App。

当然，有经验的用户总是可以直接从CMT Wallet的DApp浏览器打开DApp。 它可以在使交易签名和发送交易之间无缝衔接，从而提供最佳的用户体验。


FairPlay DApp 在CyberMiles区块链上运行，该区块链支持多种gas 费策略。 例如，CyberMiles区块链可以免除临时用户的gas 费，并允许合约创建者代替最终用户支付gas 费。 结合CMT Wallet 正在进行的UI优化，FairPlay 用户在使用DApp时不会看到支付gas 费的提示，这大大提高了新用户对交易功能的使用率。

### 结论

FairPlay DApp旨在通过智能合约搜索引擎，Web应用程序的轻松入门和无 gas 费交易等创新技术提高DApp的可用性。 
注：FairPlay由Second State开发，并部署在CyberMiles公链上。Second Sate是一个开源企业智能合约平台。
