# WTZEx WebSocket接口



## API请求域名

```
wss://wss.wtzex.io/ws/client/h5/1000
```

## 返回code

所有接口，均返回code字段，code字段值为字符串“000000”代表成功，其他值均为失败。



websocket接口返回与HTTP接口返回字段相同，字段含义请参阅HTTP接口。



# 公有接口

公共接口中，返回字段有一个

```
transType
```

该字段在订阅trade时，表示本次返回是增量数据还是全量数据。取值如下：

* all 全量数据
* inc  增量数据



当第一次订阅trade时，返回全量数据；后面的返回为增量数据。



## 心跳消息

当用户的Websocket客户端连接到Websocket服务器后，服务器会定期（当前设为20秒）向其发送ping消息并包含一整数值如下：

```
{ "ping":1492420473027 }
```

当用户的Websocket客户端接收到此心跳消息后，应返回pong消息并包含同一整数值：
```
{ "pong":1492420473027 }
```

当Websocket服务器连续三次发送了`ping`消息却没有收到任何一次`pong`消息返回后，服务器将主动断开与此客户端的连接。

## 深度

订阅深度的格式如下：

```
{
	"sub": "market.depth",
	"symbol": ""
}
```

symbol是交易对，例如：

```
{
"sub": "market.depth",
"symbol": "BTC/USDT"
}
```

返回数据样例：

```
{
    "code":"000000",
    "data":{
        "symbol":"BTC/USDT",
        "asks":[
            [
                10265.33,
                0.3991
            ],
            [
                10265.4,
                0.3345
            ],
            [
                10266.31,
                0.384
            ],
            [
                10266.96,
                0.3862
            ],
            [
                10267.85,
                4.4654
            ],
            [
                10268.11,
                0.3751
            ],
            [
                10268.22,
                0.3464
            ],
            [
                10268.5,
                0.3477
            ],
            [
                10270.65,
                0.0056
            ],
            [
                10299.35,
                0.498
            ],
            [
                10299.36,
                0.885
            ],
            [
                10334.59,
                5.147
            ],
            [
                10334.6,
                0.0963
            ],
            [
                10339.28,
                0.766
            ],
            [
                10346.61,
                1.508
            ],
            [
                10370,
                0.0284
            ],
            [
                10385.99,
                0.492
            ],
            [
                10386,
                15.968
            ],
            [
                10389.99,
                0.0524
            ],
            [
                10393,
                0.36
            ]
        ],
        "bids":[
            [
                10219.21,
                0.5712
            ],
            [
                10190.17,
                0.498
            ],
            [
                10190.16,
                0.0302
            ],
            [
                10166.05,
                5.148
            ],
            [
                10166.04,
                0.0191
            ],
            [
                10135,
                0.11
            ],
            [
                10120.7,
                2.0276
            ],
            [
                10118,
                0.05
            ],
            [
                10115,
                0.0358
            ],
            [
                10109.96,
                1.509
            ],
            [
                10100,
                2.1004
            ],
            [
                10088,
                16
            ],
            [
                10086,
                0.434
            ],
            [
                10070,
                0.2297
            ],
            [
                10058,
                0.1
            ],
            [
                10050,
                0.3
            ],
            [
                10039,
                0.055
            ],
            [
                10028.01,
                0.493
            ],
            [
                10028,
                4
            ],
            [
                10020,
                0.7328
            ]
        ]
    },
    "msg":"success",
    "msgType":"sub",
    "respStatus":"ok",
    "sub":"market.depth",
    "transType":"all"
}
```



## ticker

订阅深度的ticker如下：

```
market.{{symbol}}.ticker
或者
market.ticker
```

symbol是交易对，用_代替/，例如：

```
{
"sub": "market.btc_usdt.ticker"
}
```

如果没有symbol参数，订阅所有交易对的ticker，例如：

```
{
"sub": "market.ticker"
}
```



订阅所有交易对的返回：

