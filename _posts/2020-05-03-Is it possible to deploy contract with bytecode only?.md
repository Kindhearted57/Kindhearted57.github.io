---
title: "Is it possible to test contract with bytecode only?"
key: 2020-05-03-deploy_bytecode_contract
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---

# Is it possible to test contract with bytecode only?

##background
I only have the bytecode of the contract, and bytecode-level constuctor for the contract. I want to deploy the contract to my local private chain and invoke some methods to run my test logic on the contract. At first, I was thinking about geth, and have done a lot of preperation. But later I realised that it is acutally not necessary to use geth, testrpc or ganache-cli can make some of the parts more convenient: No need to create accounts, and transfer ETH between accounts, no need to mine the transactions anymore....etc. At last my solution is to write a web3 script, and interact with the ganache-cli chain on my local machine.

## Deployment
I successfully deployed the contract with the guidance of [this answer](https://ethereum.stackexchange.com/questions/70774/deploying-contract-using-bytecode).
The deployment process is a transaction, and the parameters is explained [here.](https://programtheblockchain.com/posts/2018/01/09/how-smart-contract-deployment-works/)

```
eth.sendTransaction({from:eth.accounts[0],to:"0x0000000000000000000000000000000000000000",value:0,data:storageBincode})
```

`storageBincode` here represents the bytecode of the contract, but we should note that the bytecode here should be (contract bytecode + constructor bytecode).
![nullcontractaddress](https://note.youdao.com/yws/api/personal/file/WEBfd1156ad9e782616edb090f268b0c86c?method=download&shareKey=1ceaeef1ffe951de08732962877413f4)
[This blog](https://medium.com/@gus_tavo_guim/deploying-a-smart-contract-the-hard-way-8aae778d4f2a) shows the hard way to deploy smart contract. At first I thought that the ABI file is always needed for deployment, but as we didn't provide the abi data, so `contractAddress` here is null. 
However, [This answer by Ismael](https://ethereum.stackexchange.com/questions/73902/contractaddress-null) explained the reason, by removing the `to` parameter, I got the contractAddress.
## Call methods
[The answers under this question](https://ethereum.stackexchange.com/questions/9733/calling-function-from-deployed-contract) reveals the method to call a function. However, the method implemented in the blog is in solidity,
`bytes4(keccak256("f1(arg1_type, arg2_type)")`
As there are just a few methods used in this test, there is no need to calculate them.
[This link](https://piyolab.github.io/playground/ethereum/getEncodedFunctionSignature/) can get the signature encoded from function string
### Transaction
`sendTransaction` method in web3 have these arguments: `from`, `to`, `value`,`gas`, `data`. As for how to fill these values, we can first test the certain contract on [Remix](http://remix.ethereum.org/), then we can learn the information from the receipt. 
![](https://note.youdao.com/yws/api/personal/file/WEB1ecc127d2eeeb316ecc3a2cfd0a445bf?method=download&shareKey=a249f0a9746c0940456eeb6964f1b18c)
### Call
Note that, call methods are much different from transactions, `sendTransaction` can only return the hash value of the transaction, while in order to get the return value from call method, we have to implement `eth.call(to:, data:,)`. 
## Testing
I need to know the status of the transaction, however, Geth uses Web3 with version lower than 1.0.0, and this is a known bug in the earlier version as presented in [this issue](https://github.com/ethereum/web3.js/issues/2410)
The speed is so low that I thought there are something about the statement in the console.
[npm: request has been deprecated](https://medium.com/better-programming/request-has-been-deprecated-a76415b4910b)
[How smart contract deployment works](https://programtheblockchain.com/posts/2018/01/09/how-smart-contract-deployment-works/)

### How do I construct my testing code?
First, it is quite important to import the json file, which contains the bytecode information for every contract to be tested.
`SyntaxError: Unexpected token } in JSON at position 93`
This error keeps showing up in all forms. At last I found out that this has something to do with my json file, (I thought there are something wrong with npm at first because it is about JSON.) At last I found out that, My json file is constructed like this: a:`{{a1:,b1:,}.....}`. So there is one comma more than normal, which lead to the 
### Etherscan analysis
The bytecode information can be divided into two types on [etherscan](https://etherscan.io). When source code is available, the bytecode is creation-bytecode, which means
[A guide to promise in web3](https://stackoverflow.com/questions/46514570/using-async-await-on-custom-promise)
### Geth parameter
There are some parameters that have to be pre-set to the geth when it started.

* With script `--exec 'loadScript("/home/jiru/桌面/mine_transactions.js")'`
* UnlockAccount


```geth --rpc --nodiscover --datadir data0  --port 30303 --rpc --allow-insecure-unlock --rpcport 8545 --rpcapi "db,eth,net,web3" --rpccorsdomain "*" --networkid 1108 --ipcdisable --exec 'loadScript("/home/jiru/桌面/mine_transactions.js")' console```

### How to write/read json file in javascript
Write:
### How to use regular expression in js?
### Cannot read property 0 of undefined


```
npm install file-saver --save
bower install file-saver
```
###
This works in node console, but won't work if I put these in a .js file and run it, keeps telling me that `accounts` is undefined
```
var accounts
async function getCurrentAccount()
{
accounts = await web3.eth.getAccounts();
return accounts;
}
getCurrentAccount()
```
## issues
### Geth part(not implemented)
#### Ethereum Script
I found a [repository](https://github.com/niksmac/ethereum-scripts) in github with many scripts that can be used in geth and make things easier. This problem was raised by:
[How to make miner to mine only when there are Pending Transactions?](https://ethereum.stackexchange.com/questions/3151/how-to-make-miner-to-mine-only-when-there-are-pending-transactions)
But later I found out that in my case it's much easier to use ganache-cli instead of geth which make everything easier XD.
[One excellent blog](https://www.zybuluo.com/qidiandasheng/note/1246984)

#### eth not defined
I didn't create an web3 instance. eth is part of web3.
create it like this.

```
const Web3 = require('web3')
const web3 = new Web3('http://localhost:8545')
```
Then this will disappear.
#### genesis.json
##### miner.start(1) hangs forever
Actually, two weeks ago when I was doing my homework, `miner.start(1)` hangs forever. [parameter explain](https://github.com/bokkypoobah/DeveryCrowdsale/blob/master/test/genesis.json#L4) 
At first I didn't understand how geth works, and was certain that there must be something wrong with installation, after install & uninstall, I pathetically found out that this is a rather silly way to try to solve any problem. Take the time to look into documentation or copy and paste the error information on Google is much more better. Later I found out that `"eip150Block"` field is blank in genesis.json. However, I don't know why this helps yet.
##### No status field in receipt

I created an issue on geth github page, and got [one response](https://github.com/ethereum/go-ethereum/issues/21016), but web3 version is one reason, later I found out another issue mentioning that when block number < 4700000 there will not be `status` field. This number reminds me of Byzantium. [This blog](https://www.felix021.com/blog/read.php?2186) is extremely clear and I learned from it that only if I set the parameter `ByzantiumBlock` in `genesis.json`, then there will be `status` field in the receipt. Because this field shows up after Byzantium.
[This is the docs](https://github.com/ethereum/wiki/wiki/JavaScript-API#contract-methods) for web3 utilized in geth.
[Geth docs](https://geth.ethereum.org/docs/)
#### gas required exceeds allowance&exceeds block gas limit
I want to send a transaction.
`var result = eth.estimateGas({from:eth.accounts[1],to:contractAddress,value:0,data:"0xa9059cbbabbbd78b61fd39cb518e08b41fdd3fe9af8ade1d0000000000000000000000000000000000000000000000000000000000000074"})`
returns `51165`.
However, when I tried to send this transaction, here comes this error:
`gas required exceeds allowance (16276726) or always failing transaction`
This result from revert.

## Conclusion
![](https://note.youdao.com/yws/api/personal/file/WEB7db5415696afe9f8ecd35212e5164ffa?method=download&shareKey=45493eb6d8d3c47de3afd26ed148ced2)