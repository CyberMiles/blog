---
title: "基于CyberMiles公链的 Ewasm 测试链正式发布"
date: 2020-04-23T10:01:23+08:00
draft: false
tags: ["ethereum","blockchain programming","WebAssembly"] 
categories: ["zh"] 
---

# 基于CyberMiles公链的 Ewasm 测试链对外发布


基于 Second State 的虚拟机技术及CyberMiles 公链的区块链软件，新的公共 Ewasm 测试链上线了。CyberMiles 成为全球首批支持以太坊下一代虚拟机的公链。开发者可以在 CyberMiles 的 [Ewasm 测试链](https://docs.secondstate.io/devchain/getting-started/cybermiles-ewasm-testnet)上运行以太坊下一代虚拟机，部署开发 Ewasm 智能合约。

![CyberMiles Ewasm TestNet](/images/20200423-ewasm-testnet-01.png)

WebAssembly 提供了接近于本机代码的性能，而不失安全性。另外，WebAssembly 社区更加庞大，通过  LLVM 工具链，WebAssembly 可以支持20多种编程语言，给了开发者更多的自由。同时这也是扩大区块链社区的契机。

WebAssembly 的这些特性吸引了诸多区块链项目的目光，Polkadot、EOS、NEAR、 Oasis 都计划或已经将 WebAssembly 作为智能合约的执行引擎。

WebAssembly 虚拟机正在成为新一代区块链网络的通用基础设施，这其中也包括了CyberMiles。

上周，区块链创业公司 [Second State](https://www.secondstate.io/)基于 CyberMiles  的区块链软件开发的 Second State DevChain 支持了下一代的以太坊虚拟机 Ewasm，使开发者可以自己运行一条 Ewasm 开发链，从而将[Ewasm 智能合约](https://docs.secondstate.io/devchain/getting-started/run-an-ewasm-smart-contract)部署到真实的区块链上。Second State 由CyberMiles 基金会孵化。

本周，CyberMiles 公链适配了 Second State 的虚拟机[ssvm-evmc](https://github.com/second-state/SSVM)，发布了公共的 Ewasm 测试链，从而帮助开发者省去了大量的部署工作，更加开发者友好。

开发者可以方便地通过 [Docker](https://docs.secondstate.io/devchain/getting-started/cybermiles-ewasm-testnet)连接 CyberMiles 发布的 Ewasm 测试链，然后就可以直接在CyberMiles 的 Ewasm 测试链上部署运行下一代以太坊智能合约。
