---
weight: 1
bookFlatSection: true
draft: true
title: "以太坊公链开发"
---

# poa
poa测试
双节点 
21节点
搭建使用配置文件的
搭建不使用配置文件的

## 05-18 poa 初步
* geth 项目环境配置 ok 
* 编译 ok
* poa 通过配置文件初步搭建 ok
* 测试 ok

* [poa初步](https://github.com/xiaoping378/blog/blob/master/posts/%E4%BB%A5%E5%A4%AA%E5%9D%8A-%E7%A7%81%E6%9C%89%E9%93%BE%E6%90%AD%E5%BB%BA%E5%88%9D%E6%AD%A5%E5%AE%9E%E8%B7%B5.md)

* [以太坊私有链搭建指南](https://g2ex.top/2017/09/12/ethereum-guidance/)


## 05-19 回顾bitcoin

https://jeiwan.net/posts/building-blockchain-in-go-part-1/
https://blog.jse.li/posts/torrent/


## 05-20 计划
[挖矿和共识算法](https://knarfeh.com/2018/03/10/go-ethereum%20%E6%BA%90%E7%A0%81%E7%AC%94%E8%AE%B0%EF%BC%88miner,%20consensus%20%E6%A8%A1%E5%9D%97-%E6%8C%96%E7%9F%BF%E5%92%8C%E5%85%B1%E8%AF%86%E7%AE%97%E6%B3%95%EF%BC%89/)

[共识算法带流程图](https://blog.csdn.net/metal1/article/details/79682636)

[以太坊源码P2P网络及节点发现机制](https://blog.csdn.net/Metal1/article/details/80442124)

[bootnode](https://blog.csdn.net/DDFFR/article/details/78925410)


## 05-21 
看如何修改源码

* 奖励部分 
     poa没有奖励
     待确定
     https://github.com/ethereum/EIPs/issues/225
* 配置部分
    params config文件
    看看怎么改
    要确认是不需要配置文件还是不需要初始配置
    params/bootnodes.go p2p中心节点
    params/config.go 主网区块和共识配置
    剩余 账号配置
    默认的genesis 配置在 core/genesis.go里 
    eth/ethconfig.go ???
    


[Setup your own private Proof-of-Authority Ethereum network with Geth](https://hackernoon.com/setup-your-own-private-proof-of-authority-ethereum-network-with-geth-9a0a3750cda8)

[Private Clique network, where can I get block sealer and block reward?](https://ethereum.stackexchange.com/questions/78535/private-clique-network-where-can-i-get-block-sealer-and-block-reward)

* 学习资源
https://learnblockchain.cn/
https://www.8btc.com/
https://blog.csdn.net/Metal1/article/details/80070231?spm=1001.2014.3001.5502
http://www.wjblog.top/categories/%E4%BB%A5%E5%A4%AA%E5%9D%8A/page/4/

* 讨论区块链浏览器
https://blockscout.com/poa/core/

* 以太坊启动流程
https://zhuanlan.zhihu.com/p/343576151  eth/ethconfig.go ???



## 05-24
以太坊生态defi套利资料

### What Is an Automated Market Maker (AMM)?
* 什么是amm
* amm如何工作
* 什么是流量池

* [What Is an Automated Market Maker (AMM)](https://academy.binance.com/en/articles/what-is-an-automated-market-maker-amm)


### MEV and Me
* 什么是mev
* 原理是什么
* 危害和展望

* [MEV and Me](https://research.paradigm.xyz/MEV)
* [mev中文版](https://www.ethereum.cn/Thinking/MEV)




##  poa 高级



# 合约开发 uniswap


# dapp


## https://etherscan.io/






