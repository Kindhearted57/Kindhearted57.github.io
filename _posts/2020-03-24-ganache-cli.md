---
layout: post
title: ganache-cli no space left on device
date: 2020-03-24
categories:
  - blog
description:A ganache error in ubuntu
image: https://note.youdao.com/yws/api/personal/file/WEB5986828890383f08c9e80ac68f1bb750?method=download&shareKey=74123af89e050b12894d7079c6db5edc
image-sm: https://note.youdao.com/yws/api/personal/file/WEB5986828890383f08c9e80ac68f1bb750?method=download&shareKey=74123af89e050b12894d7079c6db5edc
---
#Background
I'm running some test on ubuntu 18.04, there are thousands of contracts to be deployed. All of them follows the same logic like this: use account0 to transfer some amount of tokens to account1, then.....
So sometimes account0 will run out of eth and here is the error:
However, this time this error appears.
When I look into the tmp file:
