目录
=================

  * [Graphene test net JS-SDK 文档](#Graphene-JS-SDK文档)
    * [<strong>1 JS-SDK 使用说明</strong>](#1-JS-SDK使用说明)
      * [<strong>1.1 请求过程说明</strong>](#11-请求过程说明)
    * [<strong>2 接口</strong>](#2-接口)
      * [<strong>2.1 账户 accounts</strong>](#21-账户accounts)
        * [<strong>2.1.1 根据地址获取账户信息</strong>](#211-根据地址获取账户信息)
      * [<strong>2.2 交易 transactions</strong>](#22-交易transactions)
        * [<strong>2.2.1 转账</strong>](#221-转账)
        * [<strong>2.2.2 设置二级密码</strong>](#222-设置二级密码)
        * [<strong>2.2.3 注册为代理</strong>](#223-注册为代理)
        * [<strong>2.2.4 投票</strong>](#224-投票)
        * [<strong>2.2.5 注册 Dapp</strong>](#225-注册Dapp)
        * [<strong>2.2.6 成为 Dapp 代理</strong>](#226-成为Dapp代理)
        * [<strong>2.2.7 存储信息</strong>](#227-存储信息)
        * [<strong>2.2.8 锁仓</strong>](#228-锁仓)
        * [<strong>2.2.9 跨链兑换</strong>](#229-跨链兑换)
        * [<strong>2.2.10 根据 id 获取交易信息</strong>](#2210-根据id获取交易信息)
        * [<strong>2.2.11 开关代理</strong>](#2211-开关代理)
      * [<strong>2.3 区块 blocks</strong>](#23-区块blocks)
        * [<strong>2.3.1 根据高度获取区块详细信息</strong>](#231-根据高度获取区块详细信息)
    * [<strong>3 示例</strong>](#3-示例)


# Graphene-JS-SDK文档

## **1 JS-SDK 使用说明**
官方提供 js-sdk，可运行于浏览器，Electron 客户端。支持 Graphene 系统内置交易类型。本 sdk 多数函数调用需要个人密钥作为参数，但 sdk 只在本地使用密钥，发送给服务端的数据只包含公钥或者签名等信息，不包含用户敏感信息。

### **1.1 请求过程说明**
1.1 发起一笔交易，调用 sdk 导出的相应类型函数。<br/>
1.2 sdk 使用用户传入的参数，根据 Graphene 的接口规则，产生一个交易，并进行签名，最终得到完整的交易数据。<br/>
1.3 发送请求，把构造完成的数据通过 POST/GET 等方式传发送给 Graphene 节点。<br/>
1.4 服务端节点接收到请求后，立即进行校验，验证通过后便会处理该次发送过来的请求。<br/>
1.5 以 JSON 格式返回响应结果数据。每个响应都包含 code 字段，0 表示成功，其他值表示失败，并包含错误原因。

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

### **2.2 交易transactions**

#### **2.2.1 转账**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：send<br/>
参数说明：

|名称	|类型   |必填 |说明      |
|------ |-----  |---  |----              |
|secret |string |Y    |发送者密码      |
|recipient |string| Y | 接收者地址 |
|amount | number | Y | 数量 |
|secondSecret| string| N | 发送者二级密码

sdk 请求示例：
```js
const send = exports.send(secret, recipient, amount, secondSecret)
```

#### **2.2.2 设置二级密码**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：signature<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    |发送者密码      |
|secondSecret |string| Y | 二级密码 |
|secondSecretOld| string| N | 当前二级密码

备注：如果当前无二级密码，第三个参数省略。


sdk 请求示例：
```js
const signature = exports.signature(secret, secondSecret, secondSecretOld)
```

#### **2.2.3 注册代理**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：delegate<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    |发送者密码      |
|username |string| Y | 名称 |
|secondSecret| string| N | 二级密码

sdk 请求示例：
```js
const delegate = exports.delegate(secret, username，secondSecret)
```

#### **2.2.4 投票**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：vote<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string | Y |发送者密码      |
|votes |array| Y | 投票列表 |

备注：投票列表为公钥字符串数组，+/- 代表投票和取消投票

sdk 请求示例：
```js
const vote = exports.vote(secret, [
  '+bc7e64263844ab3d4f91edbe76d6c2a066efa29159b941cbcd411e0cbb825cf9'
])
```

#### **2.2.5 注册Dapp**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：dapp<br/>
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

sdk 请求示例：
```js
  const dapp = exports.dapp(secret, {
    id
  }, secondSecret)
```

#### **2.2.6 成为Dapp代理**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：joinDapp<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    | 发送者密码      |
| id | long | Y | 侧链 id |
| secondSecret |string |N | 二级密码

备注：侧链注册者自动成为 Dapp 代理。成为 Dapp 代理可处理跨链兑币交易。

sdk 请求示例：
```js
  const joinDapp = exports.joinDapp(secret, id, secondSecret)
```

#### **2.2.7 存储信息**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：storage<br/>
参数说明：

|名称	|类型   |必填 |说明              |
|------ |-----  |---  |----              |
|secret |string |Y    | 发送者密码      |
| content | string | Y | 内容 |

备注：存储的是 content 的 hash

sdk 请求示例：
```js
  const storage = exports.storage(secret, content)
```

#### **2.2.8 锁仓**

接口地址：/api/transaction/transaction<br/>
请求方式：POST<br/>
支持格式：'application/json'<br/>
sdk 导出函数：lock<br/>
参数说明：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----              |
|secret |string |Y    | 发送者密码      |
| height | integer | Y | 高度 |
| secondSecret | string | N | 二级密码

sdk 请求示例：
```js
  const lock = exports.lock(secret, height， secondSecret)
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
sdk 导出函数：turnDelegate<br/>
参数说明：

|名称	|类型   |必填 |说明            |
|------ |-----  |---  |----              |
|secret |string | Y  | 发送者密码      |
| open | boolean | Y | 打开 true，关闭 false |
| secondSecret | string | N | 二级密码

备注：只有注册为代理之后的账户此接口才有效

sdk 请求示例：
```js
  const turnDelegate = exports.turnDelegate(secret, false, secondSecret)
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

### 3 示例

```html
<!DOCTYPE html>
<html><head>
<meta charset="utf-8"/>
<title>TEST</title>
<script type="text/javascript" charset="utf-8" src="http://apps.bdimg.com/libs/jquery/1.9.1/jquery.min.js"></script>
<script type="text/javascript" charset="utf-8" src="http://7xqoxy.com1.z0.glb.clouddn.com/cryp.js"></script>
<script type="text/javascript" charset="utf-8" src="index.bundle.js"></script>
</head>
<body>
	<script type="text/javascript" charset="utf-8">
	  $(function(){
	  const secret = 'your secret'
	  const recipient = '24dYNPt2Ed8ukwuaQi66R4npkeSWdygcUFo8hEHKseZ7DZtzV8'
	  const send = exports.send(secret, recipient, 2.9)
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