```
{
	"code":"000000",
	"data":{
		"HT/USDT": {
			"pp":4,
			"c":"4.9000",
			"cg":"0.0000",
			"h":"4.9000",
			"l":"4.9000",
			"o":"4.9000",
			"p":"0.000",
			"ct":0,
			"s":"HT/USDT",
			"t":1566813000000,
			"v":"0.00",
			"vp":2,
			"to":"0.0000"
		},
		"ETC/USDT":{
			"pp":4,
			"c":"6.1600",
			"cg":"0.0000",
			"h":"6.1600",
			"l":"6.1600",
			"o":"6.1600",
			"p":"0.000",
			"ct":0,
			"s":"ETC/USDT",
			"t":1566813000000,
			"v":"0.00",
			"vp":2,
			"to":"0.0000"
		},
		"BTC/USDT":{
			"pp":4,
			"aa":"0.6287",
			"a":"10245.2900",
			"b":"10245.2900",
			"c":"10218.8900",
			"cg":"-113.7800",
			"h":"10410.1600",
			"l":"10155.3400",
			"o":"10332.6700",
			"p":"-0.011",
			"ct":0,
			"s":"BTC/USDT",
			"t":1566884793905,
			"v":"2042.5520",
			"vp":4,
			"to":"21042744.3640",
			"ba":"0.6287"
			}
		},
		"msg":"success",
		"msgType":"sub",
		"respStatus":"ok",
		"sub":"market.ticker",
		"transType":"all"}
```



订阅特定交易对的返回：

```
{
    "code":"000000",
    "data":{
        "BTC/USDT":{
            "pp":4,
            "aa":"0.3951",
            "a":"10259.3100",
            "b":"10259.3100",
            "c":"10231.1800",
            "cg":"-101.4900",
            "h":"10410.1600",
            "l":"10155.3400",
            "o":"10332.6700",
            "p":"-0.010",
            "ct":0,
            "s":"BTC/USDT",
            "t":1566884945807,
            "v":"2055.3562",
            "vp":4,
            "to":"21173797.4355",
            "ba":"0.3951"
        }
    },
    "msg":"success",
    "msgType":"sub",
    "respStatus":"ok",
    "sub":"market.ticker",
    "transType":"all"
}
```



## trades

订阅深度的格式如下：

```
{
	"sub": "market.trade",
	"symbol": ""
}
```

symbol是交易对，例如：

```
{
"sub": "market.trade",
"symbol": "BTC/USDT"
}
```



全量数据：

```
{
    "code":"000000",
    "data":[
        {
            "amount":0.017,
            "buyOrderId":"EBL90001033201908271435509944348688",
            "buyTurnover":173.55776,
            "direction":"SELL",
            "price":10209.28,
            "sellOrderId":"ESL90001033201908271435509943603886",
            "sellTurnover":173.55776,
            "symbol":"BTC/USDT",
            "tickerOrderId":"ESL90001033201908271435509943603886",
            "time":1566887751048,
            "type":"LIMIT_PRICE"
        },
        {
            "amount":0.0022,
            "buyOrderId":"EBL90001033201908271435483298289932",
            "buyTurnover":22.461626,
            "direction":"BUY",
            "price":10209.83,
            "sellOrderId":"ESL90001033201908271435483298867217",
            "sellTurnover":22.461626,
            "symbol":"BTC/USDT",
            "tickerOrderId":"EBL90001033201908271435483298289932",
            "time":1566887748360,
            "type":"LIMIT_PRICE"
        },
        {
            "amount":0.041,
            "buyOrderId":"EBL90001033201908271435252365897792",
            "buyTurnover":418.73054,
            "direction":"BUY",
            "price":10212.94,
            "sellOrderId":"ESL90001033201908271435252373823301",
            "sellTurnover":418.73054,
            "symbol":"BTC/USDT",
            "tickerOrderId":"EBL90001033201908271435252365897792",
            "time":1566887725325,
            "type":"LIMIT_PRICE"
        },
        {
            "amount":0.0503,
            "buyOrderId":"EBL90001033201908271435057018345629",
            "buyTurnover":513.841159,
            "direction":"SELL",
            "price":10215.53,
            "sellOrderId":"ESL90001033201908271435057013939335",
            "sellTurnover":513.841159,
            "symbol":"BTC/USDT",
            "tickerOrderId":"ESL90001033201908271435057013939335",
            "time":1566887705740,
            "type":"LIMIT_PRICE"
        },
        {
            "amount":0.0264,
            "buyOrderId":"EBL90001033201908271435021525634485",
            "buyTurnover":269.6958,
            "direction":"SELL",
            "price":10215.75,
            "sellOrderId":"ESL90001033201908271435021525825240",
            "sellTurnover":269.6958,
            "symbol":"BTC/USDT",
            "tickerOrderId":"ESL90001033201908271435021525825240",
            "time":1566887702208,
            "type":"LIMIT_PRICE"
        },
        {
            "amount":0.0854,
            "buyOrderId":"EBL90001000201908261002303086842068",
            "buyTurnover":881.97704,
            "direction":"SELL",
            "price":10327.6,
            "sellOrderId":"ESL90001033201908262255047589440267",
            "sellTurnover":881.97704,
            "symbol":"BTC/USDT",
            "tickerOrderId":"ESL90001033201908262255047589440267",
            "time":1566831305419,
            "type":"LIMIT_PRICE"
        }
    ],
    "msg":"success",
    "msgType":"sub",
    "respStatus":"ok",
    "sub":"market.BTC_USDT.trade",
    "transType":"all"
}
```





