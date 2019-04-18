# 网络对象 net

网络相关API，查看节点网络状态。

## web3.net.listening

```js
    web3.net.listening
    // 或异步方式
    web3.net.getListening(callback(error, result){ ... })
```

此属性是只读的，表示当前连接的节点，是否正在监听网络连接.

### 返回值

`Boolean` - `true` 表示连接上的节点正在监听网络请求.

### 示例

```js
var listening = web3.net.listening;
console.log(listening); // true of false
```

***

## web3.net.peerCount

```js
    web3.net.peerCount
    // or async
    web3.net.getPeerCount(callback(error, result){ ... })
```

属性是只读的，返回连接节点已连上的其它以太坊节点的数量.

### 返回值

`Number` - 连接节点连上的其它以太坊节点的数量.

### 示例

```js
var peerCount = web3.net.peerCount;
console.log(peerCount); // 4
```
