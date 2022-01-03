# Diem & Move初探

[diem初探](https://github.com/diem/diem)

1 clone the diem core

`git clone https://github.com/diem/diem.git && cd diem`

2 checkout the testnet branch

`git checkout testnet`

3 install dependencies to setup diem core

`./scripts/dev_setup.sh`


4 build diem cli client and connect to the testnet

`./scripts/cli/start_cli_testnet.sh`

Diem Payment Network(DPN) supports accounts created for Regulated Virtual Asset Service Providers Regulated VASP and Designated Dealers



sccache

```
./scripts/cli/start_cli_testnet.sh
```


```
Building and running client in debug mode.
    Updating git repository `https://github.com/diem/diem-devtools`
warning: spurious network error (2 tries remaining): SecureTransport error: connection closed via error; class=Net (12)
warning: spurious network error (1 tries remaining): SecureTransport error: connection closed via error; class=Net (12)
error: failed to get `nextest-runner` as a dependency of package `x v0.1.0 (/Users/jiru/Desktop/Diem/diem/devtools/x)`

Caused by:
  failed to load source for dependency `nextest-runner`

Caused by:
  Unable to update https://github.com/diem/diem-devtools?rev=1b61853016059b2dca577cefafff2825b49494a0#1b618530

Caused by:
  failed to fetch into: /Users/jiru/.cargo/git/db/diem-devtools-a0e22da56a82fb23

Caused by:
  network failure seems to have happened
  if a proxy or similar is necessary `net.git-fetch-with-cli` may help here
  https://doc.rust-lang.org/cargo/reference/config.html#netgit-fetch-with-cli

Caused by:
  SecureTransport error: connection closed via error; class=Net (12)
```

可能是因为源的问题，没办法拉取Rust库，于是[参考](https://wkwkk.com/articles/b1c6cf4f1fc959f4.html)这篇操作了一下。解决了问题。

在cargo的config文件`~/.cargo/config`

添加内容

```
[source.cartes-io]
replace=with = 'ustc'

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

在后面使用Rust的时候出现问题

```
SSL peer certificate or SSH remote key was not OK (SSL certificate problem: certificate has expired)
```

在这个时候将地址更改为
`registry="https://mirrors.ustc.edu.cn/crates.io-index"`

最后解决的方式，首先锁定这是一个ssl的问题，然后找到/etc/ssl/cert.pem，（参考了[这个](https://stackoverflow.com/questions/69387175/git-for-windows-ssl-certificate-problem-certificate-has-expired)stackoverflow）发现我这个文件里面叠了好多层，但是其中的一些cert过期了。然后现在需要给过期的部分处理一下。把过期的删掉就解决了问题，没有深入探究原因。
[另外一篇参考文献](https://cloud.tencent.com/developer/news/645218)




## Move prover user guide

### Move 语言学习
Move有两种program modules和scripts 

Move cli的下载

`cargo install --git https://github.com/diem/diem move-cli`

在后面使用`move publish src/modules`的时候报错：

```
error: Found argument 'publish' which wasn't expected, or isn't valid in this context

USAGE:
    move [FLAGS] [OPTIONS] <SUBCOMMAND>

For more information try --help

```
还是文档中，发现了原因：

```
On macOS and Linux this is usually ~/.cargo/bin. You'll want to make sure this location is in your PATH environment variable.
```

在做这个的时候把zsh弄坏了，直接所有command not found，原因是我写了一句
`export PATH = ~/.cargo/bin`

正确的做法应该是

`export PATH=~/.cargo/bin:$PATH`
prover的installation

`./scripts/dev_setup.sh -yp`

`~/.profile`

`cargo build`

在Diem tree中，`cargo run`可以调用prover

把alias加入到bash中，我用的是zsh，就在`~/.zshrc`
`alias mvp="cargo run --release --quiet --package move-prover --"`

其中`--release`可以看作一个编译时间和验证时间的权衡。release模式下的验证时间可以比debug模式下快20倍。



```
mvp --dependency . source.move
# 或者可以简写
mvp -d . source.move
```

或者也可以把dependencies写在config里面，让prover到写好的路径下面去找对应的库。

```
> echo "move_deps = [\"<path-to-diem>/language/diem-framework/modules\"]" > ~/.mvprc
> export MOVE_PROVER_CONFIG=~/.mvprc
```

#### Reference 总结


### Prover test

test文件在diem/language/move_prover/tests

```
> echo "move_deps = [\"<path-to-diem>/language/diem-framework/modules\"]" > ~/.mvprc
> export MOVE_PROVER_CONFIG=~/.mvprc
```

Question: 如何在这里写两个路径?? 
Unable to solve this..


boogie problem

`No boogie executable set.  Please set BOOGIE_EXE`

一开始以为缺了个boogie，去.Net搞了一圈，（过程中找到了一篇比较有用的[东西](https://www.wuliaole.com/2017/04/13/dotnetcore_on_mac_os/)，或许以后可以用到）之后发现是在安装move prover的时候已经下载好了，参考[这里](https://github.com/diem/diem/blob/7bd61c3273be3acb39fa31f11ce261d9c97dbd8e/language/move-prover/doc/user/install.md)，但是需要在使用之前`. ~/.profile`



## Move Specification Language


Basic knowledge for Move -> How Move prover tool work -> basic principles of pre/post condition specifications [read this](https://en.wikipedia.org/wiki/Design_by_contract) -> examples of specifications

## Move White paper

[Move White paper](https://developers.diem.com/docs/technical-papers/move-paper/)
## Rust 学习

[Learn](https://www.rust-lang.org/learn)

Rust的中文资源少之又少，在这里面做个rust笔记。

### hashmap

重新插入值会造成覆盖

```

#![allow(unused)]
fn main() {
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);

println!("{:?}", scores);
}

```
代码[参考](https://kaisery.github.io/trpl-zh-cn/ch08-03-hash-maps.html)