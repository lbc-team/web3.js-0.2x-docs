# 主对象 web3 

  在使用 web3 之前，请先确保正确[引入web3.js](index.html#import-web3)。
  
## web3

 `web3` 对象提供了所有方法

### 示例

```js
var Web3 = require('web3');
// create an instance of web3 using the HTTP provider.
// NOTE in mist web3 is already available, so check first if it's available before instantiating
var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
```

#### 示例 using HTTP Basic Authentication

```js
var Web3 = require('web3');
var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545", 0, BasicAuthUsername, BasicAuthPassword));
//Note: HttpProvider takes 4 arguments (host, timeout, user, password)
```

## web3.version

### web3.version.api

```js
web3.version.api
```

### 返回值

`String` - 以太坊js的api版本.

### 示例

```js
var version = web3.version.api;
console.log(version); // "0.2.0"
```

***

### web3.version.node

    web3.version.node
    // or async
    web3.version.getNode(callback(error, result){ ... })


#### 返回值

`String` - 客户端或节点的版本信息.

#### 示例

```js
var version = web3.version.node;
console.log(version); // "Mist/v0.9.3/darwin/go1.4.1"
```

***

### web3.version.network

    web3.version.network
    // or async
    web3.version.getNetwork(callback(error, result){ ... })


#### 返回值

`String` - 网络协议版本.

#### 示例

```js
var version = web3.version.network;
console.log(version); // 54
```

***

### web3.version.ethereum

    web3.version.ethereum
    // or async
    web3.version.getEthereum(callback(error, result){ ... })


#### 返回值

`String` - 以太坊的协议版本.

#### 示例

```js
var version = web3.version.ethereum;
console.log(version); // 60
```

***

### web3.version.whisper

    web3.version.whisper
    // or async
    web3.version.getWhisper(callback(error, result){ ... })


#### 返回值

`String` - Twhisper协议的版本.

#### 示例

```js
var version = web3.version.whisper;
console.log(version); // 20
```

***
## web3.isConnected

    web3.isConnected()

检查是否与节点连接上

### 参数
无

### 返回值

`Boolean`:true表示已连接

### 示例

```js
if(!web3.isConnected()) {

   // show some dialog to ask the user to start a node

} else {

   // start web3 filters, calls, etc

}
```

***

## web3.setProvider

    web3.setProvider(provider)

设置网络通信服务提供对象.

### 参数
none

### 返回值

`undefined`:通信服务提供对象

### 示例

```js
web3.setProvider(new web3.providers.HttpProvider('http://localhost:8545')); // 8080 for cpp/AZ, 8545 for go/mist
```

***

## web3.currentProvider

    web3.currentProvider

如果已经设置了Provider，则返回当前的Provider。这个方法可以用来检查在使用mist浏览器等情况下已经设置过Provider，避免重复设置的情况.

### 返回值

`Object` - null 或 已经设置的Provider对象;

### 示例

```js
// Check if mist etc. already set a provider
if(!web3.currentProvider)
    web3.setProvider(new web3.providers.HttpProvider("http://localhost:8545"));

```

***

## web3.reset

    web3.reset(keepIsSyncing)

用来重置web3的状态。重置除了manager以外的其它所有东西。卸载filter，停止状态轮询

### 参数

1. `flag``Boolean` - 如果设置为true，将会卸载所有的filter，但会保留web3.eth.isSyncing()的状态轮询

### 返回值

`undefined`

### 示例

```js
web3.reset();
```

***

## web3.sha3

使用keccak-256哈希算法，计算给定字符串的哈希值

    web3.sha3(string [, options])

### 参数

1. `String` - 传入的需要使用Keccak-256 SHA3算法进行哈希运算的字符串.
1. `Object` - 可选项设置。如果要解析的是hex格式的十六进制字符串。需要设置encoding为hex。因为JS中会默认忽略0x.

### 返回值

`String` - 使用Keccak-256 SHA3算法哈希过的结果.

### 示例

```js
var hash = web3.sha3("Some string to be hashed");
console.log(hash); // "0xed973b234cf2238052c9ac87072c71bcf33abc1bbd721018e0cca448ef79b379"

var hashOfHash = web3.sha3(hash, {encoding: 'hex'});
console.log(hashOfHash); // "0x85dd39c91a64167ba20732b228251e67caed1462d4bcf036af88dc6856d0fdcc"
```

***

## web3.toHex

    web3.toHex(mixed);

Converts any value into HEX.

### 参数

1. `val``String|Number|Object|Array|BigNumber` - 需要转化为HEX的值。如果是一个对象或数组类型，将会先用JSON.stringify1进行转换成字符串。 如果传入的是BigNumber2，则将得到对应的Number的HEX.

### 返回值

`String` - The hex string of `mixed`.

### 示例

```js
var str = web3.toHex({test: 'test'});
console.log(str); // '0x7b2274657374223a2274657374227d'
```

***

## web3.toAscii

将HEX字符串转为ASCII3字符串

    web3.toAscii(hexString);


### 参数

1.`hexString` `String` - 十六进制字符串.

### 返回值

`String` - 给定十六进制字符串对应的ASCII码值.

### 示例

```js
var str = web3.toAscii("0x657468657265756d000000000000000000000000000000000000000000000000");
console.log(str); // "ethereum"
```

***

## web3.fromAscii

    web3.fromAscii(string [, padding]);

将任何的ASCII码字符串转为HEX字符串.

### 参数

1. `textString``String` -ASCII码字符串.
2. `padding``Number` - 返回的字符串字节大小，不够长会自动填充.

### 返回值

`String` - 转换后的HEX字符串.

### 示例

```js
var str = web3.fromAscii('ethereum');
console.log(str); // "0x657468657265756d"

var str2 = web3.fromAscii('ethereum', 32);
console.log(str2); // "0x657468657265756d000000000000000000000000000000000000000000000000"
```

***

## web3.toDecimal

    web3.toDecimal(hexString);

将一个十六进制转为一个十进制的数字.

### 参数

1. `hexString``String` - 十六进制字符.


### 返回值

`Number` - 传入字符串所代表的十六进制值.

### 示例

```js
var number = web3.toDecimal('0x15');
console.log(number); // 21
```

***

## web3.fromDecimal

    web3.fromDecimal(number);

将一个数字，或者字符串形式的数字转为一个十六进制串.

### 参数

1. `Number|String`  - 数字.

### 返回值

`String` - 给定数字对应的十六进制表示.

### 示例

```js
var value = web3.fromDecimal('21');
console.log(value); // "0x15"
```

***

## web3.fromWei

    web3.fromWei(number, unit)

以太坊货币单位之间的转换。将以wei为单位的资金，转换为指定单位的数值:

- `Gwei`
- `Kwei`
- `Mwei`/`babbage`/`ether`/`femtoether`
- `ether`
- `finney`/`gether`/`grand`/`gwei`
- `kether`/`kwei`/`lovelace`/`mether`/`micro`
- `microether`/`milli`/`milliether`
- `mwei`/`nano`/`nanoether`
- `noether`
- `picoether`/`shannon`
- `szabo`
- `tether`
- `wei`

### 参数

1. `number``Number|String|BigNumber` - 数字或BigNumber.
2. ``unit``String` - 单位字符串.


### 返回值

`String|BigNumber` - Either a number string, or a BigNumber instance, depending on the given `number` parameter.

### 示例

```js
var value = web3.fromWei('21000000000000', 'finney');
console.log(value); // "0.021"
```

***

## web3.toWei

    web3.toWei(number, unit)

将给定资金转换为以wei为单位的数值:

- `kwei`/`ada`
- `mwei`/`babbage`
- `gwei`/`shannon`
- `szabo`
- `finney`
- `ether`
- `kether`/`grand`/`einstein`
- `mether`
- `gether`
- `tether`

### 参数

1. `Number|String|BigNumber` - 数字或BigNumber.
2. `String` - 字符串单位.

### 返回值

`String|BigNumber` - 根据传入参数的不同，分别是字符串形式的字符串，或者是BigNumber.

### 示例

```js
var value = web3.toWei('1', 'ether');
console.log(value); // "1000000000000000000"
```

***

## web3.toBigNumber

    web3.toBigNumber(numberOrHexString);

Converts a given number into a BigNumber instance.

See the [note on BigNumber](#a-note-on-big-numbers-in-web3js).

### 参数

1. `Number|String` - 数字或十六进制格式的数字.

### 返回值

`BigNumber` - BigNumber的实例.

### 示例

```js
var value = web3.toBigNumber('200000000000000000000001');
console.log(value); // instanceOf BigNumber
console.log(value.toNumber()); // 2.0000000000000002e+23
console.log(value.toString(10)); // '200000000000000000000001'
```

***

## web3.isAddress

    web3.isAddress(HexString);

检查给定的字符串是否是有效的以太坊地址.

### 参数

1. `String` - 16进制字符串.

### 返回值

`Boolean` - 无效的地址返回false。对于全小写或全大写的有效地址字符串返回true。对于大小写混合的字符串, 该函数使用 `web3.isChecksumAddress()`进行检查.

### 示例

```js
var isAddress = web3.isAddress("0x8888f1f195afa192cfee860698584c030f4c9db1");
console.log(isAddress); // true
```
