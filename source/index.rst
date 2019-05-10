Web3.js 0.20 使用说明
================================

本文档基于当前最新 `官方文档 <https://github.com/ethereum/wiki/wiki/JavaScript-API>`_ 由 `深入浅出区块链  <https://learnblockchain.cn/>`_ 社区成员翻译、整理、校队，我们虽力求准确，但如您发现纰漏，欢迎到 `GitHub 提Issues  <https://github.com/lbc-team/web3.js-0.2x-docs>`_ 指正。

尊重汗水，需转载请联系微信：xlbxiong 获取授权。


web3.js简介
==================

Web3是一套和以太坊节点进行通信的API，如果我们需要基于以太坊来开发去中心化应用，就可能需要使用web3（或者使用 `ethers.js <https://learnblockchain.cn/docs/ethers.js/>`_  ），例如需要通过Web3来获取节点状态，获取账号信息，调用合约、监听合约事件等等。

  译者注: 智能合约是运行在节点提供的虚拟机上，因此调用智能合约也需要像节点发送请求。

web3.js 是 Web3 协议的 JavaScript 实现版本，其他的多个语言版本的实现有

* `Python Web3.py <https://github.com/ethereum/web3.py>`_
* `Haskell hs-web3 <https://github.com/airalab/hs-web3>`_
* `Java web3j <https://github.com/web3j/web3j>`_
* `Scala web3j-scala <https://github.com/mslinn/web3j-scala>`_
* `Purescript purescript-web3 <https://github.com/f-o-a-m/purescript-web3>`_
* `PHP web3.php <https://github.com/sc0Vu/web3.php>`_
* `PHP ethereum-php <https://github.com/digitaldonkey/ethereum-php>`_


注意 web3.js 有两个不兼容的版本：0.20.x及1.0beta，1.0对0.20版本做了重构。

**本文档是web3.js 0.20.x版本翻译**，其对应的官方文档地址在 `JavaScript-API <https://github.com/ethereum/wiki/wiki/JavaScript-API>`_ 。
如果你使用的是 web3.js 1.0版本，其对应的官方文档地址在 `Web3js Readthedocs <http://web3js.readthedocs.io/en/1.0/index.html>`_ 。


基于以太坊开发DApp（去中心化应用程序），可以使用 `web3.js库  <https://github.com/ethereum/web3.js>`_ 提供的 **web3** 对象， 在底层实现上， **web3** 通 过 `RPC调用  <https://github.com/ethereum/wiki/wiki/JSON-RPC>`_ 与本地节点通信， **web3.js** 可以与任何暴露了RPC接口的以太坊节点连接。

`web3` 包含下面几个对象：

 - `web3.eth` 用来与以太坊区块链及合约的交互
 - `web3.shh` 用来与Whisper协议相关交互
 - `web3.net` 用来获取网络相关信息
 - `web3`     主对象 包含一些工具

**web3.js使用示例**：

- `使用web3.js API在页面中转账  <https://web3.learnblockchain.cn/transDemo.html>`_
- `使用web3.js 与合约交互及监听合约事件  <https://web3.learnblockchain.cn/ContractEventDemo.html>`_
- `多个API 使用Demo  <https://web3.learnblockchain.cn/web3_v0.20_demo.html>`_
- `官方示例  <https://github.com/ethereum/web3.js/tree/master/example>`_
- `Dapp 模式  <https://github.com/ethereum/wiki/wiki/Useful-Ðapp-Patterns>`_

想要学习去中心化应用(DAPP)开发，这门课程不容错过 `区块链全栈-以太坊DAPP开发实战  <https://wiki.learnblockchain.cn/course/dapp.html>`_


.. _import-web3:

引入web3
==================

首先你需要将web3引入到应用工程中，可以通过如下几个方法：

- npm:   ``npm install web3``
- bower:   ``bower install web3``
- meteor:   ``meteor add ethereum:web3``
- vanilla:   链接 ``dist/web3.min.js``

然后你需要创建一个 **web3** 的实例，设置一个provider。为了保证你不会覆盖一个已有的provider（Mist浏览器或安装了MetaMak的浏览器会提供Provider），需要先检查是否 **web3** 实例已存在，示例代码如下：

::

    if (typeof web3 !== 'undefined') {
      web3 = new Web3(web3.currentProvider);
    } else {
      // set the provider you want from Web3.providers
      web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
    }


译者注: 官方文档中对于web3的没有更新，现在新的检查方式为：

::

  // 检查是否是新的MetaMask 或 DApp浏览器
  var web3Provider;
  if (window.ethereum) {
     web3Provider = window.ethereum;
    try {
      // 请求用户授权
      await window.ethereum.enable();
    } catch (error) {
      // 用户不授权时
      console.error("User denied account access")
    }
  } else if (window.web3) {   // 老版 MetaMask Legacy dapp browsers...
    web3Provider = window.web3.currentProvider;
  } else {
    web3Provider = new Web3.providers.HttpProvider('http://localhost:8545');
  }
  web3 = new Web3(web3Provider);



成功引入后，你现在可以使用 `web3` 对象的API 了。

使用回调
==================

由于这套API被设计来与本地的RPC结点交互，所有函数默认使用同步的HTTP的请求。

如果你想发起一个异步的请求。大多数函数允许传一个跟在参数列表后的可选的回调函数来支持异步，回调函数支持 `Error-first回调 <http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/>`_ 的风格。

::

  web3.eth.getBlock(48, function(error, result){
      if(!error)
          console.log(JSON.stringify(result));
      else
          console.error(error);
  })


批量请求
==================

可以允许将多个请求放入队列，并一次执行。

**注意**：批量请求并不会更快，在某些情况下，同时发起多个异步请求，也许更快。这里的批量请求主要目的是用来保证请求的串行执行。


::

  var batch = web3.createBatch();
  batch.add(web3.eth.getBalance.request('0x0000000000000000000000000000000000000000', 'latest', callback));
  batch.add(web3.eth.contract(abi).at(address).balance.request(address, callback2));
  batch.execute();


web3.js中的大数处理
===================

如果是一个数据类型的返回结果，通常会得到一个BigNumber对象，因为Javascript不能正确的处理BigNumber，看看下面的例子：

::

  "101010100324325345346456456456456456456"
  // "101010100324325345346456456456456456456"
  101010100324325345346456456456456456456
  // 1.0101010032432535e+38


所以web3.js依赖 `BigNumber Library <https://github.com/MikeMcl/bignumber.js`>_ 。

::

  var balance = new BigNumber('131242344353464564564574574567456');
  // or var balance = web3.eth.getBalance(someAddress);

  balance.plus(21).toString(10); // 转化为字符串
  // "131242344353464564564574574567477"


下一个例子中，我们会看到，如果有20位以上的浮点值，仍会导致出错。所以推荐尽量让帐户余额以wei为单位，仅仅在需要向用户展示时，才转换为其它单位。

::

  var balance = new BigNumber('13124.234435346456466666457455567456');

  balance.plus(21).toString(10); // toString(10) converts it to a number string, but can only show upto 20 digits
  // "13145.23443534645646666646" // your number will be truncated after the 20th digit




.. toctree::
   :maxdepth: 2
   :caption: Web3.js 0.2x.x API 手册

   web3.md
   web3.net.md
   web3.eth.md
   web3.db.md
   web3.shh.md


Indices and tables
==================

* :ref:`genindex`
* :ref:`search`
