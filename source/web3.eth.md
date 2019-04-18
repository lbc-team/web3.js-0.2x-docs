# 以太坊交互对象 eth

 **web3.eth** 包含以太坊区块链进行交互方法

## web3.eth.defaultAccount

```js
    web3.eth.defaultAccount
```

设置在使用下述方法时使用的默认账户地址：

- [web3.eth.sendTransaction()](#web3-eth-sendtransaction)
- [web3.eth.call()](#web3-eth-call)

### 可能的取值

`String`, 20个字节 - 返回我们拥有的地址 *默认是* `undefined`.

### 返回值

`String`, 20字节的当前设置的默认地址.

### 示例

```js
var defaultAccount = web3.eth.defaultAccount;
console.log(defaultAccount); // ''

// 设置默认地址
web3.eth.defaultAccount = '0x8888f1f195afa192cfee860698584c030f4c9db1';
```

***

## web3.eth.defaultBlock

    web3.eth.defaultBlock

在以下方法中会使用该参数来指定块，不过你可以传入`defaultBlock`参数来 改变方法的目标块:

- [web3.eth.getBalance()](#web3-eth-getbalance)
- [web3.eth.getCode()](#web3-eth-getcode)
- [web3.eth.getTransactionCount()](#web3-eth-gettransactioncount)
- [web3.eth.getStorageAt()](#web3-eth-getstorageat)
- [web3.eth.call()](#web3-eth-call)
- [contract.myMethod.call()](#contract-methods)
- [contract.myMethod.estimateGas()](#contract-methods)

### 可能的取值

- `Number` - 块号
- `String` - `"earliest"`, 创世块
- `String` - `"latest"`, 最后一个块，区块链的当前最新块，是*默认值*
- `String` - `"pending"`, 当前挖掘中的块，包括其中处于pending状态的交易

### 返回值

`Number|String` - 查询状态时使用的默认块号.

### 示例

```js
var defaultBlock = web3.eth.defaultBlock;
console.log(defaultBlock); // 'latest'

// set the default block
web3.eth.defaultBlock = 231;
```

***

## web3.eth.syncing

```js
    web3.eth.syncing
    // or async
    web3.eth.getSyncing(callback(error, result){ ... })
```

只读属性，当节点与网络已经同步时返回一个同步对象，否则返回`false`.

### 返回值

`Object|Boolean` - 同步时返回的对象具有如下字段，否则返回`false`:
   - `startingBlock`: `Number` - 同步开始时的块号.
   - `currentBlock`: `Number` - 当前已经同步的最后一个块号.
   - `highestBlock`: `Number` - 需要同步的最后块号，这是一个估计值.

### 示例

```js
var sync = web3.eth.syncing;
console.log(sync);
/*
{
   startingBlock: 300,
   currentBlock: 312,
   highestBlock: 512
}
*/
```

***

## web3.eth.isSyncing

    web3.eth.isSyncing(callback);

该函数将在每次同步开始、更新和停止时调用指定的回调函数.

### 返回值

`Object` - 返回对象syncing具有如下方法:

  * `syncing.addCallback()`: 添加另一个回调函数, 当节点开始或停止同步时将被调用.
  * `syncing.stopWatching()`: 停止调用回调函数.

### 回调函数的返回值

- `Boolean` - 当同步开始时返回 `true` 停止时返回`false` .
- `Object` - 在同步过程中将返回syncing对象，有以下字段：
   - `startingBlock`: `Number` - 同步开始时的块号.
   - `currentBlock`: `Number` - 当前已经同步的最后块号.
   - `highestBlock`: `Number` - 预估需要同步的块号.


### 示例

```js
web3.eth.isSyncing(function(error, sync){
    if(!error) {
        // stop all app activity
        if(sync === true) {
           // we use `true`, so it stops all filters, but not the web3.eth.syncing polling
           web3.reset(true);

        // show sync info
        } else if(sync) {
           console.log(sync.currentBlock);

        // re-gain app operation
        } else {
            // run your app init function...
        }
    }
});
```

***

## web3.eth.coinbase

```js
    // 同步调用：
    web3.eth.coinbase
    // 异步调用：
    web3.eth.getCoinbase(callback(error, result){ ... })
```

只读属性，返回接收挖矿回报的账户地址.

### 返回值

`String` -节点旳coinbase地址.

### 示例

```js
var coinbase = web3.eth.coinbase;
console.log(coinbase); // "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
```

***

## web3.eth.mining

    web3.eth.mining
    // or async
    web3.eth.getMining(callback(error, result){ ... })


只读属性，返回true或false表示节点是否在挖矿。


### 返回值

`Boolean` - 如果节点在挖矿则返回`true` ，否则返回`false`.

### 示例

```js
var mining = web3.eth.mining;
console.log(mining); // true or false
```

***

## web3.eth.hashrate

```js
    // 同步调用：
    web3.eth.hashrate

    // 异步调用：
    web3.eth.getHashrate(callback(error, result){ ... })
```

只读属性，返回节点当前每秒可算出的hash数量.


### 返回值

`Number` - 每秒可算出的哈希值数量.

### 示例

```js
var hashrate = web3.eth.hashrate;
console.log(hashrate); // 493736
```

***

## web3.eth.gasPrice
    同步调用：
    web3.eth.gasPrice
    异步调用：
    web3.eth.getGasPrice(callback(error, result){ ... })


只读属性,返回当前的gas价格,这个值由最近几个块的gas价格的中值决定。

### 返回值

`BigNumber` - 当前的gas价格的[BigNumber](index.html#id9)实例，以wei为单位.

### 示例

```js
var gasPrice = web3.eth.gasPrice;
console.log(gasPrice.toString(10)); // "10000000000000"
```

***

## 帐户列表 accounts

```js
    // 同步调用：
    web3.eth.accounts
    // 异步调用：
    web3.eth.getAccounts(callback(error, result){ ... })
```

只读属性，返回当前节点持有的帐户列表.

### 返回值

`Array` - 节点持有的帐户列表.

### 示例

```js
var accounts = web3.eth.accounts;
console.log(accounts); // ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
```

***

## 区块号 blockNumber

```js
    // 同步调用：
    web3.eth.accounts
    // 异步调用：
    web3.eth.getblockNumber(callback(error, result){ ... })
```

只读属性，返回当前块号.

### 返回值

`Number` - 最新块号.

### 示例

```js
var number = web3.eth.blockNumber;
console.log(number); // 2744
```

***

## web3.eth.register

    web3.eth.register(addressHexString [, callback])

该方法未实现。注册指定地址到web3.eth.accounts中，从而将非私钥持有者 账户关联到一个持有的账户，例如合约钱包。

### 参数

1. `addressHexString`: `String` - 要注册的地址字符串.
2. `callback`: `Function` - 可选。如果传入该回掉函数，该方法将通过HTTP异步请求.


### 示例

```js
web3.eth.register("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
```

***

## web3.eth.unRegister

     web3.eth.unRegister(addressHexString [, callback])


该函数未实现。取消一个指定地址的注册。

### 参数

1. `addressHexString`:  `String` - 要取消注册的地址.
2. `callback`: `Function` - 可选。当使用回调函数时函数将执行异步HTTP请求.


### 示例

```js
web3.eth.unregister("0x407d73d8a49eeb85d32cf465507dd71d507100ca")
```

***

## 余额 getBalance

    web3.eth.getBalance(addressHexString [, defaultBlock] [, callback])

返回链上指定地址的账户余额.

### 参数

1. `addressHexString`: `String` - 要查询余额的地址.
2. `defaultBlock`: `Number|String` - (（可选）如果不设置此值，将使用`web3.eth.defaultBlock`设定的块，否则使用指定的块。
3. `callback`:  `Function` - （可选）回调函数，用于支持异步的执行方式。

### 返回值

`String` - 一个包含给定地址的当前余额的[BigNumber](index.html#id9)实例，单位为wei.

### 示例

```js
var balance = web3.eth.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(balance); // instanceof BigNumber
console.log(balance.toString(10)); // '1000000000000'
console.log(balance.toNumber()); // 1000000000000
```

***

## web3.eth.getStorageAt

    web3.eth.getStorageAt(addressHexString, position [, defaultBlock] [, callback])

获取一个地址指定位置的存储内容.

### 参数

1. `addressHexString`: `String` - 要读取内容的地址
2. `position`: `Number` - 存储位置索引.
3. `defaultBlock`: `Number|String` - 可选，使用该参数来覆盖[web3.eth.defaultBlock](#web3-eth-defaultblock)的值.
4. `callback`: `Function` - 可选，使用回调函数时，方法将使用异步HTTP请求。


### 返回值

`String` - 在指定位置的存储内容.

### 示例

```js
var state = web3.eth.getStorageAt("0x407d73d8a49eeb85d32cf465507dd71d507100c1", 0);
console.log(state); // "0x03"
```

***

## web3.eth.getCode

    web3.eth.getCode(addressHexString [, defaultBlock] [, callback])

获取链上指定合约地址的代码.

### 参数

1. `addressHexString`: `String` - 要获得代码的地址.
2. `defaultBlock`: `Number|String` - （可选）如果未传递参数，默认使用web3.eth.defaultBlock定义的块，否则使用指定区块.
3. `callback`: `Function` - 回调函数，用于支持异步的方式执行.

### 返回值

`String` - 回调函数，用于支持异步的方式执行.

### 示例

```js
var code = web3.eth.getCode("0xd5677cf67b5aa051bb40496e68ad359eb97cfbf8");
console.log(code); // "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
```

***

## 区块 getBlock

     web3.eth.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])

返回指定编号或哈希的块.

### 参数

1. `String|Number` - 块号或哈希值，或者为以下字符串常量："earliest"、"latest" 或"pending".
2. `Boolean` - 可选，默认值为false，如果设置为true，返回的快中将包含所有的交易对象, 否则仅返回交易哈希.
3. `Function` - 可选，当使用回调函数时将采用异步HTTP请求来访问节点.

### 返回值

`Object` - 块对象:

  - `number`: `Number` - 块编号.
  - `hash`: `String`, 32字节，块哈希，当块处于pending状态时，值为null.
  - `parentHash`: `String`, 32字节，父块的哈希.
  - `nonce`: `String`, 8字节，生成的POW哈希，块处于pending状态时，值为null.
  - `sha3Uncles`: `String`, 32字节， 块中叔伯数据的SHA3值.
  - `logsBloom`: `String`, 256字节，块中日志的bloom filter，当块处于pending状态时，值为null.
  - `transactionsRoot`: `String`, 32字节，块交易树的根节点.
  - `stateRoot`: `String`, 32字节，块最终状态树的根节点.
  - `miner`: `String`, 20字节，接收挖矿奖励的矿工地址.
  - `difficulty`: `BigNumber` -  块难度.
  - `totalDifficulty`: `BigNumber` -  截止到本块的全链总难度.
  - `extraData`: `String` - 块中的额外数据.
  - `size`: `Number` - 按字节计算的块大小.
  - `gasLimit`: `Number` - 本块允许的最大gas消耗量.
  - `gasUsed`: `Number` - 块中所有交易使用的gas总量.
  - `timestamp`: `Number` - 块校验unix时间戳.
  - `transactions`: `Array` - 交易对象数组，或者是32字节的交易哈希值，取决于参数returnTransactionObjects的值.
  - `uncles`: `Array` - 叔伯块的哈希数组.

### 示例

```js
var info = web3.eth.getBlock(3150);
console.log(info);
/*
{
  "number": 3,
  "hash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "parentHash": "0x2302e1c0b972d00932deb5dab9eb2982f570597d9d42504c05d9c2147eaf9c88",
  "nonce": "0xfb6e1a62d119228b",
  "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "transactionsRoot": "0x3a1b03875115b79539e5bd33fb00d8f7b7cd61929d5a3c574f507b8acf415bee",
  "stateRoot": "0xf1133199d44695dfa8fd1bcfe424d82854b5cebef75bddd7e40ea94cda515bcb",
  "miner": "0x8888f1f195afa192cfee860698584c030f4c9db1",
  "difficulty": BigNumber,
  "totalDifficulty": BigNumber,
  "size": 616,
  "extraData": "0x",
  "gasLimit": 3141592,
  "gasUsed": 21662,
  "timestamp": 1429287689,
  "transactions": [
    "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b"
  ],
  "uncles": []
}
*/
```

***

## web3.eth.getBlockTransactionCount

    web3.eth.getBlockTransactionCount(hashStringOrBlockNumber [, callback])

返回指定区块的交易数量

### 参数

1. `String|Number` - Number|String -（可选）如果未传递参数，默认使用web3.eth.defaultBlock定义的块，否则使用指定区块.
2. `Function` - 回调函数，用于支持异步的方式执行7.

### 返回值

`Number` - 给定区块的交易数量.

### 示例

```js
var number = web3.eth.getBlockTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1
```

***

## web3.eth.getUncle

    web3.eth.getUncle(blockHashStringOrNumber, uncleNumber [, returnTransactionObjects] [, callback])

通过索引或Hask返回叔伯块

### 参数

1. `blockHashStringOrNumber`:`String|Number` - 块编号或哈希值，或者字符串常量："earliest"、"latest" 、 "pending".
2. `uncleNumber``Number` - 叔伯块索引.
3. `returnTransactionObjects``Boolean` - 可选，默认值为false，如果设置为true，则返回的块中将包含完整的所有交易的完整数据，否则仅返回交易的哈希值.
4.`callback` `Function` - 可选，如果使用回调函数，则通过异步http向节点API发送请求。


### 返回值

`Object` -叔伯块对象，内容参考 [web3.eth.getBlock()](#web3-eth-getblock).

**提示** ：叔伯块中不包含单个交易.

### 示例

```js
var uncle = web3.eth.getUncle(500, 0);
console.log(uncle); // see web3.eth.getBlock

```

***

## 获取交易 getTransaction

    web3.eth.getTransaction(transactionHash [, callback])

返回交易hash对应的交易

### 参数

1. `String` - 交易的哈希值.
2. `Function` - [回调函数](index.html#id7)函数，用于支持异步的方式执行.


### 返回值

`Object` -一个交易对象 `transactionHash`:

  - `hash`: `String`, 32字节，交易的哈希值.
  - `nonce`: `Number` - 交易的发起者在之前进行过的交易数量.
  - `blockHash`: `String`, 32字节。交易所在区块的哈希值。当这个区块处于pending将会返回null.
  - `blockNumber`: `Number` - 交易所在区块的块号。当这个区块处于pending将会返回null.
  - `transactionIndex`: `Number` -整数。交易在区块中的序号。当这个区块处于pending将会返回null.
  - `from`: `String`, 20字节，交易发起者的地址.
  - `to`: `String`, 20字节，交易接收者的地址。当这个区块处于pending将会返回null.
  - `value`: `BigNumber` - 交易附带的货币量，单位为Wei.
  - `gasPrice`: `BigNumber` - 交易发起者配置的gas价格，单位是wei.
  - `gas`: `Number` - 交易发起者提供的gas.
  - `input`: `String` - 交易附带的数据.


### 示例

```js
var transaction = web3.eth.getTransaction('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
console.log(transaction);
/*
{
  "hash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "nonce": 2,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "transactionIndex": 0,
  "from": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "to": "0x6295ee1b4f6dd65047762f924ecd367c17eabf8f",
  "value": BigNumber,
  "gas": 314159,
  "gasPrice": BigNumber,
  "input": "0x57cb2fc4"
}
*/

```

***

## 通过区块获取交易 getTransactionFromBlock

    getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])

返回交易hash或区块好对应的交易。

### 参数

1. `hashStringOrNumber`: `String` - 区块号或哈希。或者是earliest，latest或pending。查看[web3.eth.defaultBlock](#web3-eth-defaultBlock)了解可选值.
2. `indexNumber`:  `Number` - 交易的序号.
3. `callback`: `Function` -回调函数，用于支持异步的方式执行.

### 返回值

`Object` - 交易对象，详见[web3.eth.getTransaction](#web3-eth-gettransaction):


### 示例

```js
var transaction = web3.eth.getTransactionFromBlock('0x4534534534', 2);
console.log(transaction); // see web3.eth.getTransaction

```

***

## 获取交易收据 getTransactionReceipt

    web3.eth.getTransactionReceipt(hashString [, callback])

指定一个交易哈希，返回一个交易的收据。需要指出的是，处于pending状态的交易，收据是不可用的.


### 参数

1. `String` -交易的哈希.
2. `Function` -回调函数，用于支持异步的方式执行.

### 返回值

`Object` - 交易的收据对象，如果找不到返回null:

  - `blockHash`: `String`, 32字节，这个交易所在区块的哈希.
  - `blockNumber`: `Number` - 交易所在区块的块号.
  - `transactionHash`: `String`, 32字节，交易的哈希值.
  - `transactionIndex`: `Number` - 交易在区块里面的序号，整数.
  - `from`: `String`,20字节，交易发送者的地址.
  - `to`: `String`, 20字节，交易接收者的地址。如果是一个合约创建的交易，返回null.
  - `cumulativeGasUsed `: `Number ` -当前交易执行后累计花费的gas总值10.
  - `gasUsed `: `Number ` -   执行当前这个交易单独花费的gas.
  - `contractAddress `: `String` -  20字节，创建的合约地址。如果是一个合约创建交易，返回合约地址，其它情况返回null.
  - `logs `:  `Array` - 这个交易产生的日志对象数组.
  - `status `:  `String` - '0x0' indicates transaction failure , '0x1' indicates transaction succeeded.

### 示例

```js
var receipt = web3.eth.getTransactionReceipt('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
console.log(receipt);
{
  "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "transactionIndex": 0,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "contractAddress": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         // logs as returned by getFilterLogs, etc.
     }, ...],
  "status": "0x1"
}
```

***

## 交易数量 getTransactionCount

    web3.eth.getTransactionCount(addressHexString [, defaultBlock] [, callback])

返回指定地址发起的交易数，可用于下一次交易的Nonce值。

### 参数

1. `String` - 要获得交易数的地址.
2. `Number|String` - （可选）如果未传递参数，默认使用 [web3.eth.defaultBlock](#web3-eth-defaultblock)定义的块，否则使用指定区块.
3. `Function` - 回调函数，用于支持异步的方式执行7.

### 返回值

`Number` - 指定地址发送的交易数量.

### 示例

```js
var number = web3.eth.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
console.log(number); // 1
```

***

## 发送交易 sendTransaction

    web3.eth.sendTransaction(transactionObject [, callback])

发送一个交易到网络。如果交易是一个合约创建的，请使用web3.eth.getTransactionReceipt()在交易完成后获取合约的地址。

### 参数

1. `Object` - Object - 要发送的交易对象:

    - `from`: `String` - 指定的发送者的地址。如果不指定，使用web3.eth.defaultAccount.
    - `to`: `String` -（可选）指定的发送者的地址。如果是创建合约，则不需要指定。
    - `value`: `Number|String|BigNumber` -（可选）交易携带的货币量，以wei为单位。如果合约创建交易，则为合约初始余额。
    - `gas`: `Number|String|BigNumber` - （可选）默认是自动确定，交易可使用的gas，未使用的gas会退回。
    - `gasPrice`: `Number|String|BigNumber` - （可选）默认是自动确定，交易的gas价格，默认是网络gas价格的平均值。
    - `data`: `String` - （可选）或者包含相关数据的字节字符串，如果是合约创建，则是初始化要用到的代码。
    - `nonce`: `Number`  - （可选）整数，使用此值，可以允许你覆盖自己的相同nonce（正在pending中的交易）。

2. `Function` - 回调函数，用于支持异步的方式执行.

### 返回值

`String` - 32字节的交易哈希串。用16进制表示.



### 示例

```js

// compiled solidity source code using https://chriseth.github.io/cpp-ethereum/
var code = "603d80600c6000396000f3007c01000000000000000000000000000000000000000000000000000000006000350463c6888fa18114602d57005b6007600435028060005260206000f3";

web3.eth.sendTransaction({data: code}, function(err, transactionHash) {
  if (!err)
    console.log(transactionHash); // "0x7f9fade1c0d57a7af66ab4ead7c2eb7b11a91385"
});
```

***

## 发送离线签名交易 sendRawTransaction

    web3.eth.sendRawTransaction(signedTransactionData [, callback])

发送一个已经签名的交易。比如可以用下述签名的例子。
如果交易是一个合约创建，请使用web3.eth.getTransactionReceipt()在交易完成后获取合约的地址。

### 参数

1. `signedTransacionData``String` - 16进制格式的签名交易数据.
2. `callback``Function` - 回调函数，用于支持异步的方式执行.

### 返回值

`String` - 32字节的16进制格式的交易哈希串.


### 示例

```js
var Tx = require('ethereumjs-tx');
var privateKey = new Buffer('e331b6d69882b4cb4ea581d88e0b604039a3de5967688d3dcffdd2270c0fd109', 'hex')

var rawTx = {
  nonce: '0x00',
  gasPrice: '0x09184e72a000',
  gasLimit: '0x2710',
  to: '0x0000000000000000000000000000000000000000',
  value: '0x00',
  data: '0x7f7465737432000000000000000000000000000000000000000000000000000000600057'
}

var tx = new Tx(rawTx);
tx.sign(privateKey);

var serializedTx = tx.serialize();

//console.log(serializedTx.toString('hex'));
//f889808609184e72a00082271094000000000000000000000000000000000000000080a47f74657374320000000000000000000000000000000000000000000000000000006000571ca08a8bbf888cfa37bbf0bb965423625641fc956967b81d12e23709cead01446075a01ce999b56a8a88504be365442ea61239198e23d1fce7d00fcfc5cd3b44b7215f

web3.eth.sendRawTransaction('0x' + serializedTx.toString('hex'), function(err, hash) {
  if (!err)
    console.log(hash); // "0x7f9fade1c0d57a7af66ab4ead79fade1c0d57a7af66ab4ead7c2c2eb7b11a91385"
});
```

***


## 签名 sign

    web3.eth.sign(address, dataToSign [, callback])

使用指定帐户签名要发送的数据，帐户需要处于解锁状态。

### 参数

1. `String` - 签名使用的地址.
2. `String` - 要签名的数据.
3. `Function` - （可选）[回调](index.html#id7) 函数，用于支持异步的方式执行.

### 返回值

`String` - 签名后的数据.

返回的值对应的是ECDSA（Elliptic Curve Digital Signature Algorithm）签名后的字符串:

```
r = signature[0:64]
s = signature[64:128]
v = signature[128:130]
```

需要注意的是，如果你使用ecrecover，这里的v值是00或01，所以如果你想使用他们，你需要把这里的v值转成整数，再加上27。最终你要用的值将是27或2813.

### 示例

```js
var result = web3.eth.sign(
    "0x9dd2c369a187b4e6b9c402f030e50743e619301ea62aa4c0737d4ef7e10a3d49", // first argument is web3.sha3("xyz")
    "0x135a7de83802408321b74c322f8558db1679ac20");
console.log(result); // "0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400"
```

***

## 消息调用 call

    web3.eth.call(callObject [, defaultBlock] [, callback])

执行一个消息调用交易，消息调用直接在节点旳VM中执行，而不会通过挖矿改写区块链。

### 参数

1. `callObject`: `Object` - 交易对象，参考[发送交易](#web3-eth-sendtransaction)函数的参数, 区别在于对于消息调用而言，from字段是可选的.
2. `defaultBlock`: `Number|String` - Number|String，可选，使用该参数覆盖 [web3.eth.defaultBlock](#web3-eth-defaultblock)的值.
3. `callback`: `Function` -可选，当使用[回调函数](index.html#id7) 函数时将通过异步http请求节点API.

### 返回值

`String` - 调用返回的数据.

### 示例

```js
var result = web3.eth.call({
    to: "0xc4abd0339eb8d57087278718986382264244252f",
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
});
console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"
```

***

## 估算 gasLimit estimateGas

    web3.eth.estimateGas(callObject [, callback])

估计调用需要耗费的gas量。这个方法在节点的VM中执行一个消息调用或交易，但是不会修改区块链.

### 参数

`callObject`： Object - 要发送的交易对象，可包含以下字段：
  - from: String - 指定的发送者的地址。如果不指定，使用web3.eth.defaultAccount。
  - to: String - （可选）交易消息的目标地址，如果是合约创建，则不填.
  - value: Number|String|BigNumber - （可选）交易携带的货币量，以wei为单位。如果合约创建交易，则为初始的基金。
  - gas: Number|String|BigNumber - （可选）默认是自动，交易可使用的gas，未使用的gas会退回。
  - gasPrice: Number|String|BigNumber - （可选）默认是自动确定，交易的gas价格，默认是网络gas价格的平均值 。
  - data: String - （可选）或者包含相关数据的字节字符串，如果是合约创建，则是初始化要用到的代码。
  - nonce: Number - （可选）整数，使用此值，可以允许你覆盖你自己的相同nonce的，正在pending中的交易。
`callback`：Function - 回调函数，用于支持异步的执行方式.

### 返回值

`Number` - 模拟的call/transcation花费的gas.

### 示例

```js
var result = web3.eth.estimateGas({
    to: "0xc4abd0339eb8d57087278718986382264244252f",
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
});
console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"
```

***

## web3.eth.filter

```js
// can be 'latest' or 'pending'
var filter = web3.eth.filter(filterString);
// OR object are log filter options
var filter = web3.eth.filter(options);

// watch for changes
filter.watch(function(error, result){
  if (!error)
    console.log(result);
});

// Additionally you can start watching right away, by passing a callback:
web3.eth.filter(options, function(error, result){
  if (!error)
    console.log(result);
});
```

### 参数

`String|Object` - String|Object - 字符串的可取值[latest，pending]。latest表示监听最新的区块变化，pending表示监听正在pending的区块。如果需要按条件对象过滤，对象字段如下：
- `fromBlock`: `Number|String` - 起始区块号（如果使用字符串latest，意思是最新的，正在打包的区块），默认值是latest.
- `toBlock`: `Number|String` - 终止区块号（如果使用字符串latest，意思是最新的，正在打包的区块），默认值是latest.
- `address`: `String` - 单个或多个地址。获取指定帐户的日志
- `topics`: `Array of Strings` - 在日志对象中必须出现的字符串数组。顺序非常重要，如果你想忽略主题，使用null。如，[null,'0x00...']，你还可以为每个主题传递一个单独的可选项数组，如[null,['option1','option1']]

### 返回值

Object - 有下述方法的过滤对象：
-   filter.get(callback): 返回满足过滤条件的日志。
-   filter.watch(callback): 监听满足条件的状态变化，满足条件时调用回调。
-   filter.stopWatching(): 停止监听，清除节点中的过滤。你应该总是在监听完成后，执行这个操作.

### 监听回调的返回值

- String - 当使用latest参数时。返回最新的一个区块哈希值。
- String - 当使用pending参数时。返回最新的pending中的交易哈希值。
- Object - 当使用手工过滤选项时，将返回下述的日志对象。
  - logIndex: Number - 日志在区块中的序号。如果是pending的日志，则为null。
  - transactionIndex: Number - 产生日志的交易在区块中的序号。如果是pending的日志，则为null。
  - transactionHash: String，32字节 - 产生日志的交易哈希值。
  - blockHash: String，32字节 - 日志所在块的哈希。如果是pending的日志，则为null。
  - blockNumber: Number - 日志所在块的块号。如果是pending的日志，则为null。
  - address: String，32字节 - 日志产生的合约地址。
  - data: string - 包含日志一个或多个32字节的非索引的参数。
  - topics: String[] - 一到四个32字节的索引的日志参数数组。（在Solidity中，第一个主题是整个事件的签名（如，Deposit(address,bytes32,uint256)），但如果使用匿名的方式定义事件的情况除外） 事件监听器的返回结果，见后合约对象的事件。

**注意** 事件过滤的返回值参考下文：[合约事件](#contract-events)

### 示例

```js
var filter = web3.eth.filter('pending');

filter.watch(function (error, log) {
  console.log(log); //  {"address":"0x0000000000000000000000000000000000000000", "data":"0x0000000000000000000000000000000000000000000000000000000000000000", ...}
});

// get all past logs again.
var myResults = filter.get(function(error, logs){ ... });

...

// stops and uninstalls the filter
filter.stopWatching();

```

***

## 合约对象 contract

```js
    web3.eth.contract(abiArray)
```

创建一个合约对象，用来在某个地址上初始化合约实例。

### 参数

1.  `abiArray` : `Array` - 描述合约的函数，事件的ABI对象数组,

### 返回值

`Object` - 一个合约对象，接着就可以使用以下方法来实例化

```js
var MyContract = web3.eth.contract(abiArray);

// 使用合约地址实例化合约
var contractInstance = MyContract.at(address);

// 部署新合约
var contractInstance = MyContract.new([constructorParam1] [, constructorParam2], {data: '0x12345...', from: myAccount, gas: 1000000});

// 获得部署合约数据
var contractData = MyContract.new.getData([constructorParam1] [, constructorParam2], {data: '0x12345...'});
// contractData = '0x12345643213456000000000023434234'
```

可以或者使用一个在某个地址上已经存在的合约，或者使用编译后的字节码部署一个全新的的合约。

```js
// Instantiate from an existing address:
var myContractInstance = MyContract.at(myContractAddress);


// Or deploy a new contract:

// 读取合约文件部署合约
...
const fs = require("fs");
const solc = require('solc')

let source = fs.readFileSync('nameContract.sol', 'utf8');
let compiledContract = solc.compile(source, 1);
let abi = compiledContract.contracts['nameContract'].interface;
let bytecode = compiledContract.contracts['nameContract'].bytecode;
let gasEstimate = web3.eth.estimateGas({data: bytecode});
let MyContract = web3.eth.contract(JSON.parse(abi));

var myContractReturned = MyContract.new({
   from:mySenderAddress,
   data:bytecode,
   gas:gasEstimate}, function(err, myContract){
    if(!err) {
       // NOTE: 将回调两次
       // Once the contract has the transactionHash property set and once its deployed on an address.

       // e.g. check tx hash on the first call (transaction send)
       if(!myContract.address) {
           console.log(myContract.transactionHash) // The hash of the transaction, which deploys the contract

       // check address on the second call (contract deployed)
       } else {
           console.log(myContract.address) // the contract address
       }

       // Note that the returned "myContractReturned" === "myContract",
       // so the returned "myContractReturned" object will also get the address set.
    }
  });

// 同步部署合约: The address will be added as soon as the contract is mined.
// Additionally you can watch the transaction by using the "transactionHash" property
var myContractInstance = MyContract.new(param1, param2, {data: myContractCode, gas: 300000, from: mySenderAddress});
myContractInstance.transactionHash // The hash of the transaction, which created the contract
myContractInstance.address // undefined at start, but will be auto-filled later
```

### 示例

```js
// contract abi
var abi = [{
     name: 'myConstantMethod',
     type: 'function',
     constant: true,
     inputs: [{ name: 'a', type: 'string' }],
     outputs: [{name: 'd', type: 'string' }]
}, {
     name: 'myStateChangingMethod',
     type: 'function',
     constant: false,
     inputs: [{ name: 'a', type: 'string' }, { name: 'b', type: 'int' }],
     outputs: []
}, {
     name: 'myEvent',
     type: 'event',
     inputs: [{name: 'a', type: 'int', indexed: true},{name: 'b', type: 'bool', indexed: false}]
}];

// creation of contract object
var MyContract = web3.eth.contract(abi);

// initiate contract for an address
var myContractInstance = MyContract.at('0xc4abd0339eb8d57087278718986382264244252f');

// call constant function
var result = myContractInstance.myConstantMethod('myParam');
console.log(result) // '0x25434534534'

// send a transaction to a function
myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000});

// short hand style
web3.eth.contract(abi).at(address).myAwesomeMethod(...);

// create filter
var filter = myContractInstance.myEvent({a: 5}, function (error, result) {
  if (!error)
    console.log(result);
    /*
    {
        address: '0x8718986382264244252fc4abd0339eb8d5708727',
        topics: "0x12345678901234567890123456789012", "0x0000000000000000000000000000000000000000000000000000000000000005",
        data: "0x0000000000000000000000000000000000000000000000000000000000000001",
        ...
    }
    */
});
```

***

## 调用合约方法 Contract Methods

```js
// Automatically determines the use of call or sendTransaction based on the method type
myContractInstance.myMethod(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly calling this method
myContractInstance.myMethod.call(param1 [, param2, ...] [, transactionObject] [, defaultBlock] [, callback]);

// Explicitly sending a transaction to this method
myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

// Get the call data, so you can call the contract through some other means
// var myCallData = myContractInstance.myMethod.request(param1 [, param2, ...]);
var myCallData = myContractInstance.myMethod.getData(param1 [, param2, ...]);
// myCallData = '0x45ff3ff6000000000004545345345345..'
```

合约对象内封装了使用合约的相关方法。可以通过传入参数，和交易对象来使用方法。

### 参数

- `param1|param2`: `String|Number|BigNumber` - (可选) 零或多个函数参数。如果传入一个字符串，需要使用十六进制编码，如 "0xdeadbeef"，如果已经创建了一个BigNumber对像可以直接传入
- `transactionObject`: `Object` - (可选) 最后一个参数（如果传了callback，则是倒数第二个参数），可以是一个交易对象。参考[web3.eth.sendTransaction](#web3-eth-sendtransaction)的第一个参数。**注意** 这里不需要填data和to属性。
- `defaultBlock`: `Number|String` - (可选) 如果不设置此值使用[web3.eth.defaultBlock](#web3-eth-defaultblock)设定的块，否则使用指定的块。
- `Function` - (可选) 回调函数，用于支持异步的方式执行。

### 返回值

`String` - 如果发起的是一个call，对应的是返回结果。如果是transaction，则要么是一个创建的合约地址，要么是一个transaction的哈希值。可参考[web3.eth.sendTransaction](#web3-eth-sendtransaction).


### 示例

```js
// creation of contract object
var MyContract = web3.eth.contract(abi);

// initiate contract for an address
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

var result = myContractInstance.myConstantMethod('myParam');
console.log(result) // '0x25434534534'

myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000}, function(err, result){ ... });
```

***


## 监听合约事件 Contract Events

通过合约事件，可以合约状态发生变化时，让外部世界（如DApp）知道变化的发生。

```js
var event = myContractInstance.MyEvent({valueA: 23} [, additionalFilterObject])

// watch for changes
event.watch(function(error, result){
  if (!error)
    console.log(result);
});

// Or pass a callback to start watching immediately
var event = myContractInstance.MyEvent([{valueA: 23}] [, additionalFilterObject] , function(error, result){
  if (!error)
    console.log(result);
});

```

可以像[web3.eth.filters](#web3-eth-filter)这样使用事件，他们有相同的方法，但需要传递不同的对象来创建事件过滤器。

### 参数

1. `Object` - 想要过滤的索引值，如 `{'valueA': 1, 'valueB': [myFirstAddress, mySecondAddress]}`. 默认情况下，所以有过滤项被设置为 `null`. 意味着默认匹配合约所有的日志。
2. `Object` - 附加的过滤选项, 参考 [filters](#web3-eth-filter) 第一个参数. 默认情况下，这个对象会设置address为当前合约地址，同时第一个主题为事件的签名。
3. `Function` - (可选) 传入一个回调函数，将立即开始监听，不用再主动调用 `myEvent.watch(function(){})`。

### 回调函数返回值


`Object` - 返回事件对象:

- `address`: `String`, 32 字节 - 日志产生的合约地址
- `args`: `Object` - 事件的参数
- `blockHash`: `String`, 32 字节 - 日志所在块的哈希。如果是pending的日志，则为null。
- `blockNumber`: `Number` - 日志所在块的块号。如果是pending的日志，则为null。
- `logIndex`: `Number` -  日志在区块中的序号。
- `event`: `String` - 事件名称
- `removed`: `bool` -  标示产生事件的交易是否被移除 (因为孤块) 或从未生效 (因为交易被拒绝).
- `transactionIndex`: `Number` - 产生日志的交易在区块中的序号。
- `transactionHash`: `String`, 32 字节 - 产生日志的交易哈希值。

### 示例

```js
var MyContract = web3.eth.contract(abi);
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'}
var myEvent = myContractInstance.MyEvent({some: 'args'}, {fromBlock: 0, toBlock: 'latest'});
myEvent.watch(function(error, result){
   ...
});

// would get all past logs again.
var myResults = myEvent.get(function(error, logs){ ... });

...

// would stop and uninstall the filter
myEvent.stopWatching();
```

***

## 合约所有事件 Contract allEvents

```js
var events = myContractInstance.allEvents([additionalFilterObject]);

// watch for changes
events.watch(function(error, event){
  if (!error)
    console.log(event);
});

// Or pass a callback to start watching immediately
var events = myContractInstance.allEvents([additionalFilterObject,] function(error, log){
  if (!error)
    console.log(log);
});

```

可监听合约触发的所有事件的回调。

### 参数

1. `Object` - 附加的过滤选项，参考 [filters](#web3-eth-filter) 第一个参数. 默认情况下，这个对象会设置address为当前合约地址，主题为事件的签名，不支持额外的主题。
2. `Function` - (可选) 传入一个回调函数，将立即开始监听，不用再主动调用 `myEvent.watch(function(){})`。

### 回调函数返回值


`Object` - 参考上面[合约事件](#contract-events) 返回值

### 示例

```js
var MyContract = web3.eth.contract(abi);
var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

// watch for an event with {some: 'args'}
var events = myContractInstance.allEvents({fromBlock: 0, toBlock: 'latest'});
events.watch(function(error, result){
   ...
});

// would get all past logs again.
events.get(function(error, logs){ ... });

...

// would stop and uninstall the filter
events.stopWatching();
```

****

## web3.eth.getCompilers

    web3.eth.getCompilers([callback])

获取可用编译器，不过方法被弃用 https://github.com/ethereum/EIPs/issues/209


### 参数

1. `Function` - (可选) 回调函数

### 返回值

`Array` - 可用编译器列表

### 示例

```js
var number = web3.eth.getCompilers();
console.log(number); // ["lll", "solidity", "serpent"]
```

***

## web3.eth.compile.solidity

    web3.eth.compile.solidity(sourceString [, callback])

编译solidity源码

### 参数

1. `String` - solidity源码
2. `Function` - (optional) 回调函数

### 返回值

`Object` - 合约及编译信息


### 示例

```js
var source = "" +
    "contract test {\n" +
    "   function multiply(uint a) 返回值(uint d) {\n" +
    "       return a * 7;\n" +
    "   }\n" +
    "}\n";
var compiled = web3.eth.compile.solidity(source);
console.log(compiled);
// {
  "test": {
    "code": "0x605280600c6000396000f3006000357c010000000000000000000000000000000000000000000000000000000090048063c6888fa114602e57005b60376004356041565b8060005260206000f35b6000600782029050604d565b91905056",
    "info": {
      "source": "contract test {\n\tfunction multiply(uint a) 返回值(uint d) {\n\t\treturn a * 7;\n\t}\n}\n",
      "language": "Solidity",
      "languageVersion": "0",
      "compilerVersion": "0.8.2",
      "abiDefinition": [
        {
          "constant": false,
          "inputs": [
            {
              "name": "a",
              "type": "uint256"
            }
          ],
          "name": "multiply",
          "outputs": [
            {
              "name": "d",
              "type": "uint256"
            }
          ],
          "type": "function"
        }
      ],
      "userDoc": {
        "methods": {}
      },
      "developerDoc": {
        "methods": {}
      }
    }
  }
}
```

***

## web3.eth.compile.lll

    web3.eth.compile.lll(sourceString [, callback])

编译LLL源码

### 参数

1. `String` - LLL源码
2. `Function` - (可选) 回调函数

### 返回值

`String` - 编译的LLL HEX代码


### 示例

```js
var source = "...";

var code = web3.eth.compile.lll(source);
console.log(code); // "0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056"
```

***

## web3.eth.compile.serpent

    web3.eth.compile.serpent(sourceString [, callback])

Compiles serpent source code.

### 参数

1. `String` - The serpent source code.
2. `Function` - (optional) If you pass a callback the HTTP request is made asynchronous. See [this note](#using-callbacks) for details.

### 返回值

`String` - The compiled serpent code as HEX string.


```js
var source = "...";

var code = web3.eth.compile.serpent(source);
console.log(code); // "0x603880600c6000396000f3006001600060e060020a600035048063c6888fa114601857005b6021600435602b565b8060005260206000f35b600081600702905091905056"
```

***

## web3.eth.namereg

    web3.eth.namereg

返回GlobalRegistrar对象

### Usage

查看 [namereg](https://github.com/ethereum/web3.js/blob/master/example/namereg.html) 示例.


***

## web3.eth.sendIBANTransaction

IBAN（International Bank Account Number）：国际银行账号，IBAN 账号是以太坊为了和传统的银行系统对接而引入的概念。
IBAN的作用是为全球任意一家银行中的任意一个账户生成一个全球唯一的账号

```js
var txHash = web3.eth.sendIBANTransaction('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8', 'XE81ETHXREGGAVOFYORK', 0x100);
```

调用`sendIBANTransaction()`方法向IBAN地址发送交易.

### 参数

- `string` - 要发送交易的账户地址
- `string` - 交易的目标IBAN地址
- `value` - 在IBAN交易中要发送的值

***

## web3.eth.iban

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
```

***

### web3.eth.iban.fromAddress



```js
var i = web3.eth.iban.fromAddress('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8');
console.log(i.toString()); // 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS
```

***

### web3.eth.iban.fromBban

```js
var i = web3.eth.iban.fromBban('ETHXREGGAVOFYORK');
console.log(i.toString()); // "XE81ETHXREGGAVOFYORK"
```

***

### web3.eth.iban.createIndirect

```js
var i = web3.eth.iban.createIndirect({
  institution: "XREG",
  identifier: "GAVOFYORK"
});
console.log(i.toString()); // "XE81ETHXREGGAVOFYORK"
```

***

### web3.eth.iban.isValid

```js
var valid = web3.eth.iban.isValid("XE81ETHXREGGAVOFYORK");
console.log(valid); // true

var valid2 = web3.eth.iban.isValid("XE82ETHXREGGAVOFYORK");
console.log(valid2); // false, cause checksum is incorrect

var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var valid3 = i.isValid();
console.log(valid3); // true

```

***

### web3.eth.iban.isDirect

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var direct = i.isDirect();
console.log(direct); // false
```

***

### web3.eth.iban.isIndirect

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var indirect = i.isIndirect();
console.log(indirect); // true
```

***

### web3.eth.iban.checksum

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var checksum = i.checksum();
console.log(checksum); // "81"
```

***

### web3.eth.iban.institution

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var institution = i.institution();
console.log(institution); // 'XREG'
```

***

### web3.eth.iban.client

```js
var i = new web3.eth.iban("XE81ETHXREGGAVOFYORK");
var client = i.client();
console.log(client); // 'GAVOFYORK'
```

***

### web3.eth.iban.address

```js
var i = new web3.eth.iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS');
var address = i.address();
console.log(address); // '00c5496aee77c1ba1f0854206a26dda82a81d6d8'
```

***

### web3.eth.iban.toString

```js
var i = new web3.eth.iban('XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS');
console.log(i.toString()); // 'XE7338O073KYGTWWZN0F2WZ0R8PX5ZPPZS'
```
