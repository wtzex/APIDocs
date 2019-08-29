## 说明

## API请求域名

```
https://api.wtzex.io
```
## 返回code
所有接口，均返回code字段，code字段值为字符串“000000”代表成功，其他值均为失败。

# 公有接口

## ping

**简要描述：** 

- 测试服务器是否连通

**请求方式及请求URL：** 
- GET `/v1/ping`

**参数：** 
None

**返回示例**
```
{
	"code": "000000"
}
```

**返回值说明** 
无

## timestamp

**简要描述：** 

- 获取服务器时间
- GET `/v1/timestamp`
获取服务器时间


**参数：** 
None

**返回示例**
```
{
  "code": "000000",
  "data": {
    "timezone": "UTC",
    "timestamp": 1499827319559
  },
  "msg": "success"
}
```

 **返回值说明** 

| 字段名    | 类型   | 说明   |
| :-------- | :----- | ------ |
| timezone  | string | 时区   |
| timestamp | 整数   | 时间戳 |

 **备注** 

- 暂无





## 市场(交易对)信息

**简要描述：** 

- 获取交易对详细信息

**请求方式及请求URL：** 
- GET `/v1/public/spot/market`

**参数：** 

None

 **返回示例**

``` 
{
    "code": "000000",
    "data": {
	   "BTC/USDT": {
	        "limits":{
				"amount":{
					"min":0.001,
					"max":9000
				},
				"price":{
					"min":0
				}
			},
        "precision":{
            "amount":4,
            "price":2
        },
        "percentage":true,
        "taker":0.001,
        "maker":0.001,
        "id":"BTCUSDT",
        "symbol":"BTC/USDT",
        "base":"BTC",
        "quote":"USDT",
        "baseId":"BTC",
        "quoteId":"USDT",
        "active":true
	   },
	   "ETH/USDT": {
	   	   ....
	   }
	},
    "msg": "OK"
}
```

 **返回值说明** 

| 参数名 | 类型                        | 说明             |
| :----- | :-------------------------- | ---------------- |
| data   | Map<SymbolName, SymbolInfo> | 交易对的详细信息 |

SymbolName: string， 例如`BTC/USDT`

**SymbolInfo结构：**

| 字段名    | 类型   | 说明                 |
| :-------- | :----- | -------------------- |
| limits    | Dict   | 交易对的交易限制信息 |
| precision | Dict   | 交易对精度信息       |
| taker     | float  | taker手续费          |
| maker     | float  | maker手续费          |
| id        | string | 交易对id             |
| symbol    | string | 交易对符号           |
| base      | string | 交易对/前货币名称    |
| quote     | string | 交易对/后货币名称    |
| baseId    | string | 交易对/前货币id      |
| quoteId   | string | 交易对/后货币id      |
| active    | string | 交易对状态           |

 **备注** 

- 暂无



## ticker

**简要描述：** 

- 获取交易对最新行情

**请求方式及请求URL：** 
- GET `/v1/public/spot/ticker`

**参数：** 

| 参数名 | 类型   | 是否必须 | 说明                 |
| :----- | :----- | :------- | -------------------- |
| s      | string | 否       | 交易对名称，/用-代替 |

* 如果参数symbol存在，则返回该symbol的ticker，否则返回所有symbols的ticker

**返回示例**

``` 
{
    "code": "000000",
    "data": {
		"BTC/USDT"： {
			"s": "BTC/USDT",
			"t": 1560994431098,
			"o": 1000,
			"h": 1050,
			"l": 990,
			"c": 1020,
			"p": 0.02,
			"cg": 20,
			"v": 10,
			"to": 10080,
			"a": 1022,
			"b": 1019,
			"aa": 5,
			"ba": 5.5,
		}
	},
    "msg": "OK"
}
```

 **返回值说明** 

