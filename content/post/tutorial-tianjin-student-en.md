---
title: "Tutorial: How to record your graduate certificate on CyberMiles public blockchain"
date: 2020-09-10T15:00:23+08:00
draft: false
tags: ["CMT","developer","blockchain development"] 
categories: ["en"] 
---

The graduation certificate of Tianjin University will be permanently recorded on the CyberMiles blockchain, which can be stored permanently and cannot be tampered with! How is this achieved?

The entire Dapp design is divided into two parts: * The smart contract records key information to ensure that the information cannot be corrected. This part is equivalent to the replacement of traditional applications; * The front-end display page is implemented using HTML, JavaScript, and CSS, 100% copying the real world certificate.

The two parts of the smart contract and the front-end display page are implemented through web3-cmt.js.

The following is a detailed explanation of the smart contract part for everyone. The contract basically does not involve calculations, and is mainly implemented through strings

```
contract TianjinStudent { //Contract name, here can be changed at will
  string private name; //Contract
  bool private isFemale; //Select gender
  string private birth; // Birth date
  string private prog_date; // Study time
  string private certId; //The number of the paper certificate is unique
  string private cert_date; //Certificate awarding date
  string private url; // Certificate photo link
  
  //The parameters here can be customized based on needs

  constructor (string _name, bool _isFemale, string _birth, string _certId, string _url ) public {
    name = _name;
    isFemale = _isFemale;
    birth = _birth;
    certId = _certId;
    url = _url;
    prog_date = "5/1/2019-4/30/2020";
    cert_date = "7/18/2020";
    }
    
   // constructor defines the constructor. When the contract is created, the object is initialized. The object is called the initial value. In this example, it is the various parameters we mentioned above.

  function get() public view returns (string, bool, string, string, string, string, string) {
    return （name, isFemale, birth, certid, prog_date, url）
    
   }
  }
  
  // The stage function implements the parameters of calling the corresponding parameters, returning the parameters of name, isFemale, birth, certid, prog_date, and url.
  
```
    
Open [BUIDL for CMT](https://buidl.secondstate.io/cmt)，Copy and paste the above code into BUIDL, click on compile，get the ABI and parameter of the contract.

![](/images/tianjin-student-01.png)

Fill in the parameters on the graduation certificate, name, gender, date of birth, study time, certificate number, certificate submission time, and graduation certificate student photo link into the information field at the beginning, and click "Deploy on the chain". 


![](/images/tianjin-student-02.png)

Pay a little gas, and the contract is deployed.  Click the call button and you can see the information just recorded on the chain. In this way, the core information on the graduation certificate is put on the chain. The next thing that needs to work on is the front-end display page. 

