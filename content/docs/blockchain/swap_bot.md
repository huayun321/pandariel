---
weight: 1
bookFlatSection: true
draft: true
title: "Swap Bot"
---

## 算法
多个币种之间套利
多线程判断交易池链是否获利
如果可以获利 就发起交易

部署到测试链

## 本地链
### ganache
### geth --dev console
### geth
geth --datadir new_block --http --http.api "eth,web3,miner,admin,personal,net" --http.corsdomain "*" --nodiscover --networkid 15 


## go ethereum
### 智能合约
#### 合约部署

Store.sol
```javascript
pragma solidity >=0.7.0 <0.9;

contract Store {
    event ItemSet(bytes32 key, bytes32 value);

    string public version;
    mapping (bytes32 => bytes32) public items;

    constructor(string memory _version) {
        version = _version;
    }

    function setItem(bytes32 key, bytes32 value) external {
        items[key] = value;
        emit ItemSet(key, value);
    }
}
```

```shell
	solc --abi contract_test/Store.sol -o contract_test --overwrite
    solc --bin contract_test/Store.sol -o contract_test --overwrite
    abigen --bin contract_test/Store.bin --abi contract_test/Store.abi --pkg contract_test --out contract_test/store.go

```

deploy
```golang
package main

import (
	"context"
	"crypto/ecdsa"
	"fmt"
	"log"
	"math/big"

	"github.com/ethereum/go-ethereum/accounts/abi/bind"
	"github.com/ethereum/go-ethereum/crypto"
	"github.com/ethereum/go-ethereum/ethclient"

	store "bsc-rebalance/contract_test" // for demo
)

func main() {
	client, err := ethclient.Dial("http://localhost:7545")
	if err != nil {
		log.Fatal(err)
	}

	privateKey, err := crypto.HexToECDSA("7d2342d709aa20d107598321fe86bb75ef9216e0b18c0e0ecac1b7774e1511f6")
	if err != nil {
		log.Fatal(err)
	}

	publicKey := privateKey.Public()
	publicKeyECDSA, ok := publicKey.(*ecdsa.PublicKey)
	if !ok {
		log.Fatal("cannot assert type: publicKey is not of type *ecdsa.PublicKey")
	}

	fromAddress := crypto.PubkeyToAddress(*publicKeyECDSA)
	nonce, err := client.PendingNonceAt(context.Background(), fromAddress)
	if err != nil {
		log.Fatal(err)
	}

	gasPrice, err := client.SuggestGasPrice(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	auth := bind.NewKeyedTransactor(privateKey)
	auth.Nonce = big.NewInt(int64(nonce))
	auth.Value = big.NewInt(0)     // in wei
	auth.GasLimit = uint64(6721975) // in units
	auth.GasPrice = gasPrice

	input := "1.0"
	address, tx, instance, err := store.DeployContractTest(auth, client, input)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(address.Hex())   // 0xad582B2A6A2f42130cE8F2420e0f2b9661460EFE
	fmt.Println(tx.Hash().Hex()) // 0x5152c186273a55bd9bf7978edd5efa8a763697707a966aa60b2dc892fd6feff3

	_ = instance
}
```

#### 合约加载

```golang
package main

import (
	"fmt"
	"log"

	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/ethclient"

	store "bsc-rebalance/contract_test" // for demo
)

func main() {
	client, err := ethclient.Dial("http://localhost:7545")
	if err != nil {
		log.Fatal(err)
	}

	address := common.HexToAddress("0xad582B2A6A2f42130cE8F2420e0f2b9661460EFE")
	instance, err := store.NewContractTest(address, client)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("contract is loaded")
	_ = instance
}
```

#### 合约读取

```go
package main

import (
	"fmt"
	"log"

	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/ethclient"

	store "bsc-rebalance/contract_test" // for demo
)

func main() {
	client, err := ethclient.Dial("http://localhost:7545")
	if err != nil {
		log.Fatal(err)
	}

	address := common.HexToAddress("0xad582B2A6A2f42130cE8F2420e0f2b9661460EFE")
	instance, err := store.NewContractTest(address, client)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("contract is loaded")

	version, err := instance.Version(nil)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(version) // "1.0"
}
```

#### 合约写入

```go
package main

import (
	"context"
	"crypto/ecdsa"
	"fmt"
	"github.com/ethereum/go-ethereum/crypto"
	"log"
	"math/big"

	"github.com/ethereum/go-ethereum/accounts/abi/bind"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/ethclient"

	store "bsc-rebalance/contract_test" // for demo
)

func main() {
	client, err := ethclient.Dial("http://localhost:7545")
	if err != nil {
		log.Fatal(err)
	}

	privateKey, err := crypto.HexToECDSA("7d2342d709aa20d107598321fe86bb75ef9216e0b18c0e0ecac1b7774e1511f6")
	if err != nil {
		log.Fatal(err)
	}

	publicKey := privateKey.Public()
	publicKeyECDSA, ok := publicKey.(*ecdsa.PublicKey)
	if !ok {
		log.Fatal("cannot assert type: publicKey is not of type *ecdsa.PublicKey")
	}

	fromAddress := crypto.PubkeyToAddress(*publicKeyECDSA)
	nonce, err := client.PendingNonceAt(context.Background(), fromAddress)
	if err != nil {
		log.Fatal(err)
	}

	gasPrice, err := client.SuggestGasPrice(context.Background())
	if err != nil {
		log.Fatal(err)
	}

	auth := bind.NewKeyedTransactor(privateKey)
	auth.Nonce = big.NewInt(int64(nonce))
	auth.Value = big.NewInt(0)     // in wei
	auth.GasLimit = uint64(6721975) // in units
	auth.GasPrice = gasPrice

	address := common.HexToAddress("0xad582B2A6A2f42130cE8F2420e0f2b9661460EFE")
	instance, err := store.NewContractTest(address, client)
	if err != nil {
		log.Fatal(err)
	}

	key := [32]byte{}
	value := [32]byte{}
	copy(key[:], []byte("foo"))
	copy(value[:], []byte("bar"))

	tx, err := instance.SetItem(auth, key, value)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("tx sent: %s\n", tx.Hash().Hex()) // tx sent: 0x8d490e535678e9a24360e955d75b27ad307bdfb97a1dca51d0f3035dcee3e870

	result, err := instance.Items(nil, key)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println(string(result[:])) // "bar"
}
```