| 字段名 | 类型   | 说明                          |
| :----- | :----- | ----------------------------- |
| t      | long   | 当前时间戳，单位毫秒          |
| s      | string | symbol 交易对，例如BTC/USDT   |
| o      | 浮点数 | open 开盘价                   |
| h      | 浮点数 | high 最高价                   |
| l      | 浮点数 | low 最低价                    |
| c      | 浮点数 | close 收盘价                  |
| p      | 浮点数 | percent涨幅                   |
| cg     | 浮点数 | change，开盘价-收盘价         |
| v      | 浮点数 | volume 成交量，以base计算     |
| to     | 浮点数 | turnover，成交额，以quote计算 |
| a      | 浮点数 | ask， 卖一价                  |
| b      | 浮点数 | bid, 买一价                   |
| aa     | 浮点数 | askAmount 卖一挂单量          |
| ba     | 浮点数 | bidAmount 买一挂单量          |


 **备注** 

- 如果仅需要获取最新的价格信息，推荐使用 ticker price 接口，更快
- 如果未指定symbol，返回所有symbol的ticker数据，data数据中，以各个交易对的symbol为键值，value为该symbol的ticker数据



## 深度

**简要描述：** 

- 获取交易对行情深度信息

**请求方式及请求URL：** 
- GET `/v1/public/spot/depth`

**参数：** 

| 参数名 | 类型    | 是否必须   | 说明                                   |
| :----- | :------ | :--------- | -------------------------------------- |
| s      | string  | 是         | 交易对名称，/用-代替                   |
| depth  | integer | 否，默认20 | 返回的深度个数, 可选值：5,10,20,50,100 |

**请求示例**
```
GET /v1/public/spot/depth?s=BTC-USDT&depth=5
```

**返回示例**

``` 
{
    "code": "000000",
    "data": {
        "symbol": "BTC/USDT",
		"asks": [["9.9", "1"], ["9.89", "0.4"], ["9.88","10"], ["9.7", "13.2"], ["9.68", "3.2"]],
		"bids": [["10", "9.8"], ["10.1", "1.2"], ["10.15", "0.9"], ["10.2", "3.55"], ["10.3", "1.6"]]
	},
    "msg": "OK"
}
```

 **返回值说明** 

| 字段名 | 类型              | 说明                                                         |
| :----- | :---------------- | ------------------------------------------------------------ |
|symbol|String|交易对名称|
| asks   | 数组 Array<Array> | 卖单深度数组，数组的每个元素是个数组，由价格，数量组成,按照价格从低到高排列 |
| bids   | 数组 Array<Array> | 买单深度数组，数组的每个元素是个数组，由价格，数量组成,按照价格从高到低排列 |

 **备注** 

- 请求参数中的交易对，要用-代替/，例如BTC-USDT代替BTC/USDT
- 为了防止精度丢失，价格和数量均为string类型



## 成交历史

**请求方式及请求URL：** 
- GET `/v1/public/spot/trades`

**参数：** 

| 参数名 | 类型   | 是否必须 | 说明                 |
| :----- | :----- | :------- | -------------------- |
| s      | string | 是       | 交易对名称，/用-代替 |

**请求示例**

```
GET /v1/public/spot/trades?s=BTC-ETH
```

**返回示例**

``` 
{
    "code": "000000",
    "data": [
		{
        "t":  1502962946216,            // Unix timestamp in milliseconds
        "s":    "ETH/BTC",                 // symbol
        "d":      "buy",                     // direction of the trade, 'buy' or 'sell'
        "p":      "0.06917684",             // float price in quote currency
        "v":     "1.5",                    // amount of base currency
        "to":     "15.12",                    // amount of base currency
		},
	]
    "msg": "OK"
}
```

 **返回值说明** 

| 字段名 | 类型   | 说明                  |
| :----- | :----- | --------------------- |
| t      | long   | 成交时间戳, 单位毫秒  |
| s      | string | 交易对                |
| d      | string | 买卖方向，buy或者sell |
| p      | string | 成交价格              |
| v      | string | 成交量，以base为单位  |
| to     | string | 成交额，以quote为单位 |


 **备注** 

- 暂无



## K线

**简要描述：** 

- 获取交易对K线数据，每根K线的开盘时间可视为唯一ID

**请求方式及请求URL：** 
- GET `/v1/public/spot/kline`

**参数：** 

| 参数名 | 类型   | 是否必须 | 说明                                                         |
| :----- | :----- | :------- | ------------------------------------------------------------ |
| s      | string | 是       | 交易对名称，/用-代替                                         |
| p      | string | 是       | 时间间隔(分钟制:1m，5m，10m, 15m，30m。小时制:1H, 4H, 12H，天制:1D，周： 1W, 月制:1Month) |
| l      | int    | 否       | 返回数据条数，默认500，最大1000条                            |

