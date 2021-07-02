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


### goquorum
* 本地部署测试
    查看文档  大略看完
    上k8 docker

* 安装node.js
    安装工具箱 ok
    安装完成后 
        尝试 quickstart
        手动修复三个执行文件


## 05-25 goquorum

### quorum-wizard
* quickstart ok
    raft可以跑起来 
    可以跑起来cakeshop 
    节点功能ok
    需要看看quorum的架构
* Simple Network 
    download istanbul 
* custom docker
    brew install kubectl 
    install docker desktop
    download istanbul 

* custom kubectl
    not work

https://docs.blockscout.com/for-users/smart-contract-interaction/verifying-a-smart-contract


## 05-26 
* 钱包测试
    钱包链接 ok
    添加账户 ok
    添加原有账户 ok ?
    转账 ok
        金额待确认 ok
        > var tx = {from: "0xed9d02e382b34818e88b88a309c7fe71e65f419d", to: "0x1F207Af72a8D2E27f76626A840F11C824a438207", value: web3.toWei(1, "ether")}

        //ec2 
        var tx = {from: "0xed9d02e382b34818e88b88a309c7fe71e65f419d", to: "0xC5188375D8600A05e0d10a4bc2d6C1F5E8cFE1C5", value: web3.toWei(1000000, "ether")}
        undefined

        //local
        var tx = {from: "0xed9d02e382b34818e88b88a309c7fe71e65f419d", to: "0x9825683d08bb898e086211dDd8c78403373bE7f2", value: web3.toWei(1000000, "ether")}
        undefined


        //cto
        var tx = {from: "0xed9d02e382b34818e88b88a309c7fe71e65f419d", to: "0x05090dEe912116f352a7d87e2BD9b7A9ac190210", value: web3.toWei(1000, "ether")}
        undefined


        var tx = {from: "0xed9d02e382b34818e88b88a309c7fe71e65f419d", to: "0x38584f649428ae7dBbF51D52F6f0C949Cab99076", value: web3.toWei(1000, "ether")}
        undefined
        

        0xed9d02e382b34818e88b88a309c7fe71e65f419d
        0x4234273A066B9Ec5b1C8bFfB8361765705f6C186


        > personal.sendTransaction(tx, "")
        "0x0d3a74dc9f0b7daf5c25ae46c666154a514dd3315b5730d3496b73d8887a379a"
        https://geth.ethereum.org/docs/rpc/ns-personal

        [How to import account form geth console to metamask (private blockchain)](https://ethereum.stackexchange.com/questions/45050/how-to-import-account-form-geth-console-to-metamask-private-blockchain)

* 浏览器测试
    安装环境
        https://docs.blockscout.com/for-developers/information-and-settings/requirements
        https://blog.csdn.net/weixin_38652136/article/details/105724016
        postgres https://www.codementor.io/@engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb
    环境变量配置
* 合约测试
* 共识的算法


### blockscout 
*  项目地址 https://github.com/blockscout/blockscout
[50+ Projects Building on J.P. Morgan’s Quorum Blockchain](https://medium.com/@mateo_ventures/heres-who-s-building-on-quorum-see-the-list-b18d65aa0a2c)




# 合约开发 uniswap


# dapp


## https://etherscan.io/



## 后续工作
已完成 
* 本地的主链运行 
* 转账测试 
* 容器测试（docker） (k8待完成)

待完成
* 浏览器本地部署测试 （可以配置logo）
    环境搭建
        数据库
        配置文件
        语言环境
        框架环境
    交易测试
        测试交易可正确显示 参数
    发币测试
        测试token显示及交易
    账户测试
        显示账户余额与交易统计
    合约测试
        可显示合约代码 交易


* 浏览器线上部署测试
    域名购买 配置
    ssl
    不建议使用阿里云
        需要备案 审查
    两种方案  主链推荐的
    另一个比较好看的浏览器 
        三种方案
            自部署 可修改logo
            团队提供

* 主链线上部署测试
    是否需要其他业务部门或者合作方加入 节点
    7个节点 （可商议）
    节点放置地点（例如 香港 韩国 日本 新加坡 欧洲 美国）
    共识选择（可商议）
    监控 （性能 稳定性）
    容器 （稳定性）
    容灾 （节点）
    服务器配置 （需要知道可能出现的数据量）

* swap开发 合约与前后端

    发币
    测试添加交易对
    测试流动性
    测试兑换
    解决token的icon

    域名
    前端界面
    部署
    token icon解决方案

    界面与合约的对接

## 05-27
沟通进度
浏览器本地


## 05-28
* actor
[The actor model in 10 minutes](https://www.brianstorti.com/the-actor-model/)
[十分钟理解Actor模式](https://blog.51cto.com/nxlhero/1666250)
* erlang
[Introduction to Erlang](https://serokell.io/blog/introduction-to-erlang)
* elixir
[Learn Elixir: The Ultimate Guide](https://serokell.io/blog/learn-elixir)
[8 Companies That Use Elixir in Production](https://serokell.io/blog/elixir-companies)
* Phoenix
[Up and Running](https://hexdocs.pm/phoenix/up_and_running.html)
[Installation](https://hexdocs.pm/phoenix/installation.html)
    [ERROR in Cannot find module 'node-sass'](https://stackoverflow.com/questions/48147896/error-in-cannot-find-module-node-sass)
    [npm使用国内淘宝镜像](https://blog.csdn.net/starzhou/article/details/105469268)
    [npm 淘宝镜像的安装](https://zhuanlan.zhihu.com/p/166175735)

## 05-31
* Phoenix run  //ok
    npm
        [node-sass 安装失败的各种坑](https://www.jianshu.com/p/92afe92db99f)
        擦 使用稳定版的nodejs就好了？ 日
        [node-sass安装失败番外篇](https://blog.csdn.net/palmer_kai/article/details/110142505)

    postgres
        [psql: FATAL: role “postgres” does not exist](https://stackoverflow.com/questions/15301826/psql-fatal-role-postgres-does-not-exist)

* blockscout
    拉取源码：
    git clone https://github.com/poanetwork/blockscout
    cd blockscout

    mix phx.gen.secret
    [(Mix) Could not compile dependency :keccakf1600 #60](https://github.com/blockscout/blockscout/issues/604)


## 06-01
* asdf
    [asdf](https://asdf-vm.com/#/)
    [Installing Erlang and Elixir with asdf](https://www.mitchellhanberg.com/post/2017/10/05/installing-erlang-and-elixir-using-asdf/)
    rV8dgqpE6bRD6rwfdhDmNls+ZEOBWopHQMSuxwCdNcRlfUs805DsY9lscaqQaqVW

* 数据库环境 //ok
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres

    export SECRET_KEY_BASE="Yr4+vBDj3du35i12Tzvg4AUix8adBPpzIpbe89MUVZNxLnfI9Tp/Ykle/Pa3R0AM"

    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="https://mainnet.infura.io/v3/bb5bf63ee4454829987724ec4cebd8b6"
    export ETHEREUM_JSONRPC_WS_URL="wss://mainnet.infura.io/ws/v3/bb5bf63ee4454829987724ec4cebd8b6"

    export SUBNETWORK= MAINNET

    export PORT=4000
    export COIN="Test Coin"


* 测试quo //ok
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres


    //local 
    export SECRET_KEY_BASE="rV8dgqpE6bRD6rwfdhDmNls+ZEOBWopHQMSuxwCdNcRlfUs805DsY9lscaqQaqVW"  

    //ec2   nt5FIoq0HGW/YzC4T+KhrNgCpUYPtNKpAcfWvPCURhJ8qbRLIsPcIxtTzGICnrhi

    export SECRET_KEY_BASE="nt5FIoq0HGW/YzC4T+KhrNgCpUYPtNKpAcfWvPCURhJ8qbRLIsPcIxtTzGICnrhi"  


    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="http://localhost:22000"
    export ETHEREUM_JSONRPC_WS_URL="ws://localhost:23000"

    export PORT=4000
    export COIN=POA
    export NETWORK=POA


## 06-02


## 06-04
[Transactions not updated, failed to fetch internal transactions #1231](https://github.com/blockscout/blockscout/issues/1231)

 mix do ecto.drop, ecto.create, ecto.migrate            

 [GoQuorum Wizard](https://docs.goquorum.consensys.net/en/stable/HowTo/GetStarted/Wizard/GettingStarted/)
 [CentOS7手动部署BlockScout开源区块浏览器](https://blog.csdn.net/weixin_38652136/article/details/105724016)
 [Getting Started with PostgreSQL on Mac OSX](https://www.codementor.io/@engineerapart/getting-started-with-postgresql-on-mac-osx-are8jcopb)
 [phoenix](https://hexdocs.pm/phoenix/controllers.html#content)
 [Gigalixir](https://gigalixir.readthedocs.io/en/latest/intro.html)
 [BlockScout 浏览器搭建教程](https://www.jianshu.com/p/40bbc588058f)
 [Installing Erlang and Elixir with asdf](https://www.mitchellhanberg.com/post/2017/10/05/installing-erlang-and-elixir-using-asdf/)
 [Ethereum 源码解析 1/3](https://zhuanlan.zhihu.com/p/343576151)


 ## 06-07
 * aws 账号
    注册
    绑定卡
    选择配置
    测试
 * 换房。。。


 ## 06-08
 * aws 账号 审核


 ## 06-09
 * 单实例部署
    启动ec2香港
    ssh pem
    https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html


    ubuntu add sudo user
    https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart
    不需要新的账户 
    https://aws.amazon.com/cn/premiumsupport/knowledge-center/new-user-accounts-linux-instance/


* docker
    install docker on ubuntu
    https://docs.docker.com/engine/install/ubuntu/

    install docker compose
    https://docs.docker.com/compose/install/

* quorum quick-wizard
    install nodejs
    https://github.com/nodesource/distributions/blob/master/README.md#debinstall
    install quorum-wizard
    https://docs.goquorum.consensys.net/en/stable/HowTo/GetStarted/Wizard/GettingStarted/

* 共识
    共识算法系列：PBFT算法关键点综述、优缺点总结
    https://zhuanlan.zhihu.com/p/53897982
    Istanbul BFT (IBFT)
    https://medium.com/getamis/istanbul-bft-ibft-c2758b7fe6ff
    IBFT Consensus Overview
    https://docs.goquorum.consensys.net/en/stable/Concepts/Consensus/IBFT/
    共识算法系列之一：raft和pbft算法
    https://zhuanlan.zhihu.com/p/35847127

* cakeshop
    install jdk
    https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-20-04

* 晚上回去把相关内容技术过一遍


 ## 06-10
 * chain 
    启动simple实例
    测试钱包

* erlang 
    master class
    https://www.youtube.com/watch?v=gtSOqxvoK0c&list=PLR812eVbehlwEArT3Bv3UfcM9wR3AEZb5&index=3

* asdf
    sudo apt install curl git

 ## 06-11
 * 安装erlang elixir //ok
 * 安装phonix //ok
 * 安装数据库 //ok
    How To Install and Use PostgreSQL on Ubuntu 20.04
    https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04
    sudo -i -u postgres
    修改密码为123456
    ALTER USER postgres WITH PASSWORD 'postgres';
    需要确认密码的修改
 * 跑起 Phoenix hello //ok
 * 安装剩余依赖
 sudo apt-get install libgmp-dev
 sudo apt install build-essential


* 环境ok  等待部署

* 环境问题还是需要asdf解决
    sudo apt-get install dirmngr
    https://www.mashupgarage.com/playbook/setup/asdf.html
    所需的依赖
    https://docs.riak.com/riak/ts/1.5.2/setup/installing/source/erlang.1.html#debian-ubuntu-prerequisites

* Introduction to Deployment
    https://hexdocs.pm/phoenix/deployment.html#content


## 06 15-18
* 本周
    浏览器生产级别部署 //ok
    修改浏览器 //ok了一部分
    erc20测试
    使用讲解

## 06-15 修改浏览器
* 启动
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres
    export SECRET_KEY_BASE="rV8dgqpE6bRD6rwfdhDmNls+ZEOBWopHQMSuxwCdNcRlfUs805DsY9lscaqQaqVW"  

    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="http://localhost:22000"
    export ETHEREUM_JSONRPC_WS_URL="ws://localhost:23000"

    export PORT=4000
    export COIN=POA
    export NETWORK=POA

    mix do ecto.drop, ecto.create, ecto.migrate

* 修改css 或者直接修改html //ok
    直接修改html
    需要修改css的改为嵌入式 //ok

* 找到导航对应的菜单修改菜单
    添加分支 //ok 


    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres
    export SECRET_KEY_BASE="nt5FIoq0HGW/YzC4T+KhrNgCpUYPtNKpAcfWvPCURhJ8qbRLIsPcIxtTzGICnrhi"   

    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="http://localhost:22000"
    export ETHEREUM_JSONRPC_WS_URL="ws://localhost:23000"

    export PORT=4000
    export COIN=POA
    export NETWORK=POA


## 06-16 

* 修改logo //ok
    export LOGO="/images/abel2.svg"

* hello 生产级别部署 //ok
    mix phx.gen.secret
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/hello_dev"
    export SECRET_KEY_BASE=/yE0cGYqnYf5l/rYuFlv78vAavCmupiptkInvvvd0Hr4o04VaIrDKzUYECvt5jjL

    # Initial setup
    $ mix deps.get --only prod
    $ MIX_ENV=prod mix compile
    MIX_ENV=prod mix do deps.get, local.rebar --force, deps.compile, compile

    # Compile assets
    $ npm run deploy --prefix ./assets
    $ mix phx.digest

    # Custom tasks (like DB migrations)
    $ MIX_ENV=prod mix ecto.migrate
    MIX_ENV=prod mix do ecto.create, ecto.migrate
    MIX_ENV=prod mix do ecto.drop, ecto.create, ecto.migrate

    # Finally run the server
    $ PORT=4001 MIX_ENV=prod mix phx.server //pgrep beam pkill beam
    PORT=4001 MIX_ENV=prod iex -S mix phx.server
    PORT=4001 MIX_ENV=prod elixir --erl "-detached" -S mix phx.server

* local explorer production test
    MIX_ENV=prod mix phx.gen.cert blockscout blockscout.local

    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout_dev"
    export SECRET_KEY_BASE="hqz0V4sHn9qi3RfZfKYSV8CNesItM6QMkZNfVHDH7lR1gMkKon+nKlnFHoECUXHc"


    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres
    export SECRET_KEY_BASE="Nr3v5xbfyL94dvFzpVPAeL6QKiNT398a3MS5NXREzdvLwIZkHzJ7I2vi/+iB0tj8"

    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="http://localhost:22000"
    export ETHEREUM_JSONRPC_WS_URL="ws://localhost:23000"

    export PORT=4000
    export COIN=POA
    export NETWORK=POA


## 06-17
* erc20 合约部署
    本地测试    ok
    
    主链部署的话需要支持web3

    可以部署并且查询

* 本地测试
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout_dev"
    export SECRET_KEY_BASE="H5X12JEt2lwAh/1TwKGFL4nmDSWpca98axU3iWlG3eRnw38HgAbTgxg0PssL/eAc"


    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres
    export SECRET_KEY_BASE="Yr4+vBDj3du35i12Tzvg4AUix8adBPpzIpbe89MUVZNxLnfI9Tp/Ykle/Pa3R0AM"

    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="http://localhost:22000"
    export ETHEREUM_JSONRPC_WS_URL="ws://localhost:23000"

    export PORT=4000
    export COIN=POA
    export NETWORK=POA

    export SUBNETWORK=Abel






    ```erc20
    pragma solidity ^0.4.21;

import "./EIP20Interface.sol";


contract EIP20 is EIP20Interface {

    uint256 constant private MAX_UINT256 = 2**256 - 1;
    mapping (address => uint256) public balances;
    mapping (address => mapping (address => uint256)) public allowed;
    /*
    NOTE:
    The following variables are OPTIONAL vanities. One does not have to include them.
    They allow one to customise the token contract & in no way influences the core functionality.
    Some wallets/interfaces might not even bother to look at this information.
    */
    string public name;                   //fancy name: eg Simon Bucks
    uint8 public decimals;                //How many decimals to show.
    string public symbol;                 //An identifier: eg SBX

    function EIP20(
        uint256 _initialAmount,
        string _tokenName,
        uint8 _decimalUnits,
        string _tokenSymbol
    ) public {
        balances[msg.sender] = _initialAmount;               // Give the creator all initial tokens
        totalSupply = _initialAmount;                        // Update total supply
        name = _tokenName;                                   // Set the name for display purposes
        decimals = _decimalUnits;                            // Amount of decimals for display purposes
        symbol = _tokenSymbol;                               // Set the symbol for display purposes
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balances[msg.sender] >= _value);
        balances[msg.sender] -= _value;
        balances[_to] += _value;
        emit Transfer(msg.sender, _to, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        uint256 allowance = allowed[_from][msg.sender];
        require(balances[_from] >= _value && allowance >= _value);
        balances[_to] += _value;
        balances[_from] -= _value;
        if (allowance < MAX_UINT256) {
            allowed[_from][msg.sender] -= _value;
        }
        emit Transfer(_from, _to, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    function balanceOf(address _owner) public view returns (uint256 balance) {
        return balances[_owner];
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowed[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value); //solhint-disable-line indent, no-unused-vars
        return true;
    }

    function allowance(address _owner, address _spender) public view returns (uint256 remaining) {
        return allowed[_owner][_spender];
    }
}
    ```

    ```
    pragma solidity ^0.4.21;


contract EIP20Interface {
    /* This is a slight change to the ERC20 base standard.
    function totalSupply() constant returns (uint256 supply);
    is replaced with:
    uint256 public totalSupply;
    This automatically creates a getter function for the totalSupply.
    This is moved to the base contract since public getter functions are not
    currently recognised as an implementation of the matching abstract
    function by the compiler.
    */
    /// total amount of tokens
    uint256 public totalSupply;

    /// @param _owner The address from which the balance will be retrieved
    /// @return The balance
    function balanceOf(address _owner) public view returns (uint256 balance);

    /// @notice send `_value` token to `_to` from `msg.sender`
    /// @param _to The address of the recipient
    /// @param _value The amount of token to be transferred
    /// @return Whether the transfer was successful or not
    function transfer(address _to, uint256 _value) public returns (bool success);

    /// @notice send `_value` token to `_to` from `_from` on the condition it is approved by `_from`
    /// @param _from The address of the sender
    /// @param _to The address of the recipient
    /// @param _value The amount of token to be transferred
    /// @return Whether the transfer was successful or not
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success);

    /// @notice `msg.sender` approves `_spender` to spend `_value` tokens
    /// @param _spender The address of the account able to transfer the tokens
    /// @param _value The amount of tokens to be approved for transfer
    /// @return Whether the approval was successful or not
    function approve(address _spender, uint256 _value) public returns (bool success);

    /// @param _owner The address of the account owning tokens
    /// @param _spender The address of the account able to transfer the tokens
    /// @return Amount of remaining tokens allowed to spent
    function allowance(address _owner, address _spender) public view returns (uint256 remaining);

    // solhint-disable-next-line no-simple-event-func-name
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);
}
    ```


## 06-18
* 浏览器讲解

1、显示区块，交易，合约
2、执行合约交易
3、查看账户内容

* 浏览器标题

## ec2
Ae8GwpETvVFq3c90mLI1AY/7QtZvuyt9VtEp+eA8MBCuRzZ5knzvznMAfJHEqIlj

    export LOGO="/images/abel2.svg"
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    export SECRET_KEY_BASE="Ae8GwpETvVFq3c90mLI1AY/7QtZvuyt9VtEp+eA8MBCuRzZ5knzvznMAfJHEqIlj"

    export LOGO="/images/abel2.svg"
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres
    export SECRET_KEY_BASE="Ae8GwpETvVFq3c90mLI1AY/7QtZvuyt9VtEp+eA8MBCuRzZ5knzvznMAfJHEqIlj"

    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="http://localhost:22000"
    export ETHEREUM_JSONRPC_WS_URL="ws://localhost:23000"

    export PORT=4000
    export COIN=Abel 
    export NETWORK=Ethereum 

    export SUBNETWORK=Abel
    export BLOCK_TRANSFORMER=clique

    // 根据CoinGecko 上的价格生成k图 https://www.coingecko.com/zh
    //Ethereum or poa


## 06-21
* miner编号？
    https://docs.blockscout.com/for-developers/information-and-settings/add-validator-metadata

pragma solidity ^0.4.18;


/**
 * @title EternalStorage
 * @dev This contract holds all the necessary state variables to carry out the storage of any contract
 * and to support the upgrade functionality.
 */
contract EternalStorage {

    // Version number of the current implementation
    uint256 internal _version;

    // Address of the current implementation
    address internal _implementation;

    // Storage mappings
    mapping(bytes32 => uint256) internal uintStorage;
    mapping(bytes32 => string) internal stringStorage;
    mapping(bytes32 => address) internal addressStorage;
    mapping(bytes32 => bytes) internal bytesStorage;
    mapping(bytes32 => bool) internal boolStorage;
    mapping(bytes32 => int256) internal intStorage;
    mapping(bytes32 => bytes32) internal bytes32Storage;

    mapping(bytes32 => uint256[]) internal uintArrayStorage;
    mapping(bytes32 => string[]) internal stringArrayStorage;
    mapping(bytes32 => address[]) internal addressArrayStorage;
    //mapping(bytes32 => bytes[]) internal bytesArrayStorage;
    mapping(bytes32 => bool[]) internal boolArrayStorage;
    mapping(bytes32 => int256[]) internal intArrayStorage;
    mapping(bytes32 => bytes32[]) internal bytes32ArrayStorage;

    /**
    * @dev Tells the version number of the current implementation
    * @return uint representing the number of the current version
    */
    function version() public view returns (uint256) {
        return _version;
    }

    /**
    * @dev Tells the address of the current implementation
    * @return address of the current implementation
    */
    function implementation() public view returns (address) {
        return _implementation;
    }

}



interface IKeysManager {
    function initiateKeys(address) public;
    function createKeys(address, address, address) public;
    function isMiningActive(address) public view returns(bool);
    function isVotingActive(address) public view returns(bool);
    function isPayoutActive(address) public view returns(bool);
    function getVotingByMining(address) public view returns(address);
    function getPayoutByMining(address) public view returns(address);
    function addMiningKey(address) public;
    function addVotingKey(address, address) public;
    function addPayoutKey(address, address) public;
    function removeMiningKey(address) public;
    function removeVotingKey(address) public;
    function removePayoutKey(address) public;
    function swapMiningKey(address, address) public;
    function swapVotingKey(address, address) public;
    function swapPayoutKey(address, address) public;
    function getTime() public view returns(uint256);
    function getMiningKeyHistory(address) public view returns(address);
    function getMiningKeyByVoting(address) public view returns(address);
    function getInitialKey(address) public view returns(uint8);
    function migrateInitialKey(address) public;
    function migrateMiningKey(address) public;
}


interface IProxyStorage {
    function getKeysManager() public view returns(address);
    function getBallotsStorage() public view returns(address);
    function getVotingToChangeKeys() public view returns(address);
    function getVotingToChangeMinThreshold() public view returns(address);
    function getPoaConsensus() public view returns(address);
    function initializeAddresses(address, address, address, address, address, address) public;
    function setContractAddress(uint256, address) public;
    function isValidator(address) public view returns(bool);
}


interface IBallotsStorage {
    function setThreshold(uint256, uint8) public;
    function getBallotThreshold(uint8) public view returns(uint256);
    function getVotingToChangeThreshold() public view returns(address);
    function getTotalNumberOfValidators() public view returns(uint256);
    function getProxyThreshold() public view returns(uint256);
    function getBallotLimitPerValidator() public view returns(uint256);
    function getMaxLimitBallot() public view returns(uint256);
}




/**
 * @title SafeMath
 * @dev Math operations with safety checks that throw on error
 */
library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns(uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns(uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns(uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns(uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}

contract ValidatorMetadata is EternalStorage {
    using SafeMath for uint256;

    event MetadataCreated(address indexed miningKey);
    event ChangeRequestInitiated(address indexed miningKey);
    event CancelledRequest(address indexed miningKey);
    event Confirmed(address indexed miningKey, address votingSender);
    event FinalizedChange(address indexed miningKey);
    event RequestForNewProxy(address newProxyAddress);
    event ChangeProxyStorage(address newProxyAddress);

    modifier onlyValidVotingKey(address _votingKey) {
        IKeysManager keysManager = IKeysManager(getKeysManager());
        require(keysManager.isVotingActive(_votingKey));
        _;
    }

    modifier onlyFirstTime(address _votingKey) {
        address miningKey = getMiningByVotingKey(_votingKey);
        require(uintStorage[keccak256("validators", miningKey, "createdDate")] == 0);
        _;
    }

    modifier onlyOwner() {
        require(msg.sender == addressStorage[keccak256("owner")]);
        _;
    }

    function proxyStorage() public view returns (address) {
        return addressStorage[keccak256("proxyStorage")];
    }

    function pendingProxyStorage() public view returns (address) {
        return addressStorage[keccak256("pendingProxyStorage")];
    }

    function validators(address _miningKey) public view returns (
        bytes32 firstName,
        bytes32 lastName,
        bytes32 licenseId,
        string fullAddress,
        bytes32 state,
        bytes32 zipcode,
        uint256 expirationDate,
        uint256 createdDate,
        uint256 updatedDate,
        uint256 minThreshold
    ) {
        firstName = bytes32Storage[keccak256("validators", _miningKey, "firstName")];
        lastName = bytes32Storage[keccak256("validators", _miningKey, "lastName")];
        licenseId = bytes32Storage[keccak256("validators", _miningKey, "licenseId")];
        fullAddress = stringStorage[keccak256("validators", _miningKey, "fullAddress")];
        state = bytes32Storage[keccak256("validators", _miningKey, "state")];
        zipcode = bytes32Storage[keccak256("validators", _miningKey, "zipcode")];
        expirationDate = uintStorage[keccak256("validators", _miningKey, "expirationDate")];
        createdDate = uintStorage[keccak256("validators", _miningKey, "createdDate")];
        updatedDate = uintStorage[keccak256("validators", _miningKey, "updatedDate")];
        minThreshold = uintStorage[keccak256("validators", _miningKey, "minThreshold")];
    }

    function pendingChanges(address _miningKey) public view returns (
        bytes32 firstName,
        bytes32 lastName,
        bytes32 licenseId,
        string fullAddress,
        bytes32 state,
        bytes32 zipcode,
        uint256 expirationDate,
        uint256 createdDate,
        uint256 updatedDate,
        uint256 minThreshold
    ) {
        firstName = bytes32Storage[keccak256("pendingChanges", _miningKey, "firstName")];
        lastName = bytes32Storage[keccak256("pendingChanges", _miningKey, "lastName")];
        licenseId = bytes32Storage[keccak256("pendingChanges", _miningKey, "licenseId")];
        fullAddress = stringStorage[keccak256("pendingChanges", _miningKey, "fullAddress")];
        state = bytes32Storage[keccak256("pendingChanges", _miningKey, "state")];
        zipcode = bytes32Storage[keccak256("pendingChanges", _miningKey, "zipcode")];
        expirationDate = uintStorage[keccak256("pendingChanges", _miningKey, "expirationDate")];
        createdDate = uintStorage[keccak256("pendingChanges", _miningKey, "createdDate")];
        updatedDate = uintStorage[keccak256("pendingChanges", _miningKey, "updatedDate")];
        minThreshold = uintStorage[keccak256("pendingChanges", _miningKey, "minThreshold")];
    }

    function confirmations(address _miningKey) public view returns (
        uint256 count,
        address[] voters
    ) {
        return (
            uintStorage[keccak256("confirmations", _miningKey, "count")],
            addressArrayStorage[keccak256("confirmations", _miningKey, "voters")]
        );
    }

    function pendingProxyConfirmations(address _newProxyAddress) public view returns (
        uint256 count,
        address[] voters
    ) {
        bytes32 countHash = keccak256("pendingProxyConfirmations", _newProxyAddress, "count");
        bytes32 votersHash = keccak256("pendingProxyConfirmations", _newProxyAddress, "voters");

        return (
            uintStorage[countHash],
            addressArrayStorage[votersHash]
        );
    }

    function setProxyAddress(address _newProxyAddress) public onlyValidVotingKey(msg.sender) {
        bytes32 pendingProxyStorageHash =
            keccak256("pendingProxyStorage");

        require(addressStorage[pendingProxyStorageHash] == address(0));
        
        addressStorage[pendingProxyStorageHash] = _newProxyAddress;
        uintStorage[keccak256("pendingProxyConfirmations", _newProxyAddress, "count")] = 1;
        addressArrayStorage[keccak256("pendingProxyConfirmations", _newProxyAddress, "voters")].push(msg.sender);
        
        RequestForNewProxy(_newProxyAddress);
    }

    function confirmNewProxyAddress(address _newProxyAddress)
        public
        onlyValidVotingKey(msg.sender)
    {
        bytes32 proxyStorageHash = keccak256("proxyStorage");
        bytes32 pendingProxyStorageHash = keccak256("pendingProxyStorage");
        bytes32 countHash = keccak256("pendingProxyConfirmations", _newProxyAddress, "count");
        bytes32 votersHash = keccak256("pendingProxyConfirmations", _newProxyAddress, "voters");
        
        require(addressStorage[pendingProxyStorageHash] != address(0));
        require(!isAddressAlreadyVotedProxy(_newProxyAddress, msg.sender));

        uintStorage[countHash] = uintStorage[countHash].add(1);
        addressArrayStorage[votersHash].push(msg.sender);
        
        if (uintStorage[countHash] >= 3) {
            addressStorage[proxyStorageHash] = _newProxyAddress;
            addressStorage[pendingProxyStorageHash] = address(0);
            delete uintStorage[countHash];
            delete addressArrayStorage[votersHash];
            ChangeProxyStorage(_newProxyAddress);
        }
        
        Confirmed(_newProxyAddress, msg.sender);
    }

    function initMetadata(
        bytes32 _firstName,
        bytes32 _lastName,
        bytes32 _licenseId,
        string _fullAddress,
        bytes32 _state,
        bytes32 _zipcode,
        uint256 _expirationDate,
        uint256 _createdDate,
        uint256 _updatedDate,
        uint256 _minThreshold,
        address _miningKey
    )
        public
        onlyOwner
    {
        IKeysManager keysManager = IKeysManager(getKeysManager());
        require(keysManager.isMiningActive(_miningKey));
        require(uintStorage[keccak256("validators", _miningKey, "createdDate")] == 0);
        require(_createdDate != 0);
        require(!boolStorage[keccak256("initMetadataDisabled")]);
        bytes32Storage[keccak256("validators", _miningKey, "firstName")] = _firstName;
        bytes32Storage[keccak256("validators", _miningKey, "lastName")] = _lastName;
        bytes32Storage[keccak256("validators", _miningKey, "licenseId")] = _licenseId;
        bytes32Storage[keccak256("validators", _miningKey, "state")] = _state;
        stringStorage[keccak256("validators", _miningKey, "fullAddress")] = _fullAddress;
        bytes32Storage[keccak256("validators", _miningKey, "zipcode")] = _zipcode;
        uintStorage[keccak256("validators", _miningKey, "expirationDate")] = _expirationDate;
        uintStorage[keccak256("validators", _miningKey, "createdDate")] = _createdDate;
        uintStorage[keccak256("validators", _miningKey, "updatedDate")] = _updatedDate;
        uintStorage[keccak256("validators", _miningKey, "minThreshold")] = _minThreshold;
    }

    function initMetadataDisable() public onlyOwner {
        boolStorage[keccak256("initMetadataDisabled")] = true;
    }

    function createMetadata(
        bytes32 _firstName,
        bytes32 _lastName,
        bytes32 _licenseId,
        string _fullAddress,
        bytes32 _state,
        bytes32 _zipcode,
        uint256 _expirationDate
    )
        public
        onlyValidVotingKey(msg.sender)
        onlyFirstTime(msg.sender)
    {
        address miningKey = getMiningByVotingKey(msg.sender);
        bytes32Storage[keccak256("validators", miningKey, "firstName")] = _firstName;
        bytes32Storage[keccak256("validators", miningKey, "lastName")] = _lastName;
        bytes32Storage[keccak256("validators", miningKey, "licenseId")] = _licenseId;
        bytes32Storage[keccak256("validators", miningKey, "state")] = _state;
        stringStorage[keccak256("validators", miningKey, "fullAddress")] = _fullAddress;
        bytes32Storage[keccak256("validators", miningKey, "zipcode")] = _zipcode;
        uintStorage[keccak256("validators", miningKey, "expirationDate")] = _expirationDate;
        uintStorage[keccak256("validators", miningKey, "createdDate")] = getTime();
        uintStorage[keccak256("validators", miningKey, "updatedDate")] = 0;
        uintStorage[keccak256("validators", miningKey, "minThreshold")] = getMinThreshold();
        MetadataCreated(miningKey);
    }

    function changeRequest(
        bytes32 _firstName,
        bytes32 _lastName,
        bytes32 _licenseId,
        string _fullAddress,
        bytes32 _state,
        bytes32 _zipcode,
        uint256 _expirationDate
    )
        public
        onlyValidVotingKey(msg.sender)
        returns(bool)
    {
        address miningKey = getMiningByVotingKey(msg.sender);
        return changeRequestForValidator(
            _firstName,
            _lastName,
            _licenseId,
            _fullAddress,
            _state,
            _zipcode,
            _expirationDate,
            miningKey
        );
    }

    function changeRequestForValidator(
        bytes32 _firstName,
        bytes32 _lastName,
        bytes32 _licenseId,
        string _fullAddress,
        bytes32 _state,
        bytes32 _zipcode,
        uint256 _expirationDate,
        address _miningKey
    )
        public
        onlyValidVotingKey(msg.sender)
        returns(bool) 
    {
        bytes32Storage[keccak256("pendingChanges", _miningKey, "firstName")] = _firstName;
        bytes32Storage[keccak256("pendingChanges", _miningKey, "lastName")] = _lastName;
        bytes32Storage[keccak256("pendingChanges", _miningKey, "licenseId")] = _licenseId;
        bytes32Storage[keccak256("pendingChanges", _miningKey, "state")] = _state;
        stringStorage[keccak256("pendingChanges", _miningKey, "fullAddress")] = _fullAddress;
        bytes32Storage[keccak256("pendingChanges", _miningKey, "zipcode")] = _zipcode;
        uintStorage[keccak256("pendingChanges", _miningKey, "expirationDate")] = _expirationDate;
        uintStorage[keccak256("pendingChanges", _miningKey, "createdDate")] =
            uintStorage[keccak256("validators", _miningKey, "createdDate")];
        uintStorage[keccak256("pendingChanges", _miningKey, "updatedDate")] = getTime();
        uintStorage[keccak256("pendingChanges", _miningKey, "minThreshold")] =
            uintStorage[keccak256("validators", _miningKey, "minThreshold")];
        
        delete uintStorage[keccak256("confirmations", _miningKey, "count")];
        delete addressArrayStorage[keccak256("confirmations", _miningKey, "voters")];
        
        ChangeRequestInitiated(_miningKey);
        return true;
    }

    function cancelPendingChange() public onlyValidVotingKey(msg.sender) returns(bool) {
        address miningKey = getMiningByVotingKey(msg.sender);
        _deletePendingChange(miningKey);
        CancelledRequest(miningKey);
        return true;
    }

    function isAddressAlreadyVoted(address _miningKey, address _voter) public view returns(bool) {
        bytes32 hash = keccak256("confirmations", _miningKey, "voters");
        uint256 length = addressArrayStorage[hash].length;
        for (uint256 i = 0; i < length; i++) {
            if (addressArrayStorage[hash][i] == _voter) {
                return true;   
            }
        }
        return false;
    }

    function isAddressAlreadyVotedProxy(address _newProxy, address _voter) public view returns(bool) {
        bytes32 hash = keccak256("pendingProxyConfirmations", _newProxy, "voters");
        uint256 length = addressArrayStorage[hash].length;
        for (uint256 i = 0; i < length; i++) {
            if (addressArrayStorage[hash][i] == _voter) {
                return true;   
            }
        }
        return false;
    }

    function confirmPendingChange(address _miningKey) public onlyValidVotingKey(msg.sender) {
        bytes32 votersHash = keccak256("confirmations", _miningKey, "voters");
        bytes32 countHash = keccak256("confirmations", _miningKey, "count");
        
        require(!isAddressAlreadyVoted(_miningKey, msg.sender));
        require(addressArrayStorage[votersHash].length <= 50); // no need for more confirmations

        address miningKey = getMiningByVotingKey(msg.sender);
        require(miningKey != _miningKey);

        addressArrayStorage[votersHash].push(msg.sender);
        uintStorage[countHash] = uintStorage[countHash].add(1);
        Confirmed(_miningKey, msg.sender);
    }

    function finalize(address _miningKey) public onlyValidVotingKey(msg.sender) {
        uint256 count =
            uintStorage[keccak256("confirmations", _miningKey, "count")];
        uint256 minThreshold =
            uintStorage[keccak256("pendingChanges", _miningKey, "minThreshold")];

        require(count >= minThreshold);
        require(onlyIfChangeExist(_miningKey));

        bytes32Storage[keccak256("validators", _miningKey, "firstName")] = 
            bytes32Storage[keccak256("pendingChanges", _miningKey, "firstName")];
        bytes32Storage[keccak256("validators", _miningKey, "lastName")] = 
            bytes32Storage[keccak256("pendingChanges", _miningKey, "lastName")];
        bytes32Storage[keccak256("validators", _miningKey, "licenseId")] = 
            bytes32Storage[keccak256("pendingChanges", _miningKey, "licenseId")];
        bytes32Storage[keccak256("validators", _miningKey, "state")] = 
            bytes32Storage[keccak256("pendingChanges", _miningKey, "state")];
        stringStorage[keccak256("validators", _miningKey, "fullAddress")] = 
            stringStorage[keccak256("pendingChanges", _miningKey, "fullAddress")];
        bytes32Storage[keccak256("validators", _miningKey, "zipcode")] = 
            bytes32Storage[keccak256("pendingChanges", _miningKey, "zipcode")];
        uintStorage[keccak256("validators", _miningKey, "expirationDate")] = 
            uintStorage[keccak256("pendingChanges", _miningKey, "expirationDate")];
        uintStorage[keccak256("validators", _miningKey, "createdDate")] = 
            uintStorage[keccak256("pendingChanges", _miningKey, "createdDate")];
        uintStorage[keccak256("validators", _miningKey, "updatedDate")] = 
            uintStorage[keccak256("pendingChanges", _miningKey, "updatedDate")];
        uintStorage[keccak256("validators", _miningKey, "minThreshold")] = 
            uintStorage[keccak256("pendingChanges", _miningKey, "minThreshold")];
        
        _deletePendingChange(_miningKey);
        FinalizedChange(_miningKey);
    }

    function getMiningByVotingKey(address _votingKey) public view returns(address) {
        IKeysManager keysManager = IKeysManager(getKeysManager());
        return keysManager.getMiningKeyByVoting(_votingKey);
    }

    function getTime() public view returns(uint256) {
        return now;
    }

    function getMinThreshold() public view returns(uint256) {
        uint8 thresholdType = 2;
        IBallotsStorage ballotsStorage = IBallotsStorage(getBallotsStorage());
        return ballotsStorage.getBallotThreshold(thresholdType);
    }

    function getBallotsStorage() public view returns(address) {
        return IProxyStorage(proxyStorage()).getBallotsStorage();
    }

    function getKeysManager() public view returns(address) {
        return IProxyStorage(proxyStorage()).getKeysManager();
    }

    function onlyIfChangeExist(address _miningKey) public view returns(bool) {
        return uintStorage[keccak256("pendingChanges", _miningKey, "createdDate")] > 0;
    }

    function _deletePendingChange(address _miningKey) private {
        delete bytes32Storage[keccak256("pendingChanges", _miningKey, "firstName")];
        delete bytes32Storage[keccak256("pendingChanges", _miningKey, "lastName")];
        delete bytes32Storage[keccak256("pendingChanges", _miningKey, "licenseId")];
        delete bytes32Storage[keccak256("pendingChanges", _miningKey, "state")];
        delete stringStorage[keccak256("pendingChanges", _miningKey, "fullAddress")];
        delete bytes32Storage[keccak256("pendingChanges", _miningKey, "zipcode")];
        delete uintStorage[keccak256("pendingChanges", _miningKey, "expirationDate")];
        delete uintStorage[keccak256("pendingChanges", _miningKey, "createdDate")];
        delete uintStorage[keccak256("pendingChanges", _miningKey, "updatedDate")];
        delete uintStorage[keccak256("pendingChanges", _miningKey, "minThreshold")];
    }
}


* 本地测试metadata管不管用
    只用一个meta contract
    meta
    0x6de8c3548bd1aa8e68f50b2235c80f7dc32a93edd80705161e77c8446ce02e6a

    validator
    0x3892dd7cb2048e8447520d7696dbd62c1adc2a3b3fa326249cb3333f8158bd3e

    secret
    cnnYJyfus7mw4rF4bcn3WzPbOGQ84BGQf8dTiq6Nt/N1GFfcocMbuSO25Lj/dtTN



    export LOGO="/images/abel2.svg"
    export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/blockscout"
    export DB_HOST=localhost
    export DB_PASSWORD=postgres
    export DB_PORT=5432
    export DB_USERNAME=postgres
    export SECRET_KEY_BASE="rw+9ZAP3yIYL8nhhKyz1J8l8pN08MooB5HmojzJngP/hXY7NRp3PWQGqZYyvN5IX"

    export ETHEREUM_JSONRPC_VARIANT=geth
    export ETHEREUM_JSONRPC_HTTP_URL="http://localhost:22000"
    export ETHEREUM_JSONRPC_WS_URL="ws://localhost:23000"
    export ETHEREUM_JSONRPC_TRACE_URL="http://localhost:22000"
    export BLOCK_TRANSFORMER=clique

    export PORT=4000
    export COIN=Abel 
    export NETWORK=Ethereum 

    export SUBNETWORK=Abel

    mix do ecto.drop, ecto.create, ecto.migrate

    
    export METADATA_CONTRACT="0x6a27C634F21bcA2A4655A5c8a0a43bf43fA93361"
    
    export VALIDATORS_CONTRACT="0xd0C59eC469ec5EE30C78A4F90fbBec23D243dee7"
0xd0C59eC469ec5EE30C78A4F90fbBec23D243dee7

* 删掉浏览器库 重新试一下

* 如果不能用继续找合约或者直接看源代码

## 06-22
* 测试weth //ok
* validator
    🤔istanbul的协议貌似在这里不好使 不过跑起来的话 validator的显示倒也行
    可以换成clique
* 改ether显示为abel
    查看配置文件有无 //无
    测试改一下coin? //不行
    直接修改页面 //可以修改一部分
    需要需改view层


## 06-23
* 熟悉elixir
* validator
    线上添加了validator的显示
    可以看看有什么影响
* 改ether显示为abel
    需要需改view层


## 06-24
* 熟悉elixir
* validator
    线上添加了validator的显示
    可以看看有什么影响
* 改ether显示为abel
    需要需改view层


## 06-25
* 熟悉elixir


## 06-28
* 熟悉elixir
* 熟悉phoenix


## 06-29
* 熟悉elixir
* 熟悉phoenix
* 看书

## 06-30
* 熟悉elixir
* 熟悉phoenix
* 看书

## 07-01
* phoenix in action //ok
* 修改yun-explorer ether 转 abel  //OK
    启动测试 //ok
    https://phrase.com/blog/posts/i18n-for-phoenix-applications-with-gettext/

## 07-02
* 浏览器 测试上线
    重新编译
    正式编译
    看看还有什么要改
* swap 跑起来试一下
    https://www.youtube.com/watch?v=J4g8jQoZkrY
    https://www.youtube.com/watch?v=U3fTTqHy7F4
    https://www.google.com/search?q=sushiswap+interface+run+local&oq=sushiswap+interface+run+local&aqs=chrome..69i57j33i160l3.8719j1j15&sourceid=chrome&ie=UTF-8



## 问题待办
* 确认为什么交易的 miner 编号是00000
    需要配置
* 导航 要不要搞成不要下拉的
* rpc页面
* 修改浏览器title //ok
* token的显示问题 //转账后就有
* token余额在小狐狸里的显示问题 //转账后添加即可
* k8部署
* 转币给cto //ok
* 更改 浏览器的基础币显示 
* 首页k线 //ok
* 多节点部署测试
* 浏览器显示修改完上测试链
* swap修改