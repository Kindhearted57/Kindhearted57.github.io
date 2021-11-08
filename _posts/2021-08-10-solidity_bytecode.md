---

title: "Solidity Bytecode"
key: 2021-08-10-bytecode
pageview: true
tags: Ethereum
modify_date: 2021-08-10
sharing: true
show_author_profile: true
show_subscribe: true

---

# Solidity Bytecode

## 背景

今天在做Bytecode的CFG，对于以下的source code:

```
pragma solidity 0.4.24;

contract SimpleDAO {
  mapping (address => uint) public credit;
    
  function donate(address to) payable public{
    credit[to] += msg.value;
  }
    
  function withdraw(uint amount) public{
    if (credit[msg.sender]>= amount) {
      require(msg.sender.call.value(amount)());
      credit[msg.sender]-=amount;
    }
  }  

  function queryCredit(address to) view public returns(uint){
    return credit[to];
  }
}

```

CFG出来的图十分奇怪，如下所示：

![](https://note.youdao.com/yws/api/personal/file/WEBd21acfd141127f33e9d15f2d60d6b21b?method=download&shareKey=43a77ef460d0e700261ba1b5269cc5ab)

一开始以为是这个合约的问题，结果后来一顿操作，主要是在[ethervm.io](ethervm.io)上面看bytecode和decompilation的结果，在decompilation的结果很是奇怪，只有一个函数

```
contract Contract {
    function main() {
        memory[0x40:0x60] = 0x80;
        var var0 = msg.value;
    
        if (var0) { revert(memory[0x00:0x00]); }
    
        memory[0x00:0x0320] = code[0x20:0x0340];
        return memory[0x00:0x0320];
    }
}
```

这个结果上面提示了一句话，让我茅塞顿开，应该是bytecode的类型不对。

```
This might be constructor bytecode - to get at the deployed contract, go back and remove the constructor prefix, usually up to the next 6060 or 6080.
```

于是我换成了runtime bytecode，这次出现的结果看起来还比较合理。

![](https://note.youdao.com/yws/api/personal/file/WEB4773ed7b370acfac7a40ba7b7afa050c?method=download&shareKey=4b2c3ddcdedfef045996be8ce0c69d40)



随后我很好奇为什么这个错误会导致这样的结果，因为看起来creation bytecode和runtime bytecode的不同之处似乎只有构造函数上的不同。于是我们来比较一下这两个bytecode的不同之处。

关于不同的bytecode之间的不同之处，在之前就曾经读过[这篇medium](https://medium.com/authereum/bytecode-and-init-code-and-runtime-code-oh-my-7bcd89065904)，此处依然适用。

![](https://note.youdao.com/yws/api/personal/file/WEBb2c906cf85b18204ffb9335874d00ff4?method=download&shareKey=6c0b373dfd2462116861b38dd5b6d7f1)

在remix的compilation details中，bytecode表示creation bytecode，runtime bytecode是runtime bytecode。

这两类bytecode的关系是：creation bytecode generates runtime bytecode

下面贴一下两种bytecode:

creation bytecode:

```
608060405234801561001057600080fd5b50610320806100206000396000f300608060405260043610610061576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168062362a95146100665780632e1a7d4d1461009c57806359f1286d146100c9578063d5d44d8014610120575b600080fd5b61009a600480360381019080803573ffffffffffffffffffffffffffffffffffffffff169060200190929190505050610177565b005b3480156100a857600080fd5b506100c7600480360381019080803590602001909291905050506101c6565b005b3480156100d557600080fd5b5061010a600480360381019080803573ffffffffffffffffffffffffffffffffffffffff169060200190929190505050610294565b6040518082815260200191505060405180910390f35b34801561012c57600080fd5b50610161600480360381019080803573ffffffffffffffffffffffffffffffffffffffff1690602001909291905050506102dc565b6040518082815260200191505060405180910390f35b346000808373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254019250508190555050565b806000803373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054101515610291573373ffffffffffffffffffffffffffffffffffffffff168160405160006040518083038185875af192505050151561024457600080fd5b806000803373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600082825403925050819055505b50565b60008060008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549050919050565b600060205280600052604060002060009150905054815600a165627a7a7230582016b127a1d2c86cddf3ca28ad293153e4f03a44fcdc9378a3893b902afb848a2e0029
```

runtime bytecode

```
608060405260043610610061576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168062362a95146100665780632e1a7d4d1461009c57806359f1286d146100c9578063d5d44d8014610120575b600080fd5b61009a600480360381019080803573ffffffffffffffffffffffffffffffffffffffff169060200190929190505050610177565b005b3480156100a857600080fd5b506100c7600480360381019080803590602001909291905050506101c6565b005b3480156100d557600080fd5b5061010a600480360381019080803573ffffffffffffffffffffffffffffffffffffffff169060200190929190505050610294565b6040518082815260200191505060405180910390f35b34801561012c57600080fd5b50610161600480360381019080803573ffffffffffffffffffffffffffffffffffffffff1690602001909291905050506102dc565b6040518082815260200191505060405180910390f35b346000808373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254019250508190555050565b806000803373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002054101515610291573373ffffffffffffffffffffffffffffffffffffffff168160405160006040518083038185875af192505050151561024457600080fd5b806000803373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600082825403925050819055505b50565b60008060008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549050919050565b600060205280600052604060002060009150905054815600a165627a7a7230582016b127a1d2c86cddf3ca28ad293153e4f03a44fcdc9378a3893b902afb848a2e0029

```