**请求示例**
```
GET /v1/public/spot/kline?s=BTC-ETH&p=1m
```

**返回示例**

``` 
{
    "code": "000000",
    "data": [
		[
		1566839160000,      // 开盘时间
		"0.01634790",       // 开盘价
		"0.80000000",       // 最高价
		"0.01575800",       // 最低价
		"0.01577100",       // 收盘价(当前K线未结束的即为最新价)
		"148976.11427815",  // 成交量(以base为单位)
		"2434.19055334",    // 成交额(以quote为单位)
		308,                // 成交笔数
		"2019-08-27 01:06:00"
		],
	]
    "msg": "OK"
}
```

 **返回值说明** 


| 数组元素 | 类型   | 说明                              |
| :------- | :----- | --------------------------------- |
| 0        | long   | 开盘时间，单位毫秒                |
| 1        | string | 开盘价                            |
| 2        | string | 最高价                            |
| 3        | string | 最低价                            |
| 4        | string | 收盘价(当前K线未结束的即为最新价) |
| 5        | string | 成交量(以base为单位)              |
| 6        | string | 成交额(以quote为单位)             |
| 7        | int  | 成交笔数                          |
| 8       | string  | 时间|

 **备注** 

- 暂无



# 私有接口

私有接口包括：

* 下单接口
* 取消订单接口
* 查询订单列表接口
* 查询订单详情接口
* 查询资产接口



私有接口必须包含以下参数：

* accessKey
* timestamp，当前时间戳，单位毫秒
* sign，对请求参数的签名



## 订单状态

* TRADING  交易中
* CANCCELING 取消中
* CANCELED 取消成功
* COMPLETED 交易完成

## 委托价格类型
目前仅支持限价单，LIMIT



## 返回码及错误码

接口调用成功时，返回 "000000"; 其他返回为失败状态，错误码如下：

| 错误码      | 说明                  |
| :---------- | :-------------------- |
| ORDERADD001 | 输入下单金额小于等于0 |
| ORDERADD002 | 下单数量小于等于0     |
| ORDERADD003 | 交易对不存在          |



## 签名

所有私有接口都需要校验用户的参数，校验方法如下：
1. 如果是GET方法， 参与签名计算的为？后的所有参数，排序后，作为签名参数；
2. 如果是POST方法，将请求的json结构体作为签名参数，转换为String，并排序后，作为签名参数；
3. 参数中必须有accessKey和timestamp；accessKey是用户的API accessKey，timestamp是当前时间戳，单位是毫秒；
4. 签名算法如下：
a. 将所有参数（不包含sign参数）按照ascii字符从小到大排序，并对参数值使用标准URL Encode编码，然后使用&连接，作为payload；
b. 使用secretKey作为key；
c. 使用HMACsha256算法计算签名；
d. 将签名结果转换为16进制字符串；
e. 将上一步骤的结果作为sign参数

签名过程举例如下：
1. 假设有以下几个参数：
accessKey=ce2a18e0-dshs-4c44-4515-9aca67dd706e
timestamp=1566461351656
price=10000.00
amount=0.01
symbol=BTC-USDT
side=BUY
priceType=LIMIT

排序后，得到字符串如下：
```
accessKey=ce2a18e0-dshs-4c44-4515-9aca67dd706e&amount=0.01&price=10000.00&priceType=LIMIT&side=BUY&symbol=BTC-USDT&timestamp=1566461351656
```

假设secretKey=c11a122s-dshs-shsa-4515-954a67dd706e，则最终计算得到的sign为：
```
xxx
```

最终得到的param参数为：
```
accessKey=ce2a18e0-dshs-4c44-4515-9aca67dd706e&amount=0.01&price=10000.00&priceType=LIMIT&side=BUY&symbol=BTC-USDT&timestamp=1566461351656&sign=xxx
```





## 下单接口

**简要描述：** 

- 下单接口

**请求URL：** 
- `/v1/trade/spot/add`

**请求方式：**
- POST 

**参数：** 

