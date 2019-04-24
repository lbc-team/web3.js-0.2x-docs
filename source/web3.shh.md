
## 通信协议对象 shh

Whisper是一个P2P的通信协议相关的API，这里有一个[术语解释](https://learnblockchain.cn/books/geth/term.html)，了解更多可参考[Whisper  Overview](https://github.com/ethereum/wiki/wiki/Whisper-Overview)


### 示例

```js
var shh = web3.shh;
```

***

### web3.shh.post

   web3.shh.post(object [, callback])

调用post()方法向以太坊网络发送whisper消息.

#### 参数

1. `Object` - 要发送的对象:
  - `from`: `String`, 60 Bytes HEX - (可选) 发送者
  - `to`: `String`, 60 Bytes  HEX - (可选) 接收者. 仅有接收者能解密消息。
  - `topics`: `Array of Strings` - 主题数组, 为了接收者能区分消息.
  - `payload`: `String|Number|Object` - 消息负载.
  - `priority`: `Number` - 优先级
  - `ttl`: `Number` - 存活时间（秒）
2.`callback` `Function` - 可选的回调函数，设置此参数后函数将采用异步http请求节点API.

#### 返回值

`Boolean` - 消息成功发送后返回true，否则返回false.


#### 示例

```js
var identity = web3.shh.newIdentity();
var topic = '示例';
var payload = 'hello whisper world!';

var message = {
  from: identity,
  topics: [topic],
  payload: payload,
  ttl: 100,
  workToProve: 100 // or priority TODO
};

web3.shh.post(message);
```

***

### web3.shh.newIdentity

    web3.shh.newIdentity([callback])

调用newIdentity()方法创建一个新的id.

#### 参数

1. `callback``Function` - 可选的回调函数，设置此参数时函数将采用异步http来请求节点旳API.


#### 返回值

`String` - 新id，以16进制字符串表示.


#### 示例

```js
var identity = web3.shh.newIdentity();
console.log(identity); // "0xc931d93e97ab07fe42d923478ba2465f283f440fd6cabea4dd7a2c807108f651b7135d1d6ca9007d5b68aa497e4619ac10aa3b27726e1863c1fd9b570d99bbaf"
```

***

### web3.shh.hasIdentity

    web3.shh.hasIdentity(identity, [callback])

调用hasIdentity()方法检查用户是否有指定的id.

#### 参数

1. `identity``String` - 要检查的id.
2. `callback``Function` - 可选的回调函数，设置此参数后函数将采用异步http来请求节点API.

#### 返回值

`Boolean` - 如果指定的id存在则返回true，否则返回false.


#### 示例

```js
var identity = web3.shh.newIdentity();
var result = web3.shh.hasIdentity(identity);
console.log(result); // true

var result2 = web3.shh.hasIdentity(identity + "0");
console.log(result2); // false
```

***

### web3.shh.newGroup

#### 示例
```js
// TODO: not implemented yet
```

***

### web3.shh.addToGroup

#### 示例
```js
// TODO: not implemented yet
```

***

### web3.shh.filter

```js
var filter = web3.shh.filter(options)

// watch for changes
filter.watch(function(error, result){
  if (!error)
    console.log(result);
});
```

调用web3.ssh.filter()方法来监听接收的whisper消息.

#### 参数

1. `options``Object` - 过滤器选项:
  * `topics`: `Array of Strings` - 主题字符串数组，用来过滤消息，可以如下组合:
    - `['topic1', 'topic2'] == 'topic1' && 'topic2'`
    - `['topic1', ['topic2', 'topic3']] == 'topic1' && ('topic2' || 'topic3')`
    - `[null, 'topic1', 'topic2'] == ANYTHING && 'topic1' && 'topic2'` -> `null` works as a wildcard
  * `to`: Filter by identity of receiver of the message. If provided and the node has this identity, it will decrypt incoming encrypted messages.
2. `callback``Function` - 可选的回调函数，当设置此参数后将采用异步http请求节点旳API.

#### 回调函数的返回值

`Object` - 接收到的消息:

  - `from`: `String`, 60字节，消息发送者.
  - `to`: `String`, 60字节，消息接受者.
  - `expiry`: `Number` - 消息过期时间，秒为单位.
  - `ttl`: `Number` -  消息存活时间，秒为单位.
  - `sent`: `Number` -  消息发送时间，unix时间戳格式.
  - `topics`: `Array of String` - 消息包含的主题字符串数组.
  - `payload`: `String` - 消息的载荷内容.
  - `workProved`: `Number` - 消息发送前需要完成的任务.