#### 查询erc20

erc20.sol
```javascript
pragma solidity >=0.5.0;

interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
}
```

```go
package main

import (
	"fmt"
	"log"
	"math"
	"math/big"

	"github.com/ethereum/go-ethereum/accounts/abi/bind"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/ethclient"

	token "bsc-rebalance/ierc20" // for demo
)

func main() {
	client, err := ethclient.Dial("https://mainnet.infura.io/v3/bb5bf63ee4454829987724ec4cebd8b6")
	if err != nil {
		log.Fatal(err)
	}

	// bnb
	// https://etherscan.io/token/0xB8c77482e45F1F44dE1745F52C74426C631bDD52
	tokenAddress := common.HexToAddress("0xB8c77482e45F1F44dE1745F52C74426C631bDD52")
	instance, err := token.NewIerc20(tokenAddress, client)
	if err != nil {
		log.Fatal(err)
	}

	// someone
	address := common.HexToAddress("0x8c00ea22e30cac298758660bdf2a94e91b07dfda")
	bal, err := instance.BalanceOf(&bind.CallOpts{}, address)
	if err != nil {
		log.Fatal(err)
	}

	name, err := instance.Name(&bind.CallOpts{})
	if err != nil {
		log.Fatal(err)
	}

	symbol, err := instance.Symbol(&bind.CallOpts{})
	if err != nil {
		log.Fatal(err)
	}

	decimals, err := instance.Decimals(&bind.CallOpts{})
	if err != nil {
		log.Fatal(err)
	}

	fmt.Printf("name: %s\n", name)         // "name: Golem Network"
	fmt.Printf("symbol: %s\n", symbol)     // "symbol: GNT"
	fmt.Printf("decimals: %v\n", decimals) // "decimals: 18"

	fmt.Printf("wei: %s\n", bal) // "wei: 74605500647408739782407023"

	fbal := new(big.Float)
	fbal.SetString(bal.String())
	value := new(big.Float).Quo(fbal, big.NewFloat(math.Pow10(int(decimals))))

	fmt.Printf("balance: %f", value) // "balance: 74605500.647409"
}
```

### 事件日志
#### ws

```golang
package main

import (
	"context"
	"fmt"
	"log"

	"github.com/ethereum/go-ethereum"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/core/types"
	"github.com/ethereum/go-ethereum/ethclient"
)

func main() {
	client, err := ethclient.Dial("ws://localhost:7545")
	if err != nil {
		log.Println("not connected")
		log.Fatal(err)
	}

	contractAddress := common.HexToAddress("0x0a96633331A210BF71B4f78b658E90c4F58Ea9D5")
	query := ethereum.FilterQuery{
		Addresses: []common.Address{contractAddress},
	}



	logs := make(chan types.Log)
	sub, err := client.SubscribeFilterLogs(context.Background(), query, logs)
	if err != nil {
		log.Fatal(err)
	}

	for {
		select {
		case err := <-sub.Err():
			fmt.Println("==== got an event =====")
			log.Fatal(err)
		case vLog := <-logs:
			fmt.Println("==== get an event =====")
			fmt.Println(vLog) // pointer to event log

		}
	}
}
```

## 获取几个热门币 pair

涉及到pancake core 中的两个合约
pancakeFactory
pancakePair

### pancakeFactory
可以通过 getPair 获取到 pair地址
* 编译合约 只需要接口合约即可
* 连接主网测试

```javascript
pragma solidity >=0.5.0;

interface IPancakeFactory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
```

```golang
package main

import (
	"fmt"
	"github.com/ethereum/go-ethereum/accounts/abi/bind"
	"log"
	"math/big"

	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/ethclient"

	pancake "bsc-rebalance/pancake" // for demo
)

func main() {
	client, err := ethclient.Dial("https://bsc-dataseed.binance.org")
	if err != nil {
		log.Fatal(err)
	}

	address := common.HexToAddress("0xBCfCcbde45cE874adCB698cC183deBcF17952812")
	instance, err := pancake.NewPancake(address, client)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("contract is loaded")
	_ = instance

	pl, err := instance.AllPairsLength(&bind.CallOpts{})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("AllPairsLength is ", pl)

	p1, err := instance.AllPairs(&bind.CallOpts{}, big.NewInt(1))
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("AllPairsLength is ", p1)


}

```

### pancakePair
通过事件获取最新的汇率
* 获取合约地址
* 下载合约接口文件
* 测试合约
* 测试事件
* 事件unpack
* 通过事件更新汇率
* 三个pair的汇率测试

```
	wbnb->busd
	busd->usdt
	usdt->wbnb
```
* 生成类图
* 日志优化 logrus
* 整理代码
* cobra
* 测试生成路径
* 查看bsc ws提供者的文档 处理超时

```javascript
pragma solidity >=0.5.0;

interface IPancakePair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}
```

## 路径生成算法
	* 图算法
### 排列组合
	1、 func generate (first, [tokens...], deepth)
	len tokens = n 
	deepth = m
	2、 生成监控pair (n!/(n-2)!) + 2
	3、 生成path  (n!/(n-m)!) + (n-1!/(n-m)!)...


## 更改汇率算法
* 确定计算公式
	https://github.com/pancakeswap/pancake-swap-periphery/blob/master/contracts/PancakeRouter01.sol

