---
title: "Writing a Hello World Smart Contract for CyberMiles"
date: 2019-02-15T15:01:23+08:00
draft: false
tags: ["developer"]
categories: ["en"]
---

![](/images/20190215-helloworld-01.png)

"Hello World" is often the first program those learning to code write. It's no exception for building your first blockchain smart contract. This is a step-by-step guide of how to develop a "Hello World" smart contract with Lity.

## Prerequisites

Before you start, prepare the following tools to develop on the CyberMiles blockchain.

### Install Chrome browser

The CyberMiles visual developer tools work on the Chrome browser platform. Download and install [Chrome browser](https://www.google.com/chrome/).

### Install the Venus wallet as a Chrome extension

Venus is an extension for accessing CyberMiles enabled Dapps in your Chrome browser. It works as a wallet and allows you to sign CMT transactions on the webpage. Download and install [Venus wallet](https://www.cybermiles.io/en-us/blockchain-infrastructure/venus/)

When you finish installing, click the chrome extension icon to set up the wallet according to the instruction.

Then, switch the tab to CMT Test Network.

![](/images/20190215-helloworld-02.png)

### Get 1000 Test CMTs from the CMT Testnet Faucet

With your Venus wallet address, you can get CMT test tokens from the [CMT Testnet Faucet](https://travis-faucet.cybermiles.io/).

Input the wallet address you just generated with the Venus.

![](/images/20190215-helloworld-03.png)

By clicking "Send me 1000 CMT and 1000 TEST", you will find 1000 CMT test token in your Venus wallet. You will find this useful later for paying gas free in the development process.

![](/images/20190215-helloworld-04.png)

### Load the Europa IDE

Load the [Europa IDE](http://europa.cybermiles.io/) in your Chrome browser to start coding.



## Work with the Smart Contract

Copy and paste the following code into your Europa IDE code editing window.

{{< gist juntao cc1565b76949a6bbd8bfb15aa9e53dc8 >}}

### Compile 

Click on "Start to compile". If it's compiled successfully, it will show the green bar with the name of the contract. Europa will check the code automatically for the most common coding errors.

![](/images/20190215-helloworld-05.png)

If the codes work well, you can deploy it to the smart contract, On the Run tab, click "Deploy".

### Deploy 

![](/images/20190215-helloworld-06.png)

Use the test tokens in your Venus wallet to pay the gas fee. Most of the time, the Venus payment page will pop up automatically. If it doesn't, you just need to click the extension icon.

![](/images/20190215-helloworld-07.png)

After submitting the transaction, you will see the name of the smart contract, together with the address shown on the Deployed Contracts. That is, you have successfully deployed your first smart contract on the CyberMiles blockchain! Hooray!

![](/images/20190215-helloworld-08.png)

### Run

Click on "Greet". A "Hello World" message is printed!

Click on "Owner". This shows the Venus wallet address you used to pay the gas fee.

![](/images/20190215-helloworld-09.png)

### Terminate

You can also choose to terminate the smart contract when you've done playing with it.

Click on "terminate" and pay for the gas fee.

After completing the payment, the smart contract address will turn to zero. The Greet content is no longer displayed. That is, the smart contract is not functioning and successfully terminated.

![](/images/20190215-helloworld-10.png)

## Learn to code

Now it's time to understand how each line of code works!

### Version Pragma

This indicates the smart contract programming language and version number. We use Lity as the smart contract programming language.

```sol
pragma lity ^1.2.4;
```

### Contract and function

This states the name of the contract `Human`.

It also states the only two functions of the smart contract - `greet` and `terminate`.

```sol
contract Human {
    
  function greet() {
  
  }

  function terminate() {
  
  }
}
```

### Set Owner of the contract

This sets the owner of the smart contract. First, the address of the owner is a public one. Second, it defines the one who pays the deploying the smart contract is owner. Third, it asserts that only the owner can modify the function of this smart contract.

```sol
    address public owner;    

    modifier onlyOwner() {        

        assert(msg.sender == owner);  

        _;   

     }    

    constructor () public { 

        owner = msg.sender;   

     }
```

### Implement the greet function

This indicates that when you call `greet`, it will return "Hello World". And you can change "Hello world" to any text you want for sure.

```sol
  function greet() public pure returns (string) {
      return "Hello world";
  }
```

### Clean up the smart contract

This shows how to terminate the smart contract. It defines that only the owner can execute this function.

```sol
  function terminate() external onlyOwner {    
    selfdestruct(owner);  
  }
```

