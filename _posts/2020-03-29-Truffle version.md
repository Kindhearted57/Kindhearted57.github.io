---
layout: post
title: "Truffle version"
key: 2020-03-29-truffle-version
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---
# Truffle version
## Background 
Met with one problem today, when using truffle, there are some conflicts with the solc version.

Migration.sol `pragma solidity >=0.4.21 <0.6.0;`
Contract_code.sol `pragma solidity 0.4.19;`  
solc version `0.4.24`  

truffle-config.js

```
module.exports = {networks:  
{development: 
{host: "127.0.0.1", 
port: 8545, 
network_id: "*", }},
compilers: {solc: 
{version: "0.4.24", }}}
```  
However, when I run `truffle migrate`, here is the result I got.  

```
Error: Truffle is currently using solc 0.4.24, but one or more of your contracts specify "pragma solidity 0.4.19".
Please update your truffle config or pragma statement(s).
(See https://truffleframework.com/docs/truffle/reference/configuration#compiler-configuration for information on
configuring Truffle to use a specific solc compiler version.)
```
I noticed there is also another error shown, which says:  

```
Error: CompileError: /home/jiru/EEthereum/contract_code/assdbs_ASDCBA/contracts/assdbs_ASDCBA.sol:13:1: ParserError: Source file requires different compiler version (current compiler is 0.5.12+commit.7709ece9.Linux.g++ - note that nightly builds are considered to be strictly less than the released version
pragma solidity 0.4.19;
^---------------------^

```
I tried multiple ways:

* Change the version in truffle-config.js to ^0.4.24(which means >=0.4.24).
  
* Change the version in migration.sol to `pragma solidity >=0.4.19 <0.6.0;`  
* Change the version in truffle-config.js to 0.4.21.  

The first two methods returned the same error. The last one returned 

```
/home/jiru/EEthereum/contract_code/assdbs_ASDCBA/contracts/Migrations.sol:7:14: ParserError: Expected identifier, got 'LParen'
  constructor() public {

```   
But none of these worked. However, when I tried to change the version in contract_name.sol to ^0.4.19, it worked.  
Now, here are all the settings:
Migration.sol `pragma solidity >=0.4.21`