```javascript
	function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external override ensure(deadline) returns (uint[] memory amounts) {
        amounts = PancakeLibrary.getAmountsIn(factory, amountOut, path); // this line!!!!
        require(amounts[0] <= amountInMax, 'PancakeRouter: EXCESSIVE_INPUT_AMOUNT');
        TransferHelper.safeTransferFrom(path[0], msg.sender, PancakeLibrary.pairFor(factory, path[0], path[1]), amounts[0]);
        _swap(amounts, path, to);
    }
```

* solidity 的safeMath
	https://ethereumdev.io/using-safe-math-library-to-prevent-from-overflows/

* 修改类图
* 更新计算公式
	* 计算公式的精度问题 无浮点正数18个小数位
	* 计算公式的迭代问题 没有小数就没有迭代问题

## 测试网络 整理配置
### 测试网络无websocket
		无法更新reserves 不能trigger交易
		time.tick 更新reserve trigger
		直接使用uniswap

### 确定测试网络上是否有pancake合约
	* 不知道是否有
		查一下文档 木有
		到社区问一下 没有回复
	* 可以测试下是否可以自己部署 自己部署没有数据
	* 可以使用uniswap的合约测试
		是否需要重新编译factory pair 不需要
	* 可以使用uniswap测试

### 整理代码
	整理路径相关的 使用循环代替写死的代码
	https://stackoverflow.com/questions/19992334/how-to-listen-to-n-channels-dynamic-select-statement/32381409
	https://play.golang.org/p/8zwvSk4kjx

### 添加viper配置bot参数
	https://github.com/spf13/viper
	https://zh.wikipedia.org/wiki/YAML

### 部署到本地测试 不需要
	* 部署factory
	* 部署pair
	* 给自己发token
	* 添加pair 和流动性

## 交易测试
### 编译IpancakeRouter02 done
### 获取测试币
* 新建两个地址 不需要创建 meta mask可用
* 获取测试币 账户1已获取18.75

### 获取测试对
* config_uni_rinkeby.yaml
	* infura rinkeby测试地址
* 测试两个账户之间转账
* 找可用pair对
	自己在uni rinkeby 建的不可用啊。。
	查看文档
	社区询问
	通过factory 查询
	已找到可用对

### 发起交易
* 账户交易 转账 ok
* 账户交易 代币转账 ok

* 测试生成代币 ok 
https://github.com/ethereum/EIPs
* 测试代币转账
	部署到测试网
* 代币转账是否可以调用合约
	可以
	需要解决返回的错误
* swap token流程 
	token library transferhelper _swap pair.swap _safetransfer

* 调用合约交易需要什么设定 
	是不是需要部署合约呢
		不部署合约 
	如果不部署合约是否可以调用成功呢
* 发起一次token交换请求 需要解决返回的错误
* 设定想要获取的利润大小 触发交易

### 解决交易问题
* 确定是否授权的正确的token shi
* 确定确认的额度正确 shi 
* 找问题原因
	测试其他方法
	直接调用safe transfer
	原因竟然是没有填写option
	要自己看代码 源码


## 整理代码
* 封装交换
	交易自动触发 
		事件触发后 如果交易可获利 判断是否 启用最大套利选项  
			根据当前账户 token最大额度 发起getamountsout 如果有利 发起交易
		类图套利人儿 
		套利人对象池
		查看对象池文档
* pair 的生成 应该放在bot里
	清理pair
	单路径的时候不需要放在bot里
	多路径时候放入
	还是需要实现pubsub
	不能使用使用单个chan发送数据 否则 不够用来消费

## 整理代码2
* gollum添加swap 
	解决溢出
* pair添加发布订阅
* chain添加发布订阅
* git workflow


## 整理代码3 
* git flow   //OK
* 生成pair放到 路径初始化的时候 //ok  //目前的pair生成是根据路径的 之后要改成 根据token所生成pair对的 
* chain的发布订阅 //ok
* getAmountout 可以放到其他地方  //ok
* 发布订阅 //ok
* 添加gas费用到 gas per pair  profit check 获利检测 //ok
* 将获取参数都转移到 cmd //ok

## 合约整理
直接用别人的就可以了
还是自己部署的安心点啊
你大爷的编译不了
还是得用公用的

## 上线前调整
* factory 和 router 的合约地址 //ok
	自己部署或者找主网上的
	factory: "0xBCfCcbde45cE874adCB698cC183deBcF17952812"
	router02: "0x05fF2B0DB69458A0750badebc4f9e13aDd608C7F"

## 日志整理
* 整理日志输出更容易阅读
	1、整理输出格式   // ok
	2、文件
	3、分析工具

* 交易频率限制每两秒一次  //ok 


## 主网整理
* 日志输出到文件时 改变格式 ok
* 可交易事件分开记录 // 分等级记录即可 ok
* 高频率是否 不记录这么多日志  // 分等级记录即可 ok
* 日志记录等级设计 //ok
	初始化日志 trace
	调试日志  debug
	pair事件日志 trace
	chain事件 计算结果日志 debug
	gollum info
		获得交易请求  
		计算交易结果 
		发起交易 
* 调整getAmountOut ok
	本地部署 
	本地改一个router测试
	或者找一个低频率的网络 和链 不发起交易 直接测试getAmountsOut
	本地 输出没有问题 算法没有问题
	会不会是时间延迟导致本地与线上不同步 ， 这样的话 就没有问题 看第一个pair的交易
	问题是不同网络 getAmountOut fee乘的参数不同
	uniswap     uint amountInWithFee = amountIn.mul(997);
	pancake     uint amountInWithFee = amountIn.mul(998);