| 参数名    | 必选 | 类型   | 说明                                     |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | 是   | string | accessKey                                |
| price     | 是   | string | 价格                                     |
| amount    | 是   | string | 委托量                                   |
| priceType | 否   | string | 委托类型，目前仅支持限价单LIMIT          |
| symbol    | 是   | string | 交易对，/替换为-，例如BTC-USDT           |
| direction | 是   | string | 买卖方向  买：BUY； 卖：SELL             |
| no        | 是   | string | request no, 如果带，服务器返回相同的参数 |
| sign      | 是   | string | 签名                                     |

 **返回示例**

``` 
  {
    "code": "000000",
    "data": {
      "orderId": "EBLxxxxxxxxxxx"
    },
	"no": ""
  }
```

 **返回参数说明** 

| 参数名  | 类型   | 说明       |
| :------ | :----- | ---------- |
| orderId | string | 订单号     |
| no      | string | request no |

 **备注** 

- 更多返回错误代码请看首页的错误代码描述
- price 和 amount 使用字符串类型，如果使用decimal类型，可能导致精度丢失，造成sign值不一致！



## 取消订单接口

**请求URL：** 
- `/v1/trade/spot/cancel`

**请求方式：**
- POST

**参数：**

| 参数名    | 必选 | 类型   | 说明                                     |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | 是   | string | accessKey                                |
| orderId   | 是   | string | 价格                                     |
| no        | 是   | string | request no, 如果带，服务器返回相同的参数 |
| sign      | 是   | string | 签名                                     |

 **返回示例**

```
  {
    "code": "000000"
	"data": {
		"orderId": ""
	},
	"no": ""
  }
```

 **返回参数说明** 

| 参数名  | 类型   | 说明       |
| :------ | :----- | ---------- |
| orderId | string | 订单号     |
| no      | string | request no |

 **备注**

- 该接口为异步接口，服务器返回成功不代表该订单取消成功，用户必须调用订单详情来获取订单状态来判断该订单是否取消成功



##  查询单个订单详情接口

**请求URL：**
- `/v1/trade/spot/detail`

**请求方式：**
- GET/POST， 推荐使用POST方式

**参数：**

| 参数名    | 必选 | 类型   | 说明                                     |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | 是   | string | accessKey                                |
| orderId   | 是   | string | 价格                                     |
| no        | 是   | string | request no, 如果带，服务器返回相同的参数 |
| sign      | 是   | string | 签名                                     |

 **返回示例**

``` 
  {
    "code": "000000"
	"data": {
        "amount": 0,
        "baseSymbol": "string",
        "coinSymbol": "string",
        "completedTime": 0,
        "createTime": "2019-08-22T11:31:47.453Z",
        "directionStr": "string",
        "dk": 0,
        "fee": 0,
        "memberId": 0,
        "orderId": "string",
        "price": 0,
        "statusStr": "string",
        "symbol": "string",
        "time": 0,
        "tradedAmount": 0,
        "tradedAvgPrice": 0,
        "turnover": 0,
        "typeStr": "string"
		"details": [{
			"tradedAmount": 0,
			"dk": 0,
			"fee": 0,
			"price": 0,
			"time": 0,
			"turnover": 0
		}]
	},
	"no": ""
  }
```

 **返回参数说明** 

| 字段名         | 类型    | 说明                                      |
| :------------- | :------ | ----------------------------------------- |
| amount         | string  | 订单号                                    |
| baseSymbol     | string  | 目标货币，交易对"/"后的名称               |
| coinSymbol     | string  | 交易货币，交易对"/"前的名称               |
| completedTime  | string  | 完成时间，仅当order处于完成态的时候才有值 |
| createTime     | string  | 订单创建时间                              |
| directionStr   | string  | 买卖方向：BUY，SELL                       |
| dk             | decimal | 点卡使用                                  |
| fee            | string  | 交易佣金                                  |
| memberId       | long    | 用户id                                    |
| orderId        | string  | 订单id                                    |
| price          | string  | 订单挂单价格                              |
| statusStr      | string  | 订单状态                                  |
| symbol         | string  | 订单交易对名称                            |
| time           | string  | 成交时间戳，单位毫秒                      |
| tradedAmount   | string  | 成交数量                                  |
| tradedAvgPrice | string  | 成交平均价格                              |
| turnover       | string  | 成交金额                                  |
| typeStr        | string  | 价格类型，目前仅有限价类型LIMIT_PRICE     |