增量数据：

```
{
    "code":"000000",
    "data":[
        {
            "symbol":"BTC/USDT",
            "amount":0.0085,
            "buyTurnover":86.783555,
            "sellTurnover":86.783555,
            "type":"LIMIT_PRICE",
            "sellOrderId":"ESL90001033201908271435590024766422",
            "buyUserId":16780003,
            "sellOrder":{
                "symbol":"BTC/USDT",
                "amount":0.0085,
                "statusStr":"TRADING",
                "orderId":"ESL90001033201908271435590024766422",
                "machineNodeId":"8",
                "useDiscount":"0",
                "baseSymbol":"USDT",
                "typeStr":"LIMIT_PRICE",
                "tradedAmount":0.0085,
                "coinSymbol":"BTC",
                "price":10209.83,
                "directionStr":"SELL",
                "time":1566887759001,
                "userType":"normal",
                "turnover":86.783555,
                "memberId":16780003
            },
            "buyOrderId":"EBL90001033201908271435590015143722",
            "sellUserId":16780003,
            "tickerOrderId":"ESL90001033201908271435590024766422",
            "price":10209.83,
            "time":1566887759042,
            "buyOrder":{
                "symbol":"BTC/USDT",
                "amount":0.0085,
                "statusStr":"TRADING",
                "orderId":"EBL90001033201908271435590015143722",
                "machineNodeId":"8",
                "useDiscount":"0",
                "baseSymbol":"USDT",
                "typeStr":"LIMIT_PRICE",
                "tradedAmount":0.0085,
                "coinSymbol":"BTC",
                "price":10209.83,
                "directionStr":"BUY",
                "time":1566887759001,
                "userType":"normal",
                "turnover":86.783555,
                "memberId":16780003
            },
            "tradeId":"615917432256106496",
            "direction":"SELL"
        }
    ],
    "msg":"success",
    "msgType":"data",
    "sub":"market.BTC_USDT.trade",
    "transType":"inc"
}
```



## K线

K线不提供websocket订阅接口，请使用HTTP接口获取



## 私有接口

* 使用接口使用opt字段标识操作类型，例如addOrder, cancelOrder等
* 签名的字段是对optData进行签名
* 本章的例子中
  * accessKey=sU3rVWSuinNwA5IQxjaqt9emyTWdkade
  * secretKey=sh95xNNHfP0rLn9s8ZAcpSSogROjOnmayiT5DxeCbaL7WMjdOkqHPtNxXx0NRkH9



## 下单