## 发起一笔交易 路径生成
* 配置成允许发起一笔交易
	获利的
	查看交易结果
	仅发起一次
	in 0.5 bnb 
	profit 1/5000000 获利
	gas per pair 90000
	gas gasp * pl -1 
	遇到错误
	require(amounts[amounts.length - 1] >= amountOutMin, 'PancakeRouter: INSUFFICIENT_OUTPUT_AMOUNT');
	说明 发起交易时 获得的amountOut 比最小获利点小 
	说明 在交易时 涉及到的pair已经被改变 
	* 网络原因
	* 费用原因
	* 没有及时发起交易 涉及到的pair已经被改变 

* 打印的日志 修改为可配置等级等参数 //ok
* 看看能不能修改在命令行的 颜色 //ok

* 发起了一次交易 被revert 
	0x724d50e411b84a346c8ac0ea986e894183af81edadd9df75ffc546eb347e62b3
	正常的gprice 机器人 
	https://bscscan.com/tx/0x06f941206065ffdc9edc18146180564ed5ebd03734d05240ddac6d1568fcf715

	0x508e3498cef153803a8a8b4fa5c72d6abaf1d00e1943a03b2cc3be0a82904477
	11g 的影响交易的机器人
	https://bscscan.com/tx/0x5e7ca3bd52d8304e3739e42d6ea93e941babc415fefaebaefa9095d00e0c042a

## 03-13
* 调整gprice为可以通过配置文件获取 //ok
* 路径生成 //ok
	先组合 后置换
	路径测试 //ok

## 03-15 多路径测试
* 配置文件整理 使用map //ok
* 将所有配置文件的读取抽取到一个函数中 //ok
	需要设计配置文件 //ok
	尽量一次抽取到一个struct当中 //ok
	减少struct //ok
* 所有配置应该在初始前打印 //ok
* 初始化时检查网络 //ok
* 多路径生成函数 检测 //ok
* 路径 和对的初始化 //ok
	对的subEvent和初始化要分开执行 //ok
* 管道异步
	chain 异步处理事件 //ok
		需要优化
	gollum 异步处理 //ok
* 整理代码


## 03-16 
* 交易次数可以放到配置文件中  //ok
	lock比atomic更合适
* 添加token看看会不会触发错误 //ok
	确认如果一个pair初始化错误
	那就跳过该chain的生成 //ok
* 交易时 要想办法不发生重复交易 //ok
	根据事件和路径名 限制发起交易 //ok
* 一定要注意 转入 转出 账户的设置 //ok
* 部署10 15 20 25 三个实例 每个限制交易50 //oks
	配置文件 日志


交易被抢
https://bscscan.com/tx/0xd598f08b5b7748ecb6d372e06fc4634f6d061a9633844365702e175fd60f877e tx
https://bscscan.com/tx/0x974d0fe8609567aea059d2a3f6ece6f07d76e221aa512b4e75dfd5ef1dea9ab6 bot

https://bscscan.com/tx/0x6f18b1b315372792834b0fd173a52eed76cb6b0b2989286f124f11ed3bd4849c tx 

https://bscscan.com/tx/0x0ec640b750a220410dd8f1bad099094ccc4700e25432b9a1ce7dcee015a8bb7a tx amount out

https://bscscan.com/tx/0x4c86be72f5bc037e5cdf9a470e77e93b563ea047ae9b5b75be17d596c897b2ca tx amount out
https://bscscan.com/tx/0x16d2e4bc61dc3f2b0d6f2b285fa10023500a71192b7ad6c620807f415c12acac bot  10.7

https://bscscan.com/tx/0xab125229472017c6defb3eb3bd806acc54cf75ef28ffe0b615878d7fe7988b3e tx amount out

https://bscscan.com/tx/0x9e11a2b54de31ede2ab56bd5b4500eeb223f5210b0f8aea08e49225d12d58251 tx amount out

https://bscscan.com/tx/0x8a3874f07e4c35e3d6034f1c9fc35f50bcaa35ce0b442355f40a210cfa0d317f tx amount out

https://bscscan.com/tx/0x8fe2e1db28a716fff653d96364a58ada4009c6b63ace1c4b4b1472482ac96e15 tx amount out 

https://bscscan.com/tx/0xdc39146248938f1d038b7261e79a73e275bcdc47a4e076c9a0c38fa6a705ad0d bot 12

0x4b403756ed8d7103665e15e48eafda1a9d0504cc273b4f3fa597392eeca623ac tx
https://bscscan.com/tx/0x41d4a7f26818cdb9ace7581a162d7b5d6070064da2149e2db9a449c0dc7f5fb3 bot 16

10 g
0xe62afadea188dd4b303b7ed6e7581b47d5002074a57ef7a7a3f6639a42eb04d8
0x2cb4d16437bb18bbad529766ad8bb32479838cba20eb5ae262c8ece279752647
0x17118b637819c3553eeb3ab144ebbd52a1c96c89b7ba79fb0ab391b6390a8198
0x791838208044c4aa890ac19f97410d05b10974acdef4be7131b259ae9c3e468b
0xd3a4bfe62c013c80ac55aecef82b9393e61732743829feb0660c1a55f00f0009
0x8d94fdd0778a4abfbe8fed0e173f07163dc1eed4a378610b2aef813082af6ea5
0x0bcc65462a3abba5bbac42b399e2c690a6a2ca08b8e79aec8660b49596ff661a
0xde498be2dc1513219ce5b5326fff7b2a5b646394855a050f11c515b52d6a66c5
0x37b3ae4eccde96d98e17dbdccdfc1cebd9f2e844366451bc70b40d4bc31e34e7
0x7342c2402439fad912d53d4e7ec975a49e42d4d0d8db82c3b2b48967c8791e85


11 g
0xf18e9bb5bd55ffb0837eb7f88b2483bb6906d5110a5c2d10845a4dd38977c1bc 
0xb0a981b3dacccbff5096085b775496c2e6e22dca1ddb2f48b0287dee9a7c7aba