details是该订单已经成交的交易，该结构中的字段说明如下：

| 字段名         | 类型    | 说明                 |
| :------------- | :------ | -------------------- |
| dk             | decimal | 点卡使用             |
| fee            | string  | 交易佣金             |
| memberId       | long    | 用户id               |
| orderId        | string  | 订单id               |
| price          | string  | 订单挂单价格         |
| symbol         | string  | 订单交易对名称       |
| time           | string  | 成交时间戳，单位毫秒 |
| tradedAmount   | string  | 成交数量             |
| tradedAvgPrice | string  | 成交平均价格         |

## 根据状态获取订单接口

**请求URL：**

- `/v1/trade/spot/listOrders`

**请求方式：**

- GET/POST， 推荐使用POST方式

**参数：**

| 参数名    | 必选 | 类型   | 说明                                           |
| :-------- | :--- | :----- | ---------------------------------------------- |
| accessKey | 是   | string | accessKey                                      |
| status    | 是   | string | 见下文描述                                     |
| symbol    | 否   | string | 交易对，如果没有该参数，返回所有交易对         |
| pageNum   | 否   | int    | 分页, 页码从1开始                              |
| pageSize  | 否   | int    | 每页返回数量，默认10，最大100                  |
| side      | 否   | string | 交易方向：BUY， SELL                           |
| status    | 否   | string | 交易状态                                       |
| startTime | 否   | long   | 毫秒级时间戳，查询订单创建时间晚于该时间的订单 |
| endTime   | 否   | long   | 毫秒级时间戳，查询订单创建时间早于该时间的订单 |
| no        | 是   | string | request no, 如果带，服务器返回相同的参数       |
| sign      | 是   | string | 签名                                           |

**status参数**
status参数取值如下：

| 取值      | 说明                             |
| :-------- | :------------------------------- |
| TRADING   | 交易中的订单，包括部分成交的订单 |
| COMPLETED | 交易完成的订单                   |
| CANCELED  | 已经取消的订单                   |
| CANCELING | 取消中的订单                     |

 **返回示例**

``` 
  {
    "code": "000000"
	"data": {
	"pages": 12,
	"total": 112,
	"list": [{
        "amount": 0,
        "baseSymbol": "string",
        "coinSymbol": "string",
        "completedTime": 0,
        "createTime": "2019-08-22T11:31:47.453Z",
        "directionStr": "string",
        "dk": 0,
        "fee": 0,
        "memberId": 0,
        "orderId": "string",
        "price": 0,
        "statusStr": "string",
        "symbol": "string",
        "time": 0,
        "tradedAmount": 0,
        "tradedAvgPrice": 0,
        "turnover": 0,
        "typeStr": "string"
	}]
	},
	"no": ""
  }
```

 **返回结果说明**

| 字段名 | 类型  | 说明     |
| :----- | :---- | -------- |
| list   | Array | 订单列表 |
| pages  | int   | 总页数   |
| total  | int   | 订单总数 |

 **返回结果list数组说明**

| 字段名         | 类型    | 说明                                      |
| :------------- | :------ | ----------------------------------------- |
| amount         | string  | 订单号                                    |
| baseSymbol     | string  | 目标货币，交易对"/"后的名称               |
| coinSymbol     | string  | 交易货币，交易对"/"前的名称               |
| completedTime  | string  | 完成时间，仅当order处于完成态的时候才有值 |
| createTime     | string  | 订单创建时间                              |
| directionStr   | string  | 买卖方向：BUY，SELL                       |
| dk             | decimal | 点卡使用                                  |
| fee            | string  | 交易佣金                                  |
| memberId       | long    | 用户id                                    |
| orderId        | string  | 订单id                                    |
| price          | string  | 订单挂单价格                              |
| statusStr      | string  | 订单状态                                  |
| symbol         | string  | 订单交易对名称                            |
| time           | string  | 成交时间戳，单位毫秒                      |
| tradedAmount   | string  | 成交数量                                  |
| tradedAvgPrice | string  | 成交平均价格                              |
| turnover       | string  | 成交金额                                  |
| typeStr        | string  | 价格类型，目前仅有限价类型LIMIT_PRICE     |



