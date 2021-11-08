---
title: "Blocksci: How to get the addresses of each transaction"
key: 2021-02-12-blocksci
pageview: true
tags: blocksci
modify_date: 2021-02-12
sharing: true
show_author_profile: true
show_subscribe: true
license: CC-BY-SA-4.0

---


# Blocksci: How to get the addresses of each transaction

## Background

在使用blocksci的时候遇到了一个问题，很久都没能解决。我的目的是想遍历transaction，然后针对每个transaction找到他们的输入放到一个list里面。从blocksci的文档中我看到大概是这么个思路

```
tx.inputs.address.with_type()
```
然后在with_type中输入对应的type，如果有的话就会返回对应type的地址，如果没有的话就返回NULL，文档的原话是

```
For each item: If the address has the given type return it, otherwise return None
```

我搞不懂文档上面的这段内容:

```
with_type(arg0: blocksci::AddressType::Enum) → Union[blocksci.NonstandardAddressIterator, blocksci.PubkeyAddressIterator, blocksci.PubkeyHashAddressIterator, blocksci.MultisigPubkeyIterator, blocksci.ScriptHashAddressIterator, blocksci.MultisigAddressIterator, blocksci.OpReturnIterator, blocksci.WitnessPubkeyHashAddressIterator, blocksci.WitnessScriptHashAddressIterator, blocksci.WitnessUnknownAddressIterator]
```

甚至到[stackexchange](https://bitcoin.stackexchange.com/questions/101990/how-to-get-input-and-output-address-in-a-transaction-with-blocksci)的bitcoin分版上面去提了个问题。

我的思路是这样，我了解到这个是python中一个叫[typing](https://docs.python.org/zh-cn/3/library/typing.html)的东西。所以大概Union里面的东西就是列举了一些可能的type，那我随便拿一个过来试试：

`result =tx.inputs.address.with_type(blocksci.NonstandardAddressIterator)`

输出：

```
Traceback (most recent call last):
  File "data_adjustment.py", line 10, in <module>
    result =tx.inputs.address.with_type(blocksci.NonstandardAddressIterator)
  File "/d1/BlockSci/blockscipy/blocksci/__init__.py", line 575, in method
    return apply_map(rng._self_proxy, getattr(rng._self_proxy.nested_proxy, name)(*args))(rng)
TypeError: with_type(): incompatible function arguments. The following argument types are supported:
    1. (self: blocksci.proxy.AddressProxy, arg0: blocksci::AddressType::Enum) -> Union[blocksci.proxy.OptionalNonstandardAddressProxy, blocksci.proxy.OptionalPubkeyAddressProxy, blocksci.proxy.OptionalPubkeyHashAddressProxy, blocksci.proxy.OptionalMultisigPubkeyProxy, blocksci.proxy.OptionalScriptHashAddressProxy, blocksci.proxy.OptionalMultisigAddressProxy, blocksci.proxy.OptionalOpReturnProxy, blocksci.proxy.OptionalWitnessPubkeyHashAddressProxy, blocksci.proxy.OptionalWitnessScriptHashAddressProxy, blocksci.proxy.OptionalWitnessUnknownAddressProxy]

Invoked with: <blocksci.proxy.AddressProxy object at 0x7fdcd04326f0>, <class 'blocksci.NonstandardAddressIterator'>


```
这个proxy又是什么东西啊！！！我晕了。

痛定思痛胡乱找了好久之后我发现这个blocksci是用c++写的，然后搜了一下

```
TypeError: with_type(): incompatible function arguments. The following argument types are supported:
```
发现[这个回答](https://stackoverflow.com/questions/45589229/error-incompatible-function-arguments)可能会有点帮助，至少可以了解一些cython的东西。

于是我去简单了解了一点pybind11，从[docs](https://pybind11.readthedocs.io/en/stable/basics.html)里面使用了一些例子

```
c++ -O3 -Wall -shared -std=c++11 -undefined dynamic_lookup `python3 -m pybind11 --includes` example.cpp -o example`python3-config --extension-suffix`
```


复习 cpp的[lambda]()，[template](https://www.geeksforgeeks.org/templates-cpp/)

最后可以说是换了一条路解决了这个问题，并没有弄明白with_type是怎么回事，而是曲线救国了一波。最终调用地址的方式:

```
for block in tqdm(chain):
    for tx in block:
        inputs = tx.inputs.address.to_list()
        outputs = tx.outputs.address.to_list()
        
```
## 另外
在后面使用blocksci的时候还遇到了一个大问题，这个问题直接导致了我最后就没有用blocksci。在GitHub上面提交了[issue](https://github.com/citp/BlockSci/issues/452)，但是显然现在blocksci已经没有在维护了。

总结，目前的一些community上的工具还不是很成熟，特别是在需要操作全节点的时候更是需要慎重，或许可以先稍微同步一部分的数据，然后先试试自己需要的功能是否能够实现。