12 g
0xd28ea35b701b759f4a5bf71df11c0a1b36c4187544c1057b4db5bfba924580a1

13 g
0x18007767e560500e832711fd6b327bf4725663241ebada2ddcc23b63b0b2bfe8 13
0x87db9972823db74908e66f63d8cb85bae600438fe3e964d8ababc663c2caa004
0xd86dc23806820e463f3f57a1ed082614c58311481d0737da6761cac38f418564
0x082e1ff1ffc333722b2d880d4a624476bb56c2615f71ed70e697331b13f95323
0x85f3c64ce3ec8b66323780e7554e83790889f036f16af1f91fa07a99b0e2657a
0xfa53e441a9ba7f443838053b0b61fe2dd772ecb2a298cf2fd3219f4387fc1ec9

0xbc452d7b7452aeea3ebf1279e0d3a3d5990cb27d542275be0b4d5fec962b998a
https://bscscan.com/address/0xbd67d157502a23309db761c41965600c2ec788b2#code 发现有人写了合约

0xd3ad7d057b35852dc5b88cf792cc236240dc5ad020381ca1d2694b5ab7f52867
0x82699cf4bd5d2bff6def33dd94a9d23aa51ab74d84255c1b9807dc5834d99534
0x18007767e560500e832711fd6b327bf4725663241ebada2ddcc23b63b0b2bfe8


15g
0x5525069ec5f69fc816d9bbe895d55466545b862c6ec0ba53fb0406da8116cc68 amount out
0x69f1aea22596ff9d8729e0f43d2cab245156112ad359c4b1142ae6faa0821f62
0x5525069ec5f69fc816d9bbe895d55466545b862c6ec0ba53fb0406da8116cc68
0x792fc2d3c7c6725fa2011e8695c655c1a744e5a29ea4e1585d6be32e10bbb0c6

0x49bacb2c41d149b15e14d4b4ef5878251b66ca59f5baeb438a7bf37791b17a94  TRANSFER_FROM_FAILED
0xad65eec9396794c50078d1b755ba12f547281699f45321c2c847687bc1c1a0c6
0x7c66d99c7d40a968bbe5d48a53bc9eefc37ed03e007ccc4e1ad8ddec0843678a
0x6ce1643b72558ee8e8ba1e24b859750183645ba01717ef0a8b4198e95c3e804d

16g
0x3ed45947193bbf040d72e63168b13642f0917f1eb0603c49e4bdf608ce282b53
0xfce2867ff5265eb9464e4d586e8eadee6cfb3bf1a7c62ec342a18f660176c288
0x367706f3038b2974dcfaa214873db677b29391317349c0a9de427c7bc7856cd3
0x56293e9f34199e3f17c045c573128f77f8925e71c44a96a1555ec75487e7a128
0xfe49ef2455395cdf7d8989c9ce559cb8825e54292debf979d05b584b9cbac1a9
0xdd15c15622420421111f4e894c1041044d74f3c480653e2a6e77f3618f986d45

17g
0x821b0451a062e2add57621106c8f866c816140a7dd5fa5b3756065dff5e1b8c6 amount out
https://bscscan.com/tx/0x37fd0b5b04bdbdd6384b5ae1ca924de47607a2b8c19ea435e4e019712d94e333 bot 20g 私密合约
https://bscscan.com/tx/0xa4e2a88f0c39dc39b79618cb2af850a4105b8f833e53a49fa163c81ddff42a92


0x58895b9ebd6acc6ac84eda544a0ac7402198098e6f1cb868e1cb3b59e77e7786 amount out
0x3e97bdfee8112242b5721dc240003bc0b81063701a6aa0cedaa4bd751fb4d300
0x58895b9ebd6acc6ac84eda544a0ac7402198098e6f1cb868e1cb3b59e77e7786
0x2c1cdfb71217b006d2076ffd559bfeecf68b86fcb7c8571282f9fb65be1a703e
0x7f260fb76d2f28209eed9002d7078adc0c7b0a3dba6110f1057885277831859d
0x87d2806f71945e334e7b65a5def5ade67d5566061f9a636a4807db658d080855
0xc74b24dcf0bd378c905200e4ef950e11225cef4183191ad6ce2bf034e038ad14


0x2c1cdfb71217b006d2076ffd559bfeecf68b86fcb7c8571282f9fb65be1a703e TRANSFER_FROM_FAILED 
可能原因： 单机跑了三个实例 余额被占用 没有币 或者没有授权
0x7f260fb76d2f28209eed9002d7078adc0c7b0a3dba6110f1057885277831859d
0xebc43efb56d5426cd73b116aaf2a6b9e61ca86c236c255674f416b358995ce19
0x1dec3b533d40325f596f3272b54e98de7fa4f42e2e8cdd9a59e49694cd02e9fc
  function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

0x3e97bdfee8112242b5721dc240003bc0b81063701a6aa0cedaa4bd751fb4d300 amount out


17g 
0x6009f706ca550b01cfd5a74df9d6f2951937ef652cef52290ca49b7f1f0ac5e8 amount out
https://bscscan.com/tx/0x2dd981746a6a7779eddc708c28db6e48159d7a778dacd08531a0afa6dbb18fb6


https://justliquidity.org/earns 不少抢跑交易会被发送到这个合约上
https://bscscan.com/address/0xbd67d157502a23309db761c41965600c2ec788b2#code
 *Submitted for verification at BscScan.com on 2020-10-04


https://bscscan.com/tx/0xc8b829b61c1595a633336acd40736d4ef8cd2b8e40bf56d34e837ccdfdd20781



## 03-17 优化整理
* 生成路径
	 失败路径中的pair 要从allPair中删除 以免浪费资源 //ok
	 生成可以使用chan //速度是一样的  如果并发执行 会遇到网络问题 还有并发数据问题  //进一步优化需要好好梳理一下

