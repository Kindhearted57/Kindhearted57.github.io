---
title: "Bitcoin address"
key: 2021-02-12-bitcoin-address
pageview: true
tags: bitcoin
modify_date: 2021-02-12
sharing: true
show_author_profile: true
show_subscribe: true
license: CC-BY-SA-4.0

---

# Bitcoin address 分类初探

## Background



今天在研究blocksci的时候，发现了一个tag叫EquivAddress，有两种address equivalence: type equivalence, script equivalence。

* Type Equivalence

1) Pay to pubkey

2) Pay to pubkeyhash 

3) [pay to witness pubkey hash](https://medium.com/getamis/深入了解-segregated-witness-segwit-db5d98bb534c)

require knowledge of a given pubkey


pay to scripthash, pay to witness scripthash

require knowledge of a given spending script

* Script Equivalence

[Multisig address](https://bitcoin.stackexchange.com/questions/6100/how-will-multisig-addresses-work)


还有一种情况被列成non standard，其中的头号代表就是
[一个例子](https://www.blockchain.com/btc/tx/e411dbebd2f7d64dafeef9b14b5c59ec60c36779d43f850e5e347abee1e1a455)

于是了解了一下为什么会出现`Unable to decode output address`这个问题。参考[stackexchange](https://bitcoin.stackexchange.com/questions/64504/what-does-unable-to-decode-output-address-mean)上面的一个回答。

