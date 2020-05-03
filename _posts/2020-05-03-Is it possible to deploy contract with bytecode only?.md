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
I only have the bytecode of the contract, and bytecode-level constuctor for the contract. I want to deploy the contract to my local private chain and invoke some methods to run my test logic on the contract.

## Deployment
I successfully deployed the contract with the guidance of [this answer](https://ethereum.stackexchange.com/questions/70774/deploying-contract-using-bytecode).
The deployment process is a transaction, and the parameters is explained [here.](https://programtheblockchain.com/posts/2018/01/09/how-smart-contract-deployment-works/)
```
eth.sendTransaction({from:eth.accounts[0],to:"0x0000000000000000000000000000000000000000",value:0,data:storageBincode})
```
`storageBincode` here represents the bytecode of the contract, but we should note that the bytecode here should be (contract bytecode + constructor bytecode).
![nullcontractaddress](https://note.youdao.com/yws/api/personal/file/WEBfd1156ad9e782616edb090f268b0c86c?method=download&shareKey=1ceaeef1ffe951de08732962877413f4)
[This blog](https://medium.com/@gus_tavo_guim/deploying-a-smart-contract-the-hard-way-8aae778d4f2a) shows the hard way to deploy smart contract. The ABI file is always needed for deployment, but as we didn't provide the abi data, so `contractAddress` here is null. 
[This answer by Ismael](https://ethereum.stackexchange.com/questions/73902/contractaddress-null) explained the reason, by removing the to parameter, I got the contractAddress.
## Call methods
[The answers under this question](https://ethereum.stackexchange.com/questions/9733/calling-function-from-deployed-contract) reveals the method to call a function. I implemented the method mentioned in the answer post:
`bytes4(keccak256("f1(arg1_type, arg2_type)")`
[This link can get the signature encoded from function string](https://piyolab.github.io/playground/ethereum/getEncodedFunctionSignature/)
Actually, I want to call the `transfer` method in the contract, as there is no instance in this case, I use the method signature and send a transaction like the deploy process. 
## Testing
I need to know the status of the transaction, however, Geth uses Web3 with version lower than 1.0.0, and this is a known bug in the earlier version as presented in [this issue](https://github.com/ethereum/web3.js/issues/2410)
The speed is so low that I thought there are something about the statement in the console.
[npm: request has been deprecated](https://medium.com/better-programming/request-has-been-deprecated-a76415b4910b)
[How smart contract deployment works](https://programtheblockchain.com/posts/2018/01/09/how-smart-contract-deployment-works/)

[Geth docs](https://geth.ethereum.org/docs/)
[One excellent blog](https://www.zybuluo.com/qidiandasheng/note/1246984)
[How to make miner to mine only when there are Pending Transactions?](https://ethereum.stackexchange.com/questions/3151/how-to-make-miner-to-mine-only-when-there-are-pending-transactions)