## 获取用户balance接口

**请求URL：**
- `/v1/trade/spot/balance`

**请求方式：**

- GET/POST， 推荐使用POST方式

**参数：**

| 参数名    | 必选 | 类型   | 说明                                     |
| :-------- | :--- | :----- | ---------------------------------------- |
| accessKey | 是   | string | accessKey                                |
| no        | 是   | string | request no, 如果带，服务器返回相同的参数 |
| sign      | 是   | string | 签名                                     |

 **返回示例**

```
  {
    "code": "000000"
	"data": {
		"BTC": {
		"balanceAvailable": "1.0",
        "balanceFrozen": "0.0",
        "balanceOTCFrozen": "0.0"
		}
	},
	"no":""
  }
```

**返回参数说明**

| 参数名           | 类型   | 说明                |
| :--------------- | :----- | ------------------- |
| balanceAvailable | String | 可用数量            |
| balanceFrozen    | String | 冻结数量            |
| balanceOTCFrozen | String | OTC交易冻结资金数量 |



# 附录

## 签名算法



**JAVA校验签名代码如下：**



```
public class HMACUtil {
    private static final String signatureMethodValue = "HmacSHA256";
    private static final String signKey = "sign";

    static public String toStringByKey(Map<String, Object> json) throws Exception {
        Map<String, String> map = new TreeMap<>();
        for (String key: json.keySet()) {
            if (key.equals(signKey)) {
                continue;
            }
            map.put(key, json.get(key).toString());
        }

        StringBuilder sb = new StringBuilder(1024);
        for (Map.Entry<String, String> entry : map.entrySet()) {
            if (!("").equals(sb.toString())) {
                sb.append("&");
            }

            sb.append(entry.getKey());
            sb.append("=");
            sb.append(urlEncode((String)entry.getValue()));
        }

        return sb.toString();
    }

    static public String hmacSign(JSONObject json, String secretKey) throws Exception {
        String payload = toStringByKey(json);
        return hmacSign(payload, secretKey);
    }

    /**
     * 使用标准URL Encode编码。注意和JDK默认的不同，空格被编码为%20而不是+。
     *
     * @param s String字符串
     * @return URL编码后的字符串
     */
    private static String urlEncode(String s) throws Exception {
        try {
            return URLEncoder.encode(s, "UTF-8").replaceAll("\\+", "%20");
        } catch (UnsupportedEncodingException e) {
            throw new Exception("[URL] UTF-8 encoding not supported!");
        }
    }

    static public String hmacSign(String payload, String secretKey) throws Exception {
        Mac hmacSha256;

        hmacSha256 = Mac.getInstance(signatureMethodValue);
        SecretKeySpec secKey = new SecretKeySpec(secretKey.getBytes(StandardCharsets.UTF_8),
                signatureMethodValue);
        hmacSha256.init(secKey);

        byte[] hash = hmacSha256.doFinal(payload.getBytes(StandardCharsets.UTF_8));

        return Hex.encodeHexString(hash);
    }
}
```





**python计算签名代码如下：**



```
import hmac
import hashlib

# msg 是排序完成的字符串
def hmacsha256(msg, secret):
    hash = hmac.new(secret, msg=msg, digestmod=hashlib.sha256).hexdigest()
    return hash
```





**javascript计算签名代码如下：**



```
const CryptoJS = require("crypto-js")

// 对参数排序
function toStringByKey(param) {
    let result = ''
    let keys = []

    for (let key in param) {
        keys.push(key)
    }
    keys.sort()
    keys.forEach(key => {
            let val = param[key]
            result += key + '='
            if (typeof val === 'string') {
                result += encodeURIComponent(val)
            } else if (typeof val === 'number') {
                result += encodeURIComponent(+val)
            }
        result += '&'
    })

    return result.slice(0, result.length-1)
}

// payload 是排序完成的字符串
function sign(payload, secret) {
	let hash = CryptoJS.HmacSHA256(payload, secret)
	// return CryptoJS.enc.Base64.stringify(hash)
	return CryptoJS.enc.Hex.stringify(hash)
}
```