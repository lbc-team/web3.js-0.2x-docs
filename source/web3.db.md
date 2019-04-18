# 数据对象 db

**web3.db** 存储数据相关的接口

## web3.db.putString

```js
    web3.db.putString(db, key, value)
```

使用putString()方法在本地leveldb数据库中存储字符串.

### 参数

1. `db`: `String` - 存储数据库名称.
2. `key`: `String` - 存储键名.
3. `value`: `String` - 要存入的字符串.

### 返回值

`Boolean` - 成功返回`true`，否则返回`false`

### 示例

```js
web3.db.putString('testDB', 'key', 'myString') // true
```

***

## web3.db.getString

```js
    web3.db.getString(db, key)
```

从本地leveldb数据库获取字符串。

### 参数

1. `db`: `String` - 目标数据库名称.
2. `key`: `String` - 要读取的键.

### 返回值

`String` - 返回对应键所保存的字符串.

### 示例

```js
var value = web3.db.getString('testDB', 'key');
console.log(value); // "myString"
```

***

## web3.db.putHex

```js
    web3.db.putHex(db, key, value)
```

使用putHex()方法在本地leveldb数据库中以二进制存储方式存入 16进制字符串.

### 参数

1. `db`: `String` - 目标数据库名称.
2. `key`: `String` - 目标键名称.
3. `value`: `String` - 要保存的16进制字符串.

### 返回值

`Boolean` - 成功时返回`true`，失败时返回`false`.

### 示例
```js
web3.db.putHex('testDB', 'key', '0x4f554b443'); // true

```

***

## web3.db.getHex

```js
    web3.db.getHex(db, key)
```

使用getHex()方法从本地leveldb数据库中读取指定键的16进制格式字符串.

### 参数

1. `db`: `String` - 目标数据库.
2. `key`: `String` - 要读取的键名称.

### 返回值

`String` - 16进制字符串值.


### 示例

```js
var value = web3.db.getHex('testDB', 'key');
console.log(value); // "0x4f554b443"
```

***