* 初始化时打印出当前的所有路径数量 pair数量 //ok

## 03-18 
* 日志配置成方便阅读的 
* 发布订阅的修改订阅方  //ok
* 整理异步部分到函数 //ok
* 整理类图  看看要不要调整代码
* 打印区块号
	对比发现利润的区块 和 实际打包的区块 
	观察
	当前区块为发现的时间
	后两个区块为截止的时间
	抢跑的可能在后面的区块
	https://github.com/ethereum/go-ethereum/issues/15210
	https://github.com/ethereum/go-ethereum/pull/16912
	https://github.com/ethereum/go-ethereum/pull/17662
* Nonce=303 Nonce=303 相同会报错  //已修改 需要测试
	ERRO[2021-03-18T11:51:45+08:00] swap errorreplacement transaction underpriced
	需要提高10%油费


```go
// BlockByHash returns the given full block.
//
// Note that loading full blocks requires two requests. Use HeaderByHash
// if you don't need all transactions or uncle headers.
func (ec *Client) BlockByHash(ctx context.Context, hash common.Hash) (*types.Block, error) {
	return ec.getBlock(ctx, "eth_getBlockByHash", hash, true)
}

// BlockByNumber returns a block from the current canonical chain. If number is nil, the
// latest known block is returned.
//
// Note that loading full blocks requires two requests. Use HeaderByNumber
// if you don't need all transactions or uncle headers.
func (ec *Client) BlockByNumber(ctx context.Context, number *big.Int) (*types.Block, error) {
	return ec.getBlock(ctx, "eth_getBlockByNumber", toBlockNumArg(number), true)
}

// TransactionByHash returns the transaction with the given hash.
func (ec *Client) TransactionByHash(ctx context.Context, hash common.Hash) (tx *types.Transaction, isPending bool, err error) {
	var json *rpcTransaction
	err = ec.c.CallContext(ctx, &json, "eth_getTransactionByHash", hash)
	if err != nil {
		return nil, false, err
	} else if json == nil {
		return nil, false, ethereum.NotFound
	} else if _, r, _ := json.tx.RawSignatureValues(); r == nil {
		return nil, false, fmt.Errorf("server returned transaction without signature")
	}
	if json.From != nil && json.BlockHash != nil {
		setSenderFromServer(json.tx, *json.From, *json.BlockHash)
	}
	return json.tx, json.BlockNumber == nil, nil
}


// TransactionReceipt returns the receipt of a transaction by transaction hash.
// Note that the receipt is not available for pending transactions.
func (ec *Client) TransactionReceipt(ctx context.Context, txHash common.Hash) (*types.Receipt, error) {
	var r *types.Receipt
	err := ec.c.CallContext(ctx, &r, "eth_getTransactionReceipt", txHash)
	if err == nil {
		if r == nil {
			return nil, ethereum.NotFound
		}
	}
	return r, err
}
```

明显被抢跑
0x132133aeabdc2d8464dd4e4d56e78781b9dc93084252f62f91b1cb547092fcf8  11.000001
https://bscscan.com/tx/0xd95d1da52f3e724f6b8b350eb05bb050f16ce5d772e3118bd85ee711da20fbfe 11.000005

0xe543aa571e7b4e01fda11530bf5a1ec89e00ef0008af1233bb74e7cc01b883b6   11.000006
https://bscscan.com/tx/0xf31f31957dbb6e570427d324c4e8e59866f81e623599c29006d841231658f360  20.700000009

# 竟然跑成一笔。。犀利
https://bscscan.com/tx/0x3bd377d9e10aac3eb111f850aaf78352ff3a21b231be720352e27da9293e5837


## 03-19
* validator 的设计是不是有问题
	修改chain validator //ok
	可以用责任链模式

* 测试下nonce分支的问题
	能跑起来就可以 注意控制器的数据 //有待并发数据出来的结果
	修改
	得添加时间
	还要检查 加了油费之后 还有没有利润 //ok
	check profit //ok

* 思考减小失败率的办法
	想几个交易策略
	不同的价格

* profitCheck 等等这些bit.int的计算还可以再优化

又一个 bot ?
0xbe1c1b11d7946fc7c1da79d61fb9572d762836ad46f54e48a6c978f298c8dc99
https://bscscan.com/tx/0xfcab2f95068ce1f5408aa555e0d561ae7d87e1b5246d8ef0121333d451018366


## 03-20
* 自动查询余额
	从余额中进行其他策略
* 整理最大余额策略  
	应该找出获利最大而不是输出最大

