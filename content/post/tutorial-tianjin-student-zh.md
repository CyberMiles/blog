---
title: "教程：如何将毕业证书存入 CyberMiles 公链"
date: 2020-08-10T15:00:23+08:00
draft: false
tags: ["CMT","developer","blockchain development"] 
categories: ["zh"] 
---

天津大学的毕业证书将永久记录到 CyberMiles 区块链，实现了永久可保存、不可篡改！这是如何实现的呢？

整个Dapp 设计分为两部分：
* 智能合约记录关键信息，确保信息不可篡改，这一部分相当于传统应用的后端；
* 前端展示页面，用 HTML、JavaScript、CSS 实现，100% 复制真实世界的证书。

智能合约与前端展示页面这两部分通过 web3-cmt.js 实现。

下面详细为大家讲解智能合约部分，合约基本不涉及计算，主要通过 string 实现。

```
contract TianjinStudent { //合约名字，此处可以随意更改
  string private name; //合约
  bool private isFemale; //选择性别
  string private birth; // 出生日期
  string private prog_date; // 学习时间
  string private certId; //纸质证书的编号，是唯一的
  string private cert_date; //证书颁发时间
  string private url; // 证书照片链接
  
  //这里的参数可以根据实际需求自定义

  constructor (string _name, bool _isFemale, string _birth, string _certId, string _url ) public {
    name = _name;
    isFemale = _isFemale;
    birth = _birth;
    certId = _certId;
    url = _url;
    prog_date = "5/1/2019-4/30/2020";
    cert_date = "7/18/2020";
    }
    
   // constructor 定义了构造函数，在创建合约时，初始化对象，为对象赋予初始值，在这个例子上，是我们上文提到的各种参数。

  function get() public view returns (string, bool, string, string, string, string, string) {
    return （name, isFemale, birth, certid, prog_date, url）
    
   }
  }
  
  // 这段函数实现了调用相应的参数，返回 name, isFemale, birth, certid, prog_date, url 这些参数。
  
```


    
打开 [BUIDL for CMT](https://buidl.secondstate.io/cmt)，将上述代码复制粘贴到BUIDL ，点击 compile，得到合约的 ABI 与参数。

![](/images/tianjin-student-01.png)

将毕业证书上的参数，姓名、性别、出生日期、学习时间、证书编号、证书颁发时间、毕业证书学生照片链接填入左侧的信息栏，点击 Deploy on the chain。 


![](/images/tianjin-student-02.png)

支付一点点gas，部署成功。点击 call 按钮就可以看到刚刚上链的信息。这样就实现了毕业证书上的核心信息上链。接下来需要解决的就是前端展示页面了。




