目录
=================

  * [Eros Test Net JS-SDK 文档](#Eros-JS-SDK文档)
    * [<strong>1 JS-SDK 使用说明</strong>](#1-JS-SDK使用说明)
      * [<strong>1.1 请求过程说明</strong>](#11-请求过程说明)
    * [<strong>2 接口</strong>](#2-接口)
      * [<strong>2.1 账户accounts</strong>](#21-账户accounts)
        * [<strong>2.1.1 根据地址获取账户信息</strong>](#211-根据地址获取账户信息)
        * [<strong>2.1.2 由主账户密码得到密钥对</strong>](#212-由主账户密码得到密钥对)
        * [<strong>2.1.3 由地址计算公钥</strong>](#213-由地址计算公钥)
        * [<strong>2.1.4 由公钥计算地址</strong>](#214-由公钥计算地址)
        * [<strong>2.1.5 获取相关地址的所有交易</strong>](#215-获取相关地址的所有交易)
      * [<strong>2.2 交易 transactions</strong>](#22-交易transactions)
        * [<strong>2.2.1 转账</strong>](#221-转账)
        * [<strong>2.2.2 设置二级密码</strong>](#222-设置二级密码)
        * [<strong>2.2.3 注册为代理</strong>](#223-注册为代理)
        * [<strong>2.2.4 投票</strong>](#224-投票)
        * [<strong>2.2.5 注册Dapp</strong>](#225-注册Dapp)
        * [<strong>2.2.6 成为Dapp代理</strong>](#226-成为Dapp代理)
        * [<strong>2.2.7 存储信息</strong>](#227-存储信息)
        * [<strong>2.2.8 锁仓</strong>](#228-锁仓)
        * [<strong>2.2.9 跨链兑换</strong>](#229-跨链兑换)
        * [<strong>2.2.10 根据id获取交易信息</strong>](#2210-根据id获取交易信息)
        * [<strong>2.2.11 开关代理</strong>](#2211-开关代理)
        * [<strong>2.2.12 获取全部交易</strong>](#2212-获取全部交易)
        * [<strong>2.2.13 提交合约</strong>](#2213-提交合约)
        * [<strong>2.2.14 调用合约</strong>](#2214-调用合约)
        * [<strong>2.2.15 注册资产</strong>](#2215-注册资产)
        * [<strong>2.2.16 转移资产</strong>](#2216-转移资产)
      * [<strong>2.3 区块blocks</strong>](#23-区块blocks)
        * [<strong>2.3.1 根据高度获取区块详细信息</strong>](#231-根据高度获取区块详细信息)
        * [<strong>2.3.2 获取最高区块</strong>](#232-获取最高区块)
        * [<strong>2.3.3 获取全部区块</strong>](#233-获取全部区块)
      * [<strong>2.4 代理delegate</strong>](#24-代理delegate)
        * [<strong>2.4.1 获取代理列表</strong>](#241-获取代理列表)
      * [<strong>2.5 投票vote</strong>](#25-投票vote)
        * [<strong>2.5.1 查询历史投票</strong>](#251-查询历史投票)
      * [<strong>2.6 其他导出函数</strong>](#26-其他导出函数)
        * [<strong>2.6.1 导出Buffer对象</strong>](#261-导出Buffer对象)
        * [<strong>2.6.2 导出jshash对象</strong>](#262-导出jshash对象)
        * [<strong>2.6.3 将交易或者区块的timestamp转换为世界时</strong>](#263-将交易或者区块的timestamp转换为世界时)
        * [<strong>2.6.4 交易类型的文字描述</strong>](#264-交易类型的文字描述)
        * [<strong>2.6.5 判断地址是否合法</strong>](#265-判断地址是否合法)
        * [<strong>2.6.6 判断字符串是否为交易id</strong>](#266-判断字符串是否为交易id)
        * [<strong>2.6.7 判断是否为有效高度</strong>](#267-判断是否为有效高度)
      * [<strong>2.7 智能合约</strong>](#27-智能合约)
    * [<strong>3 示例</strong>](#3-示例)

# Eros-JS-SDK文档

## **1 JS-SDK 使用说明**
官方提供JS-SDK，可运行于浏览器、Electron客户端和手机端。支持 Eros 系统内置交易类型。本开发包多数函数调用需要个人密钥作为参数，但只在本地使用密钥，发送给服务端的数据只包含公钥或者签名等信息，不包含用户敏感信息。

### **1.1 请求过程说明**
1.1 创建一笔新的交易，调用SDK导出的相应函数。<br/>
1.2 SDK使用用户传入的参数，根据 Eros 的接口规则，产生一个交易，并进行签名，最终得到完整的交易数据。<br/>
1.3 发送请求，把构造完成的数据通过 POST/GET 等方式传发送给 Eros 节点。<br/>
1.4 服务端节点接收到请求后，立即进行校验，验证通过后便会处理该次发送过来的请求。<br/>
1.5 以 JSON 格式返回响应结果数据。每个响应都包含 code 字段，0 表示成功，其他值表示失败，并包含错误原因。

### **交易费用**

TODO

---

## **2 接口**

### **2.1 账户accounts**

#### **2.1.1 根据地址获取账户信息**
接口地址：/api/account/address/:address<br/>
请求方式：GET<br/>
备注：返回此地址账户详细信息

请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/account/address/24dYNPt2Ed8ukwuaQi66R4npkeSWdygcUFo8hEHKseZ7DZtzV8'
```

#### **2.1.2 由主账户密码得到密钥对**

```
const keyPair = erosLib.fromSecret('your secret')
console.log(keyPair)
```

#### **2.1.3 由地址计算公钥**

erosLib.getPublicKeyByAddress

#### **2.1.4 由公钥计算地址**

erosLib.getAddressByPublicKey

#### **2.1.5 获取相关地址的所有交易**

接口地址：/api/account/trss/:address<br/>
请求方式：GET<br/>
参数说明：

|名称	|类型   |必填 |说明      |
|------ |-----  |---  |----   |
| page | string | Y    | 分页的页数，从零开始  |


请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/account/trss/24dYNPt2Ed8ukwuaQi66R4npkeSWdygcUFo8hEHKseZ7DZtzV8?page=0'
```


### **2.2 交易transactions**

#### **2.2.1 转账**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：send<br/>
参数说明：

|名称	|类型   |必填 |说明      |
|------ |-----  |---  |----   |
|secret |string |Y    |发送者密码      |
|recipient |string| Y | 接收者地址 |
|amount | number | Y | 数量 |
|message | string | N  | 备注信息
|secondSecret| string| N | 发送者二级密码

SDK 请求示例：
```js
const send = erosLib.send(secret, recipient, amount, message，secondSecret)
```

#### **2.2.2 设置二级密码**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：signature<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    |发送者密码      |
|secondSecret |string| Y | 二级密码 |
|secondSecretOld| string| N | 当前二级密码

备注：如果当前无二级密码，第三个参数省略。


SDK 请求示例：
```js
const signature = erosLib.signature(secret, secondSecret, secondSecretOld)
```

#### **2.2.3 注册代理**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：delegate<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    |发送者密码      |
|username |string| Y | 名称 |
|secondSecret| string| N | 二级密码

SDK 请求示例：
```js
const delegate = erosLib.delegate(secret, username，secondSecret)
```

#### **2.2.4 投票**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：vote<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string | Y |发送者密码      |
|votes |array| Y | 投票列表 |

备注：投票列表为公钥字符串数组，+/- 代表投票/取消投票

SDK 请求示例：
```js
const vote = erosLib.vote(secret, [
  '+bc7e64263844ab3d4f91edbe76d6c2a066efa29159b941cbcd411e0cbb825cf9'
])
```

#### **2.2.5 注册Dapp**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：dapp<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    | 发送者密码      |
| opt | object | Y | option 对象 |
| opt.id | long | Y | 侧链 id |
| opt.name | string | N |名称|
| opt.description | string | N |描述 |
|opt.link | string| N | 主页链接 |
| secondSecret | string |N | 二级密码

SDK 请求示例：
```js
  const dapp = erosLib.dapp(secret, {
    id
  }, secondSecret)
```

#### **2.2.6 成为Dapp代理**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：joinDapp<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    | 发送者密码      |
| id | long | Y | 侧链 id |
| secondSecret |string |N | 二级密码

备注：侧链注册者自动成为 Dapp 代理。成为 Dapp 代理可处理跨链兑币交易。

SDK 请求示例：
```js
  const joinDapp = erosLib.joinDapp(secret, id, secondSecret)
```

#### **2.2.7 存储信息**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：storage<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    | 发送者密码      |
| content | string | Y | 内容 |

备注：存储的是 content 的 hash

SDK 请求示例：
```js
  const storage = erosLib.storage(secret, content)
```

#### **2.2.8 锁仓**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数：lock<br/>
参数说明：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----              |
|secret |string |Y    | 发送者密码      |
| height | integer | Y | 高度 |
| secondSecret | string | N | 二级密码

SDK 请求示例：
```js
  const lock = erosLib.lock(secret, height， secondSecret)
```

####  **2.2.9 跨链兑换**

```
待开放
```

#### **2.2.10 根据id获取交易信息**

接口地址：/api/transaction/:id<br/>
请求方式：GET<br/>
备注：返回此交易原始信息

请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/transaction/1722550667642e772fb834c6e8d3b9eb3aa04c7f27fd05df74a62758290f8e4a'
```

#### **2.2.11 开关代理**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数: turnDelegate<br/>
参数说明：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----              |
|secret |string | Y  | 发送者密码      |
| open | boolean | Y | 打开 true，关闭 false |
| secondSecret | string | N | 二级密码

备注：只有注册为代理之后的账户才可关闭

SDK 请求示例：
```js
  const turnDelegate = erosLib.turnDelegate(secret, false, secondSecret)
```

#### **2.2.12 获取全部交易**

接口地址：/api/transaction/trss/:page<br/>
请求方式：GET<br/>
请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/transaction/trss/0'
```

#### **2.2.13 提交合约**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数: contract<br/>
参数说明：

| 名称	| 类型 | 必填 | 说明 |
|------|------|------|------|
| secret | string | Y  | 发送者密码 |
| option | object | Y | option 对象 |
| option.code | string | Y | 合约代码 |
| option.message | string | N | 备注，最多255个字符 |
| secondSecret | string | N | 二级密码

SDK 请求示例：
```js
  const code = `function main(args) {
  var strget = storageGet()
  var getObj = []
  if (strget) {
    getObj = JSON.parse(strget)
  }
  if (getObj.indexOf(requesterKey) >= 0) {
    return JSON.stringify({
      amount: 0
    })
  } else {
    getObj.push(requesterKey)
    storageSet(JSON.stringify(getObj))
    return JSON.stringify({
      amount: 5
    })
  }
}`
  const contract = erosLib.contract(secret, { code }, secondSecret)
```


#### **2.2.14 调用合约**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数: invoke<br/>
参数说明：

| 名称	| 类型 | 必填 | 说明 |
|------|------|------|------|
| secret | string | Y  | 发送者密码 |
| option | object | Y | option 对象 |
| option.contractId | string | Y | 合约id |
| option.args | array | N | 字符串数组，不大于8项 |
| option.message | string | N | 备注，最多255个字符

SDK 请求示例：
```js
  const invoke = erosLib.invoke(secret, { contractId })
```

#### **2.2.15 注册资产**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数: regAsset<br/>
参数说明：

| 名称	| 类型 | 必填 | 说明 |
|------|------|------|------|
| secret | string | Y  | 发送者密码 |
| option | object | Y | option 对象 |
| option.amount | long | Y | 资产总额 |
| option.message | string | N | 备注，最多255个字符
| secondSecret | string | N | 二级密码

SDK 请求示例：
```js
  const regAsset = erosLib.regAsset(secret, { amount }, secondSecret)
```

#### **2.2.16 转移资产**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
SDK 导出函数: assetTransfer<br/>
参数说明：

| 名称	| 类型 | 必填 | 说明 |
|------|------|------|------|
| secret | string | Y  | 发送者密码 |
| receiver | string | Y | 接收者地址 |
| option | object | Y | option 对象 |
| option.assetId | string | Y | 资产id |
| option.amount | long | Y | 数量 |
| option.message | string | N | 备注，最多255个字符 |
| secondSecret | string | N | 二级密码

SDK 请求示例：
```js
  const assetTransfer = erosLib.assetTransfer(secret, { assetId, amount }, secondSecret)
```

### **2.3 区块blocks**

#### **2.3.1 根据高度获取区块详细信息**

接口地址：/api/block/height/:height<br/>
请求方式：GET<br/>
参数：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----              |
| org |string | N    | 只能为 1      |

备注：返回此区块原始信息

请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/block/height/230?org=1'
```

#### **2.3.2 获取最高区块**

接口地址：/api/block/top<br/>
请求方式：GET<br/>
参数：

备注：返回已入库之最高区块

请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/block/top'
```

#### **2.3.3 获取全部区块**

接口地址：/api/block/blocks/:page<br/>
请求方式：GET<br/>
参数：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----         |
| page |string |  Y    | 页数，从零开始     |

请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/block/blocks/0'
```

### **2.4 代理delegate**

#### **2.4.1 获取代理列表**

接口地址：/api/delegate/list/:page<br/>
请求方式：GET<br/>
参数：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----         |
|page | integer | Y | 第几页，从零开始 |
| address | string | N | 自己的地址 |

备注：address 用?address=xxx 来传，可以没有，如果有则额外返回是否给相应的代理投了票

请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/delegate/list/1'
```

### **2.5 投票vote**

#### **2.5.1 查询历史投票**

接口地址：/api/vote/voted/:address<br/>
请求方式：GET<br/>
参数：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----         |
| page | string | Y | 页信息，从零开始    |

请求示例：
```bash
curl -k -X GET 'http://123.56.187.196:10086/api/vote/voted/2S1pW43jTeZAvhXZRWcVjbpZhqS1ZMEczaRq7u4mGJfPwEoxW3?page=0'
```

### **2.6 其他导出函数**

#### **2.6.1 导出Buffer对象**

erosLib.Buffer

#### **2.6.2 导出jshash对象**

示例：
```
erosLib.jshash.sha256().update('abc').digest('hex')
```

#### **2.6.3 将交易或者区块的timestamp转换为世界时**

示例：
```
new Date(erosLib.realTime(trs.timestamp)).toLocaleString()
```

#### **2.6.4 交易类型的文字描述**

示例：
```
console.log(erosLib.typeLiteral(trs.type))
```

#### **2.6.5 判断地址是否合法**

示例:
```
if (erosLib.isAddress(address)) {
  // ...
}
```

#### **2.6.6 判断字符串是否为交易id**

示例:
```
if (erosLib.isTrsId('02be8404f181404847b63f7f19fcb52734f95f7ad22095848d76adb2faf2f67c')) {
  // ...
}
```

#### **2.6.7 判断是否为有效高度**

示例:
```
if (erosLib.isValidHeight('20')) {
  // ...
}
```

### **2.7 智能合约**

[技术白皮书-合约](https://github.com/ErosPlatform/docs/blob/master/eros_whitepaper_zh.md "https://github.com/ErosPlatform/docs/blob/master/eros_whitepaper_zh.md")

### 3 示例

```html
<!DOCTYPE html>
<html><head>
<meta charset="utf-8"/>
<title>TEST</title>
<script type="text/javascript" charset="utf-8" src="http://apps.bdimg.com/libs/jquery/1.11.3/jquery.min.js"></script>
<script type="text/javascript" charset="utf-8" src="http://7xqoxy.com1.z0.glb.clouddn.com/cryp.js"></script>
<script type="text/javascript" charset="utf-8" src="index.bundle.js"></script>
</head>
<body>
	<script type="text/javascript" charset="utf-8">
	  $(function(){
	  const secret = 'your secret'
	  const recipient = '24dYNPt2Ed8ukwuaQi66R4npkeSWdygcUFo8hEHKseZ7DZtzV8'
	  const send = erosLib.send(secret, recipient, 2.9)
	  $.ajax({
	    url: 'http://123.56.187.196:10086/api/transaction/transaction',
	    type: 'POST',
	    contentType: 'application/json',
	    data: JSON.stringify({ transaction: send }),
	    success: function(res){
	      console.log(res)
	    },
	    error: function(err){
	    }
	  })

	})
	</script>
</body>
</html>
```
