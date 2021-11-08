---

title: "Python Bytecode"
key: 2021-08-16-bytecode
pageview: true
tags: python
modify_date: 2021-08-16
sharing: true
show_author_profile: true
show_subscribe: true

---

# Bytearray.fromhex()
```
str = '5b8282810180911115909157600080fd'
bytearray.fromhex(str)
```

```
bytearray(b'[\x82\x82\x81\x01\x80\x91\x11\x15\x90\x91W`\x00\x80\xfd')
```

发现变成bytearray类型之后少了一些字符，比如开头的`5b`，[参考](https://segmentfault.com/q/1010000004593411)这个回答发现，是`\x5b`对应的字符`[`显示了出来，但是明明`\x82`对应的是字母`R`，为什么没有显示出来呢。