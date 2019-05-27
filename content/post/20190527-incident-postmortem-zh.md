---
title: "服务中断事件事件回溯"
date: 2019-05-27T17:07:23+08:00
draft: false
tags: ["disruption","incident"]
categories: ["zh"]
---
 
2019年5月26日，CyberMiles公链遭遇技术故障，区块链因此暂停运行17小时。在此期间，区块链没有接受或记录任何交易，也没有发生数据丢失也没有交易回滚的事情。
 
作为一个去中心的公链，CyberMiles由全球24个验证人维护。任何软件的更改，必须经过批准，然后由2/3的验证人实施才能生效。服务中断非常令人遗憾，但凭借多元化的独立验证人，在CyberMiles区块链暂停服务时可以通过验证人共识，更好地维护交易的安全性和完整性。下面是事件过程。
 
在2019年5月26日美国东部时间凌晨5点左右，一位用户提交了一笔交易，用以在CyberMiles区块链上创建智能合约。该交易包含在区块高度1724748中。当每个区块链节点的CyberMiles虚拟机（CVM）执行该交易时，触发了一个导致CVM崩溃的错误。具体而言，当用户请求执行交易而不需支付gas时，CVM的[“free gas”](https://www.litylang.org/gas/)规则会检查交易的目标地址。但是，**该用户创建的智能合约，交易的目标地址为空（null）。** 由此导致软件抛出了臭名昭著的空指针异常，导致CVM崩溃。
 
![](/images/20190527-incident-postmortem-01.PNG)

所有CyberMiles节点无法处理块高1724748，都在块高1724747处停止了工作。此问题被报告给CyberMiles核心开发人员。位于台北的CVM开发人员立即解决了这个问题，并制作了一个[软件补丁](https://github.com/second-state/lityvm/commit/557cc4935d94d6e1d6b947143788838ca98908f9)。然后，北京和美国的区块链工程团队对其进行了测试并发布了新的软件版本[v0.1.8-beta-hotfix](https://github.com/CyberMiles/travis/releases/tag/v0.1.8-beta-hotfix)。核心开发人员已准备好，并通知验证人审核，批准和实施v0.1.8-beta-hotfix。
 
![](/images/20190527-incident-postmortem-02.png)

然而，这时第二个问题出现了。由于空指针异常使CVM崩溃，因此虚拟机异常关闭。位于北京，洛杉矶和奥斯汀的运营团队发现，此次崩溃发生后，某些节点的数据库文件可能已经损坏。因此，使用新的二进制应用程序直接重启，已经不可能。
 
![](/images/20190527-incident-postmortem-03.PNG)

出于谨慎的预防考虑，CyberMiles工程团队决定遵从CyberMiles [快照快速同步程序](https://travis.readthedocs.io/en/latest/connect-mainnet.html)，建议所有节点从块高1724748的未损坏处快照，进行重启。 
 
在社区中可以找到在块高1724748上具有所有验证人签名的未损坏节点。它在压缩后的数据大小为18GB。然后，在向社区再次提供这个文件之前，此节点的[数据快照](https://s3-us-west-2.amazonaws.com/travis-ss-bucket/mainnet/travis_ss_mainnet_1558862782_1724747.tar)进行了审核，验证，并证明了有效可行。
 
美国东部时间下午5点，北美的第一个验证人节点上线了更新的软件v0.1.8-beta-hotfix。但是，如果没有达到17个有效验证人数量（即块高度为1724748的24个验证人的2/3），则区块链不能产生下一个块。在处于亚洲的节点在周一早上恢复工作后，美国东部时间晚上10点，验证人进行了软件升级的人数达到了三分之二，区块链服务，如[CMT Wallet](https://www.cybermiles.io/en-us/blockchain-infrastructure/cmt-wallet/)，[CMT Cube](https://www.cybermiles.io/en-us/ cmt / cmt-cube /)和区块浏览器[CMT Tracking](https://www.cmttracking.io/)很快就完全恢复了。
 
主要经验教训
 
* CyberMiles开发团队必须提高测试覆盖率，尤其是我们的原创和创新的功能。
*核心开发人员和验证人之间的事件响应协调需要改进。CyberMiles团队快速识别和修补了问题发生的根本原因，但验证人之间的协调花费了很长时间。
*像CyberMiles这样的DPoS区块链可能会遭遇[服务中断](https://www.trustnodes.com/2018/06/16/eos-stops-functioning-network)而没有出现分叉或安全漏洞。
*多样化的验证人，可以提高区块链的安全性，但是，由于足够人数的验证人难以快速做出反应，服务中断的情况会延长。
