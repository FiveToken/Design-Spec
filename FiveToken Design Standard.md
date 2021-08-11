# Overview

The standards are compiled from the following reference:

- [MetaMask link to Ethereum](https://docs.metamask.io/guide/ethereum-provider.html#table-of-contents)，as the standard for FiveToken Chrome Extension as a link on Filecoin network; 

- [Walletconnect Mobile Linking](https://docs.walletconnect.org/mobile-linking)，as the standard for FiveToken Web3 APP to customize wallet connections on Filecoin network;

# General Requirements

- Provide DApps with API to FiveToken that requires compliance with the broader MetaMask link to Ethereum standards.  

- Walletconnect currently supports Ethereum only, with some parameters tweaked over the Walletconnect protocol for Filecoin networks.

  

# DAppLink API Plug-in

This documentation is mainly for the Filecoin Wallet chrome extension. DAppLink API into the website the user visits, the API allows the website to request the user's Filecoin account, read data from the user's connected blockchain, and suggest messages and transactions for the user to sign.

## Method

### DAppLink.version

Extension version number

###  DAppLink.isConnected()

Returns true to indicate that the API has accessed the current site, or if access fails, the page must be reloaded to re-establish the connection.

###  DAppLink.request(args)

Example：

```js
args :{
  method: string;
  params: object;
}
DAppLink.request({
    // open Filecoin Wallet
    method: "openFilecoinWallet",
    params:{
        // width
        width:window.outerWidth,
        // origin
        origin:'https://filscan.io'
    }
})
```

### DAppLink.onMessage(tag,callback)

tag Event identifier // tag:filecoinWalletAddress. Listen to the connection address data return data

You can listen for events like the following,

```js
// tag: string;
// callback:Funtion;
AppLink.onMessage(tag,(data)=>{
    console.log(data)
})
```

# Walletconnect API（Filecoin）

Connect Dapp and FilecoinWallet

1. Init WalletConnect

```javascript
import WalletConnect from "@walletconnect/client";
    // create new connector
    const connector = new WalletConnect({
      bridge, qrcodeModal: QRCodeModal
    });
```

2.  Listen `connect` event

```javascript
connector.on("connect", (error, payload) => {
    const { chainId, accounts } = payload.params[0];
    const address = accounts[0];
     // after connect, you can get a filecoin address , but the chainId is invalid because      filecoin is different from eth, you should handle it by check the address
  });
```

3. Send Custom Request and listen `fil_sendTransaction` event

```javascript
    connector.sendCustomRequest({
           method: 'fil_sendTransaction',
           params: [{
             to: 't16wkgzlglyejqlougingwbnztnp7lrh2xgzlbviq',
             value: '20'
           }]
    });
   connector.on('fil_sendTransaction',async (error,payload)=>{
      console.log('fil_sendTransaction')
      console.log(payload)
    })
 //wallet will return a object    {signed_cid:'bafy2bzacecexugyxkrmomzo5afpldp5j4atf6fin63si2tqsjkcx77w6e2nqy'} after finish push message 



```

