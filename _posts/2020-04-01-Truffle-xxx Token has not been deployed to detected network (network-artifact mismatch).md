---
layout: post
title: "Truffle|xxx Token has not been deployed to detected network (network/artifact mismatch)"
date: 2020-04-01
tags: Ethereum
category: blog
key:  truffle_token_deploy_error
author: Ji Ru
pageview: true
modify_date: 2020-4-28
shraring: true
show_author_profile: true
show_subscribe: true
---
# Truffle|xxx Token has not been deployed to detected network (network/artifact mismatch)
## Description
I'm testing one contract today with truffle console, and at the beginning I was running `truffle test` to test the contract. However, for some reason, I shifted to [truffle console](https://www.trufflesuite.com/docs/truffle/getting-started/interacting-with-your-contracts) method. And I got the error above.
## Solutions on line
I looked up many solutions on line, here are my finding.

* Network error. 127.0.0.1:8545(in my case there is nothing wrong with network) might be unavaliable.
* Account error. The account used hasn't been unlocked.

## My solution
It turns out the problem lies in 2\_deploy\_migration.js:
`var Ether_contract = artifacts.require("xxxToken");`
truffle get to this first, but by the time the token has not been deployed.