```
{
	"opt": "addOrder"
	"optData": {
		"accessKey": "",
		"no": "",
		"timestamp": 1560994431098,
		"direction": side,
         "symbol": symbol,
         "price": price,
         "amount": amount,
         "sign": ""
	}
}
```

说明：

* side：交易方向，取值"BUY", "SELL"



例如：

```
{
	"opt":"addOrder",
	"optData":{
        "no":123456,
        "symbol":"EOS/USDT",
        "price":"1.0",
        "amount":"1.0",
        "direction":"BUY",
        "timestamp":1566891676570,
        "accessKey":"sU3rVWSuinNwA5IQxjaqt9emyTWdkade",
        "sign":"1214813d2a3fa0195b4f5f3fc49a89f76dcf5c3f2966d7d72908a0b8f670d5ec"
	}
}
```

返回结果如下：

```
{
	"msg":"success",
	"no":"123456",
	"opt":"addOrder",
	"code":"000000",
	"data":{"orderId":"EBL90002033201908271541165957367701"}
}
```



## 撤单



```
{
	"opt": "cancelOrder"
	"optData": {
		"accessKey": "",
		"no": "",
         "symbol": "BTC-USDT",
		"timestamp": 1560994431098,
		"orderId": "",
         "sign": ""
	}
}
```



例如：

```
{
	"opt":"cancelOrder",
	"optData":{
	"no":123456,
	"orderId":"EBL90002033201908271603419159347442",
	"timestamp":1566893038346,
	"accessKey":"sU3rVWSuinNwA5IQxjaqt9emyTWdkade",
	"sign":"228ebb3626b635d88e1c2da543dfd0de8c3c019f88bc68412c2857326945ff01"}
}
```



响应：

```
{"msg":"success","no":"123456","opt":"cancelOrder","code":"000000"}
```



注：该接口是异步接口，返回成功并不代表取消成功，需要用户调用查询结果查询该订单状态是否取消成功。



## 查询单个订单详情

```
{
	"opt": "orderDetail"
	"optData": {
		"accessKey": "",
		"no": "",
         "symbol": "BTC-USDT",
		"timestamp": 1560994431098,
		"orderId": "",
         "sign": ""
	}
}
```



例如：

```
{
	"opt": "orderDetail",
	"optData": {
		"no": 123456,
		"orderId": "EBL90002033201908271541165957367701",
		"timestamp": 1566892754972,
		"accessKey": "sU3rVWSuinNwA5IQxjaqt9emyTWdkade",
		"sign": "97808336ee3be2b9f3e3d031238b052b570cee64326d49a3d492fb7659834a93"
	}
}
```

响应：

```
{
	"msg": "success",
	"no": "123456",
	"opt": "orderDetail",
	"code": "000000",
	"data": {
		"symbol": "EOS/USDT",
		"statusStr": "TRADING",
		"orderId": "EBL90002033201908271541165957367701",
		"dk": 0.0,
		"fee": 0.0,
		"baseSymbol": "USDT",
		"coinSymbol": "EOS",
		"price": 1.0,
		"turnover": 0.0,
		"memberId": 16780003,
		"amount": 1.0,
		"tradedAvgPrice": 0.0,
		"typeStr": "LIMIT_PRICE",
		"tradedAmount": 0.0,
		"createTime": 1566891677000,
		"detailList": [],
		"directionStr": "BUY",
		"time": 1566891676593,
		"userType": "normal"
	}
}
```





## 订阅订单状态变化

```
{
	"sub": "market.order.myorders"
	"optData": {
		"accessKey": "",
         "symbol": "BTC-USDT",
		"timestamp": 1560994431098,
         	"sign": ""
	}
}
```





## 取消订阅depth

```
{
	"unsub": "market.depth",
	"symbol": "BTC/USDT"
}
```





## 取消订阅ticker

```
{
	"unsub": "market.ticker"
}
```



## 取消订阅 trade

```
{
	"unsub": "market.trade",
	"symbol": "BTC/USDT"
}
```