0x8eb1ac80db92e37232c7603cc3b2b4318099f4b1c33530d7005ec3f28b735662
https://bscscan.com/tx/0x683d37ff960107ec9a29c436eea690c4a2919ca9d4ab47fa78618327b11d3828
* pending池 需要节点 
	* [How to get pending transactions with using geth or other client?](https://ethereum.stackexchange.com/questions/80552/how-to-get-pending-transactions-with-using-geth-or-other-client)


## 03-22
* 余额策略整理
* 起个本地bsc节点
* 阅读源码 //ok 阅读三个？章节
* 先实现个链 //阅读了4个章节
	起个项目 //ok
	边做边看吧 //ok
 
sort.Strings(monitored)
	if cols := len(monitored) / ctx.Int(monitorCommandRowsFlag.Name); cols > 6 {
		utils.Fatalf("Requested metrics (%d) spans more that 6 columns:\n - %s", len(monitored), strings.Join(monitored, "\n - "))
	}
哈哈哈哈 源码里有错误

## 03-23
* 实现go区块链
	第一章 block hash//ok
	第二章 proof of work //ok
	第三章 persistence and cli //

* 看一下bsc打包规则
	文档上没有找到 
	还是要看以太坊的
	或者源码
* 本地搭一个节点 //OK
	查看相关配置
	https://docs.binance.org/smart-chain/developer/fullnode.html
	需要的配置有点高
	比较占用带宽


## 03-24
网络

竞速
排序打包
程序层面

跑几笔交易去对应的池子里面看看是什么原因



## bot 注意事项
* 确定有待交换的 token erc20 支付gas的token
* 例如 wbnb bnb
* 确认 待交换token 已经授权 且额度够用
* 确认 收支账户 是否配置正确 包括配置文件及代码中
* 确认 放入生成路径的token 小数位正确






## 后续
* 尝试控制利润 增大gas 看看能不能 减小失败率 作为一种交易策略  看看能不能做成策略模式

* 控制利润 譬如如果cover3笔交易即可发起交易
* 通过不同的amount in 发现更多机会 譬如 0.1 0.5 1.0
* 精简合约 压低成本 以发现更多机会

* 失败分析 另外写一个工具 接收bot的交易数据 分析失败原因
* front run bot 找找这方面资料看看怎么抢跑
* 路径生成测试 //ok
* 在与其他人竞争的时候要考虑的因素
	gprice价格
		其他普通用户 比他们高一点就可以
		其他机器人 要比他们设定的高
			通过获取交易失败的区块链上的 有影响的交易 的g price 分析
			获取他们的价格 在我的交易之前的 合约是router的 价格时间 所影响的对
			收集数据 分析数据
	网络

	
* pair的检测
	初始化检测  //ok
	终止时也应该销毁链

* 确认交易的安全 //ok
	上线后 发起一笔交易后 可以确认油费
	默认情况下 除非油费差很大 否则不会出现什么太大问题

* 日志输出到文件
	输出到elk 或者 
	https://prometheus.io/docs/visualization/grafana/
	https://cloud.elastic.co/deployments/create

* pair生成放在 chain里还是 单独生成呢
	1、 排列组合生成  会有不存在的pair
	2、 从factory 拿所有pair 会有路径用不到的 //还是生成好

* 路径生成 //ok
* pair生成时检测是否有流量 没有流量就取消该chain的初始化  //ok
* 需要把交易从事件到发起 都简化 
* 需要worker pool  消费交易事件

* 多路径测试  //ok
* pair chain 的subEvent要不要放到 对象的run方法里  整理日志的时候 可以放进去 这样输出好一点  //ok

* 自己发点token 玩下
* 路径生成 并且测试该路径是否能获得合法pair(存在 且有流动性)  //ok
* 检查pair是否存在 是否有量 在初始化的时候 //ok
* 计算gas 一个池子 110000 * gas price //ok
* 发起交易 //ok
* 连接测试网 //ok
* 失败成功处理
* 代码完善整理
* 分布式bot
* 链上bot
* 交易时机 微分
* 交易统计 分析交易结果 关闭交易或者优化 等等 放在每一个chain上
* 整理该文档
* chain 和 pair的 关停 关闭routine
* 同nonce串 geth tx排序 pending池 





#### 熟悉truffle



## 参考资料


* [what is an amm](https://academy.binance.com/en/articles/what-is-an-automated-market-maker-amm)

* [a guide to pancakeswap](https://academy.binance.com/en/articles/a-guide-to-pancakeswap)

* [pancakeswap docs](https://docs.pancakeswap.finance/)

* [pancakeswap github](https://github.com/pancakeswap)

* [bsc developer guide](https://docs.binance.org/smart-chain/developer/deploy/remix.html)

* [bsc scan](https://bscscan.com/)

* [Use MetaMask For Binance Smart Chain](https://docs.binance.org/smart-chain/wallet/metamask.html)

* [Uniswap V2 Overview](https://uniswap.org/blog/uniswap-v2/)

* [uniswap v2 core](https://github.com/Uniswap/uniswap-v2-core)

* [以太坊：什么是ERC20标准？](https://www.jianshu.com/p/a5158fbfaeb9)

* [etherscan](https://etherscan.io/)

* [geth getting started](https://geth.ethereum.org/docs/getting-started)

* [geth Private Networks](https://geth.ethereum.org/docs/interface/private-network)

* [Local Binance Smart Chain Network](https://docs.binance.org/smart-chain/developer/deploy/local.html)

* [ETHEREUM FOR GO DEVELOPERS](https://ethereum.org/zh/developers/docs/programming-languages/golang/)

* [Develop Ethereum applications with Go](https://goethereumbook.org/zh/)

* [A Step By Step Guide To Testing and Deploying Ethereum Smart Contracts in Go](https://hackernoon.com/a-step-by-step-guide-to-testing-and-deploying-ethereum-smart-contracts-in-go-9fc34b178d78)

* [智能合约开发环境搭建及Hello World合约](https://www.cnblogs.com/tinyxiong/p/7898599.html)

* [ETH官方客户端Geth的使用(一)](https://www.jianshu.com/p/e1292dcc72c1)

* [通过Geth搭建多节点私有链](https://mobilesite.github.io/2018/03/11/geth-private-chain/)

* [250+ Ethereum Developer Tools List](https://simpleaswater.com/ethereum-developer-tools-list/)

* [Go Contract Bindings](https://geth.ethereum.org/docs/dapp/native-bindings)

* [A Definitive List of Ethereum Developer Tools](https://media.consensys.net/an-definitive-list-of-ethereum-developer-tools-2159ce865974)

* [Go Ethereum Docs](https://pkg.go.dev/github.com/ethereum/go-ethereum#Subscription)

* [ethereum中go-event库的使用](https://www.jianshu.com/p/115f0fb78e56?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

* [TRUFFLE OVERVIEW](https://www.trufflesuite.com/docs/truffle/getting-started)

* [uniswap whitepaper](https://uniswap.org/whitepaper.pdf)

* [uniswap contracts docs](https://uniswap.org/docs/v2/smart-contracts/factory)

* [uniswap docs](https://uniswap.org/docs/v2)

* [Go - Divide big.Float](https://stackoverflow.com/questions/36797819/go-divide-big-float)

* [go big docs](https://golang.org/pkg/math/big/#Int.Div)

* [直接计算出pair地址](https://uniswap.org/docs/v2/smart-contract-integration/getting-pair-addresses/)

* [bsc JSON-RPC Endpoint](https://docs.binance.org/smart-chain/developer/rpc.html)

* [How to decode Log.Data in Go](https://ethereum.stackexchange.com/questions/28637/how-to-decode-log-data-in-go)

* [以太坊源码分析](https://blog.csdn.net/zhichunqi/article/details/81810842)

* [Go-ethereum 源码解析之 core/types/log.go](https://www.jianshu.com/p/4b0f7792d45e?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

* [读取erc20事件](https://goethereumbook.org/zh/event-read-erc20/)

* [event-read](https://goethereumbook.org/zh/event-read/)

* [brew install java](https://devqa.io/brew-install-java/)

* [一键解决 go get golang.org/x 包失败](https://shockerli.net/post/go-get-golang-org-x-solution/)

* [golang.org/x一键安装脚本](https://segmentfault.com/a/1190000016310831)

* [understanding init in go](https://www.digitalocean.com/community/tutorials/understanding-init-in-go)

* [MEV and Me](https://research.paradigm.xyz/MEV)

* [阶乘](https://zh.wikipedia.org/wiki/%E9%9A%8E%E4%B9%98)

* [组合数学](https://zh.wikipedia.org/wiki/%E7%BB%84%E5%90%88%E6%95%B0%E5%AD%A6)

* [排列](https://zh.wikipedia.org/wiki/%E7%BD%AE%E6%8F%9B)

* [组合](https://zh.wikipedia.org/wiki/%E7%B5%84%E5%90%88)

* [Go语言实现的排列组合问题实例(n个数中取m个)](https://cloud.tencent.com/developer/article/1073631)

* [goLang 实现排列组合的代码](https://golangnote.com/topic/85.html)

* [无重复字符串的排列组合（golang）](https://www.jianshu.com/p/b0f8547b971b)

* [有重复字符串的排列组合(golang)](https://studygolang.com/articles/29673)

* [测试网获取测试币](https://faucet.ropsten.be/)

* [TRANSFERS AND APPROVAL OF ERC-20 TOKENS FROM A SOLIDITY SMART CONTRACT](https://ethereum.org/en/developers/tutorials/transfers-and-approval-of-erc-20-tokens-from-a-solidity-smart-contract/)

* [erc 20 token implement](https://github.com/ConsenSys/Tokens/blob/fdf687c69d998266a95f15216b1955a4965a0a6d/contracts/eip20/EIP20.sol)

* [ethereum eips](https://github.com/ethereum/EIPs)

* [erc20 token zhihu](https://zhuanlan.zhihu.com/p/35499387)

* [bsc binance](https://testnet.binance.org/faucet-smart)

* [深入Golang之sync.Pool详解](https://www.cnblogs.com/sunsky303/p/9706210.html)

* [Go Commons Pool](https://github.com/jolestar/go-commons-pool)

* [Go Commons Pool发布以及Golang多线程编程问题总结](http://jolestar.com/go-commons-pool-and-go-concurrent/)

* [Apache Commons Pool](https://commons.apache.org/proper/commons-pool/examples.html)

* [纪念](https://rinkeby.etherscan.io/tx/0x6fb3e9711399cb622a63c18b0349e6e16d88eacdd7269dce3f939fa7cad656c3)

* [Using git-flow to automate your git branching workflow](https://jeffkreeftmeijer.com/git-flow/)

* [gitflow](https://github.com/nvie/gitflow)

* [Go进阶10:logrus日志使用教程](https://mojotv.cn/2018/12/27/golang-logrus-tutorial)

* [logrus hooks](https://github.com/sirupsen/logrus/wiki/Hooks)

* [Git is not working after macOS Update (xcrun: error: invalid active developer path](https://stackoverflow.com/questions/52522565/git-is-not-working-after-macos-update-xcrun-error-invalid-active-developer-pa)

* [pancake bot tx](https://bscscan.com/tx/0x76b4a4c33c280c3ed760f77d44ceac19d3c61b2042156bfd7b0520f198372181)

* [Go 语言标准库中 atomic.Value 的前世今生](https://blog.betacat.io/post/golang-atomic-value-exploration/)

* [front running](https://www.coindesk.com/new-research-sheds-light-front-running-bots-ethereum-dark-forest)

* [How to get pending transactions with using geth or other client?](https://ethereum.stackexchange.com/questions/80552/how-to-get-pending-transactions-with-using-geth-or-other-client)

* [Concurrency patterns for account nonce](https://ethereum.stackexchange.com/questions/39790/concurrency-patterns-for-account-nonce?newreg=8f20ebbac792457c9fa22141ef5e9ed4)

* [以太坊设计与源代码之美](https://zhuanlan.zhihu.com/p/58212573)

* [go-ethereum 源码笔记（概览）](https://knarfeh.com/2018/03/10/go-ethereum%20%E6%BA%90%E7%A0%81%E7%AC%94%E8%AE%B0%EF%BC%88%E6%A6%82%E8%A7%88%EF%BC%89/)

* [欢迎来到以太坊中文维基页](https://github.com/ethereum/wiki/wiki/%5BSimplified-Chinese%5D-Ethereum-TOC)

* [以太坊设计原理](https://eth.wiki/en/fundamentals/design-rationale)

* [geth 1.10 blog](https://blog.ethereum.org/2021/03/03/geth-v1-10-0/)

* [币安智能链](https://github.com/binance-chain/whitepaper/blob/master/%E5%B8%81%E5%AE%89%E6%99%BA%E8%83%BD%E9%93%BE.md)