---
title: "Under the hood: the FairPlay DApp (with FAQs)"
date: 2019-05-07T10:01:23+08:00
draft: false
tags: ["DApp","development"]
categories: ["en"]
---

In this article, I will explain the technical details behind the FairPlay DApp. If you haven't tried this DApp, please have a look at [FairPlay: decentralized prize drawing for e-commerce](https://blog.cybermiles.io/post/20190502-fairplay-en/).

> Talk is cheap, show me your code!

Sure, please see [here](https://github.com/CyberMiles/smart_contracts/tree/master/FairPlay). Any pull request or issue is welcome ;)

## Overview

This DApp is based on standard web technologies. It uses the [web3-cmt.js](https://cybermiles.github.io/web3-cmt.js/) JavaScript library to interact with the smart contract. When you scan the QRcode to [create a drawing](https://blog.cybermiles.io/post/20190502-fairplay-creator-en/) or [participate in a drawing](https://blog.cybermiles.io/post/20190502-fairplay-player-en/)), the DApp will render in a `WebView` in your CMT Wallet application. 

Anytime when the user creates a new FairPlay drawing, a new contract is deployed on the CyberMiles public blockchain. In the new contract, we use `mapping (address => Player) players; ` to store the basic information of the participants. To be more specific, the `Player` struct is as follows:

``` 
struct Player {
    uint ts;
    string name;
    string contact;
    string mesg;
    string confirm_mesg;
}
```

The variable `ts` specifies the time when the player participates in the play. The `name` and `contact` can be picked by the user as he wants. The `contact` can be entered in a number of ways, such as E-mail, Telegram, Twitter, etc. It's worth noting that the user doesn't need to enter his private information -- he can enter an anonymous twitter account or email address as long as he can be reached via them in case he wins. The `mesg` is the note the player leaves when he participates, whereas the `confirm_mesg` is the final comment given by the winner. Both messages are public.


## FAQ

1. What happens when different people in different time zones participate in the same Fair Play Drawing?

    * Interesting question. The answer is that the Date Time shown in the DApp varies with the user's local time zone. It is impossible to deceive the contract about the time. Sounds really smart, right? The solution is as following:
        * **When create the play**: The Date Time picked by the user as the end time, is converted to a timestamp as seconds since Unix epoch, according to the user's local time zone, then is sent to the contract, where it is compared against the special variable `now` (the current block timestamp as seconds since Unix epoch), in order to make sure the drawing time is after now.
        * **When participate in the play**: The front-end first gets the timestamp from the contract, then converts this timestamp to the readable date time, according to the user's local time zone.
    
2. Is this DApp partially or completely decentralized? 

    * We are completely decentralized! Yeah, you might be wondering how we manage to save the pictures uploaded in a totally decentralized DApp, without a back-end server. It could be possible to encode the whole picture and then store it on our blockchain. But that would a terrible waste of blockchain storage space (and gas fees). Instead, our DApp first directly uploads your picture from the browser to [cloudinary](https://cloudinary.com/documentation/upload_images#direct_uploading_from_the_browser), a secure and public platform providing  media management services, and then fetches the URL from this platform and stores it on the CyberMiles public blockchain.

3. Is the FairPlay DApp really fair?

    * Yes! We have built-in function [rand()](https://www.litylang.org/rand/) to generate the real random number. The relevant part of code to calculate the winners is as follows:
    
        ```
        // Construct the winner_addresses array
        for (i=0; i<player_addresses.length; i++) {
            cache.push(player_addresses[i]);
        }
        for (i=0; i<number_of_winners; i++) {
            uint r = rand () % cache.length;
            winner_addresses.push(cache[r]);
            cache[r] = cache[cache.length-1];
            delete cache[cache.length-1];
            cache.length--;
        }
        ```

4. How many languages does it support now?
    
    * Now only the English and Chinese are supported. If you are interested in this DApp and willing to contribute to the translation of other languages, please go to follow the format of [this file](https://github.com/CyberMiles/smart_contracts/blob/master/FairPlay/dapp/js/language/en.js) and make a pull request. Thanks.

