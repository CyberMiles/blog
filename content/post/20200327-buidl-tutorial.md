---
title: "How to build a forum on blockchain via BUIDL for CMT"
date: 2020-03-27T10:01:23+08:00
draft: false
tags: ["BUIDL","smart contract","CyberMiles","development","blockchain"]
categories: ["en"]
---

The example dapp is a web application where people can publish their comments on a simple forum. All the comments are recorded on the blockchain.

![](/images/20200327-forum-04.png)

Create your own forum DApp according to the following steps.

### Step 1

Open the [BUIDL IDE tool for CMT](https://www.secondstate.io/buidl/) in any browser. [http://buidl.secondstate.io/cmt](http://buidl.secondstate.io/cmt)

### Step 2

Open the Accounts tab and send a little CMTs to your default account.

> If you have CyberMiles' Venus wallet, you could opt to use Venus in the Providers tab. BUIDL and dapps it creates will now use the default account in Venus to make contract calls and to pay for gas.

### Step 3

#### 3.1 Copy and paste the following code to the contract section of BUIDL.

The smart contract is very simple. It provides topics to be comment and keeps a record of comments. The `addComment` method is called by people to comment.

```
pragma lity ^1.2.3;

contract PublicComments {

    address owner;
    string public greeting;
    struct Comment {
        string name;
        string email;
        string comment;
    }
    mapping (address => Comment) comments;
    address [] addrs;

    constructor (string _greeting) public {
        owner = msg.sender;
        greeting = _greeting;
    }

    function setGreeting (string _greeting) public {
        require (msg.sender == owner);
        greeting = _greeting;
    }

    function addComment (string _name, string _email, string _comment) public {
        comments[msg.sender] = Comment(_name, _email, _comment);
        addrs.push(msg.sender);
    }

    function getComment(address _addr) public constant returns(string, string, string) {
        return (comments[_addr].name, comments[_addr].email, comments[_addr].comment);
    }

    function getAddrs () public constant returns (address []) {
        return addrs;
    }
}
```

> If you are not using Venus as the Provider, please open the Accounts tab and make sure that the default address has a little CMTs.

#### 3.2 Click on Compile and you will see the following. Enter your text and image URL to be voted on, and then click on deploy on chain.

![](/images/20200327-forum-01.png)

The contract is now deployed on the CyberMiles blockchain, and you can call its functions directly from inside BUIDL.

![](/images/20200327-forum-02.png)

#### 3.3 Go to the dapp section. Click on the Resources tab, and add the following as resources.

* CSS: https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css
* JavaScript: https://code.jquery.com/jquery-3.4.1.min.js

#### 3.4 Next, copy and paste the following HTML code into the HTML editor.

```
<div class="container">
    <br/>
    <div class="jumbotron">
        <p class="lead" id="greeting"></p>
        <hr/>
        <form id="form">
            <div class="form-group">
                <label for="name">Name</label>
                <input type="text" class="form-control" id="name" placeholder="">
            </div>
            <div class="form-group">
                <label for="email">Email</label>
                <input type="email" class="form-control" id="email" placeholder="">
            </div>
            <div class="form-group">
                <label for="comment">Comment</label>
                <input type="text" class="form-control" id="comment" placeholder="">
            </div>
            <button type="button" id="submit" class="btn btn-primary">Send Comment</button>
        </form>
        <div id="formSubmitted" style="display:none">Please wait up to 20 seconds for confirmation ...</div>
        <p id="me" style="display:none">Thank you, <span id="myname" class="badge badge-info"></span></p>
    </div>
    
    <table class="table table-striped">
        <thead>
            <tr>
                <th scope="col">Name</th>
                <th scope="col">Comment</th>
            </tr>
        </thead>
        <tbody id="likes">
        </tbody>
    </table>
    <p style="text-align:center">Created with <a target="_blank" href="https://docs.secondstate.io/buidl-developer-tool/why-buidl">the BUIDL IDE</a>.</p>
</div>
```
#### 3.5 Copy and paste the following JavaScript code into the JS editor.

```
var instance = null;
window.addEventListener('web3Ready', function() {
  var contract = web3.ss.contract(abi);
  instance = contract.at(cAddr);
  reload();
});

function reload() {
    instance.greeting(function (e, r) {
        $("#greeting").html(r);
    });
    
    // $("#form").css("display", "block");
    $("#formSubmitted").css("display", "none");
    $("#me").css("display", "none");
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            instance.getComment(address, function (ee, result) {
                if (result[0]) {
                    $("#form").css("display", "none");
                    $("#me").css("display", "block");
                    $("#myname").html(result[0]);
                }
            });
            
            var likes = "";
            instance.getAddrs(function (ee, addrs) {
                addrs.forEach(function(addr) {
                    instance.getComment(addr, function (ee, r) {
                        if (!ee) {
                            likes = likes + "<tr><td>" + r[0] + "</td><td>" + r[2] + "</td></tr>";
                            $("#likes").html(likes);
                        }
                    });
                });
            });
            $("#likes").html(likes);
        }
    });
}

$("#submit").click(function() {
    if ((!$("#name").val().trim()) || (!$("#comment").val().trim())) {
      alert("Please enter both a name and a comment");
      return false;
    }
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            $("#form").css("display", "none");
            $("#formSubmitted").css("display", "block");
            instance.addComment ($("#name").val(), $("#email").val(), $("#comment").val(), {
                gas: 499000,
                gasPrice: 0
            }, function (ee, r) {
                if (ee) {
                    window.alert("Failed at " + address);
                }
            });
            setTimeout(function () {
                reload ();
            }, 20 * 1000);
        }
    });
    return false;
});
```

> On CyberMiles, we can make contract function calls with gas price set to zero. That is because CyberMiles allows gas free operations for many operations.

#### 3.6 Click on Run to see the dapp in action! You can now vote thumb up or down inside BUIDL.

![](/images/20200327-forum-03.png)

#### 3.7 Finally, you can publish the dapp. 

Just click on the Publish button and give the dapp a name. Once published, you can share the published URL to the public to comment on your issue!

> If the dapp user has Venus for CMT installed, the dapp will ask whether she would like to use her Venus account instead of auto-generated or imported accounts.
