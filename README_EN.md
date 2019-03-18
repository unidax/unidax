# UniDax REST API
# Basic Information
* The baseurl of the REST API in this article: https://api.unidax.com
* The response of all interfaces is in JSON format.

# General rules
## 
Request parameters are sorted by dictionary，then stitched into a string in the form of keyvalue，finally sign=MD5(string+secretKey)。Note: If the value of the request parameter is NULL, the signature string is not counted when splicing the string.
Parameters are as follows：
```
{
'symbol': 'ltcbtc',
'api_key': '0816016bb06417f50327e2b557d39aaa',
'sign': 'a169740d0588141ef70b71cf11ff8bf3',
'time': '1522055680'
}
```
After the splicing is completed：
string=api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680
sign = MD5(string+secretKey) = MD5(api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680xxxxxxxxxxxxxxxxx)
finally：sign=48105baf432a6626051a72fb160b617e

* submit data in a form format when requesting post parameters
  content-type:application/x-www-form-urlencoded
  
## error code

| error code | description | remark |
| --- | --- | --- |
| 0  |success|   |    
| 5 | order failed |  |
| 6 | the quantity is less than the minimum |  |
| 7 | the quantity is greater than the maximum |  |
| 8 | order cancellation failed |  |
| 9 | the transaction is frozen |  |
| 13 | sorry, the program has a system error, please contact the webmaster |  |
| 19 | insufficient available balance  |  |
| 22 | order does not exist |  |
| 23 | missing transaction quantity parameter |  |
| 24 | missing transaction price parameter |  |
| 100001 | system exception |  |
| 100002 | system upgrade |  |
| 100004 | request parameter is invalid |  |
| 100005 | parameter signature error |  |
| 100007 | illegal IP |  |
| 110002 | unknown cryptocurrency code |  |
| 110003 | fund password error |  |
| 110004 | cash withdrawal is frozen |  |
| 110005 | insufficient available balance |  |
| 110020 | username does not exist |  |
| 110023 | phone number is registered |  |
| 110024 | the email is registered |  |
| 110025 | account locked by  admin |  |
| 110032 | this user does not have permission to do this |  |
| 110033 | recharge failed |  |
| 110034 | withdrawal failure |  |

# API information
## market

| API | description |
| --- | --- |
| /open/api/get_records | get K line data |
| /open/api/get_ticker | get transaction record |
| /open/api/get_trades | get the market transaction record |
| /open/api/market_dept | query market depth |

## transaction

| API | description |
| --- | --- |
| /open/api/all_order | get all orders |
| /open/api/all_trade | get all the transaction records |
| /open/api/cancel_order | cancel all orders |
| /open/api/common/symbols | query all transaction pairs and precision supported by the system |
| /open/api/create_order | create order |
| /open/api/market | get the latest transaction price for each transaction pair |
| /open/api/new_order | get the current latest oder |
| /open/api/order_info | get order details |
| /open/api/user/account | asset balance |

# API detail
## GET /open/api/get_records    get K line data
The interface does not perform signature verification.
* parameter

| parameter | type | remark |
| --- | --- | --- |
| symbol | essential | market mark, bchbtc, see below for details |
| period | essential | the unit is minutes, such as 1 for 1 minute and 1440 for one day. |
* return


| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |  as blow：|
```
[
    [
      1514445780, //timestamp 
      1.12, //opening price 
      1.12, //high price 
      1.12, //low price
      1.12, //closing price
      0 //volume
    ],
    [
      1514445780, //timestamp
      1.12, //opening price  
      1.12, //high price  
      1.12, //low price 
      1.12, //closing price 
      0 //volume
    ],
    [
      1514445780, //timestamp 
      1.12, //opening price  
      1.12, //high price  
      1.12, //low price 
      1.12, //closing price 
      0 //volume
    ]
]
```
## GET /open/api/get_ticker  get transaction record
* parameter


| parameter | type | remark |
| --- | --- | --- |
| symbol | essential | market mark, bchbtc, see below for details |

* return

| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |  as blow：|
```
{
    "high": 1,//highest value
    "vol": 10232.26315789,//trading volume "last": 173.60263169,//latest transaction price "low": 0.01,//lowest value
    "buy": "0.01000000",//bid1
    "sell": "1.12345680",//ask1 "time": 1514448473626
}

```
## GET /open/api/get_trades get the market transaction record
* parameter

| parameter | type | remark |
| --- | --- | --- |
| symbol | essential | market mark，btcusdt，see below for details |

* return

| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |  as blow：|
```
[
    {
        "amount": 0.55,//volume
        "price": 0.18519949,//final price
        "id": 447121,
        "type": "buy"//buy and sell 
    },
    {
        "amount": 16.45,
        "price": 0.18335468,
        "id": 447120,
        "type": "buy"
    }
]
```

## GET /open/api/market_dept query market depth
* parameter

| parameter | type | remark |
| --- | --- | --- |
| symbol | essential | market mark，btcusdt，see below for details |
| type | essential | depth type，step0, step1, step2(Merging depth 0-2);The highest precision when step0 |
* return

| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |  as blow：|
```
{
    "tick":{
            "asks":[//sell
                        {22112.22,0.9332}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2},
                    ],
            "bids":[//buy
                        {22112.22,0.9332}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2},
                    ]
            }
}
```
## GET /open/api/all_order get all orders
* parameter

| parameter | type | remark |
| --- | --- | --- |
| symbol | essential | market mark，btcusdt，see below for details |
| pageSize | optional | |
| page | optional | |
| api_key | essential |api_key |
| time | essential | timestamp|
| sign | essential | signature|
* return

| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |  as blow：|
```
    {
    "count":10,
    "orderList":[
        {
            "side":"BUY",
            "total_price":"0.10000000",
            "created_at":1510993841000,
            "avg_price":"0.10000000",
            "countCoin":"btc",
            "source":1,
            "type":1,
            "side_msg":"buy", 
            "volume":"1.000", 
            "price":"0.10000000",
            "source_msg":"WEB", 
            "status_msg":"complete transaction", 
            "deal_volume":"1.00000000", 
            "id":424, 
            "remain_volume":"0.00000000", 
            "baseCoin":"eth", 
            "tradeList":[
                {
                    "volume":"1.000", 
                    "feeCoin":"YLB", 
                    "price":"0.10000000", 
                    "fee":"0.16431104", 
                    "ctime":1510996571195, 
                    "deal_price":"0.10000000", 
                    "id":306,
                    "type":"buy"
                } 
            ],
            "status":2
        },
        {
            "side":"SELL",
            "total_price":"0.09900000",
            "created_at":1510993715000,
            "avg_price":"0.10000000",
            "countCoin":"btc",
            "source":1,
            "type":1,
            "side_msg":"sell", 
            "volume":"1.000", 
            "price":"0.09900000", 
            "source_msg":"WEB", 
            "status_msg":"complete transaction", 
            "deal_volume":"1.00000000", 
            "id":423, 
            "remain_volume":"0.00000000", 
            "baseCoin":"eth", 
            "tradeList":[
                {
                    "volume":"1.000", 
                    "feeCoin":"YLB", 
                    "price":"0.10000000", 
                    "fee":"0.16597075", 
                    "ctime":1510993723973, 
                    "deal_price":"0.10000000", 
                    "id":261,
                    "type":"sell"
                } 
            ],
            "status":2
         } 
    ]
}
```
## GET /open/api/all_trade get all the transaction records
* parameter

| parameter | type | remark |
| --- | --- | --- |
| symbol | essential | market mark，btcusdt，see below for details |
| pageSize | optional | |
| page | optional | |
| api_key | essential |api_key |
| time | essential | timestamp|
| sort | optional | 1 indicates reverse order|
| sign | essential | signature|
* return


| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |  as blow：|
```
{
    "count":22,
    "resultList":[
        {
            "volume":"1.000",
            "side":"BUY",
            "feeCoin":"YLB",
            "price":"0.10000000",
            "fee":"0.16431104",
            "ctime":1510996571195,
            "deal_price":"0.10000000",
            "id":306,
            "type":"buy", 
            "bid_user_id":10001, 
            "ask_user_id":10001
        },
        {
            "volume":"1.000",
            "side":"BUY",
            "feeCoin":"YLB",
            "price":"0.10000000",
            "fee":"0.16431104",
            "ctime":1510996571195,
            "deal_price":"0.10000000",
            "id":306,
            "type":"sell", 
            "bid_user_id":10001, 
            "ask_user_id":10001
        },
        {
            "volume":"1.000",
            "side":"BUY",
            "feeCoin":"YLB",
            "price":"0.10000000",
            "fee":"0.16431104",
            "ctime":1510996571195,
            "deal_price":"0.10000000",
            "id":306,
            "type":"buy", 
            "bid_user_id":10001, 
            "ask_user_id":10001
        }
    ]
}
```
## GET /open/api/cancel_order   cancel all orders
* parameter

| parameter | type | remark |
| --- | --- | --- |
| order_id | essential | order ID |
| symbol | essential | market mark，btcusdt，see below for details |
| api_key | essential |api_key |
| time | essential | timestamp|
| sign | essential | signature|
* return


| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |  |

## GET /open/api/common/symbols  query all transaction pairs and precision supported by the system
* parameter

| parameter | type | remark |
| --- | --- | --- |
| none | | |
* return

| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |   |as blow|
```
{
    "code": "0",
    "msg": "suc",
    "data": [
        {
            "symbol": "ethbtc", //Transaction pair
            "count_coin": "btc", //pricing currency
            "amount_precision": 3, //number of digits of precision (0 is a single digit)
            "base_coin": "eth", //base currency
            "price_precision": 8 //number of digits of price (0 is a single digit)
        },
        {
            "symbol": "ltcbtc", 
            "count_coin": "btc", 
            "amount_precision": 2, 
            "base_coin": "ltc", 
            "price_precision": 8 
        },
        {
            "symbol": "bchbtc", 
            "count_coin": "btc", 
            "amount_precision": 3, 
            "base_coin": "bch", 
            "price_precision": 8 
        },
        {
            "symbol": "etcbtc", 
            "count_coin": "btc", 
            "amount_precision": 2, 
            "base_coin": "etc", 
            "price_precision": 8 
        },
        {
            "symbol": "ltceth", 
            "count_coin": "eth", 
            "amount_precision": 2, 
            "base_coin": "ltc", 
            "price_precision": 8 
        },
        {
            "symbol": "etceth", 
            "count_coin": "eth", 
            "amount_precision": 2, 
            "base_coin": "etc", 
            "price_precision": 8
        }
    ]
}
```
## POST /open/api/create_order  create order
* parameter

| parameter | type | remark |
| --- | --- | --- |
| side | essential | buying and selling direction BUY、SELL |
| type | essential | pending order type, 1: limit order commission, 2: market price commission |
| volume | essential |purchase quantity (multi-meaning, reuse field) type=1:indicates the number of buy or sell type=2:buy means the total price, and sell means the total number |
| price | optional | entrusted unit price:type=2:this parameter is not required|
| fee_is_user_exchange_coin | optional | 0，when trading all platform coins, this parameter indicates whether to use the platform currency to pay the handling fee, 0 no, 1 is|
| api_key | essential | api_key|
| time | essential | timestamp|
| sign | essential | signature|
* return


| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |  {"order_id":34343} |  successfully returned transaction ID|

## GET /open/api/market   get the latest transaction price for each transaction pair
* parameter

| parameter | type | remark |
| --- | --- | --- |
| api_key | essential | api_key|
| time | essential | timestamp|
| sign | essential | signature|
* return

| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |  {"btcusdt":15000,"ethusdt":800}|  |

## GET /open/api/new_order   get the current latest oder
* parameter

| parameter | type | remark |
| --- | --- | --- |
| symbol | essential | market mark，btcusdt，see below for details |
| pageSize | optional | pagesize|
| page | optional | page number|
| api_key | essential | api_key|
| time | essential | timestamp|
| sign | essential | signature|
* return


| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |  |  as blow：|
```
    {
    "count":10,
    "resultList":[
        {
            "side":"BUY",
            "total_price":"0.10000000",
            "created_at":1510993841000,
            "avg_price":"0.10000000",
            "countCoin":"btc",
            "source":1,
            "type":1,
            "side_msg":"buy", 
            "volume":"1.000", 
            "price":"0.10000000", 
            "source_msg":"WEB", 
            "status_msg":"complete transaction", 
            "deal_volume":"1.00000000", 
            "id":424, 
            "remain_volume":"0.00000000", 
            "baseCoin":"eth", 
            "tradeList":[
                {
                    "volume":"1.000", 
                    "feeCoin":"YLB", 
                    "price":"0.10000000", 
                    "fee":"0.16431104", 
                    "ctime":1510996571195, 
                    "deal_price":"0.10000000", 
                    "id":306,
                    "type":"buy"
                } 
            ],
            "status":2
        },
        {
            "side":"SELL",
            "total_price":"0.09900000",
            "created_at":1510993715000,
            "avg_price":"0.10000000",
            "countCoin":"btc",
            "source":1,
            "type":1,
            "side_msg":"sell", 
            "volume":"1.000", 
            "price":"0.09900000", 
            "source_msg":"WEB", 
            "status_msg":"complete transaction", 
            "deal_volume":"1.00000000", 
            "id":423, 
            "remain_volume":"0.00000000", 
            "baseCoin":"eth", 
            "tradeList":[
                {
                    "volume":"1.000", 
                    "feeCoin":"YLB", 
                    "price":"0.10000000", 
                    "fee":"0.16597075", 
                    "ctime":1510993723973, 
                    "deal_price":"0.10000000", 
                    "id":261,
                    "type":"sell"
                } 
            ],
            "status":2

                                                                                                                                                                                                                                                            
        }
    ]
}
order status(status) description:
INIT(0,"initial order, not done, not entered the market"), NEW_(1,"new order, not done, entered the market"), FILLED(2,"complete transaction"), PART_FILLED(3,"partial transaction"), CANCELED(4,"withdrawn"), PENDING_CANCEL(5,"pending order"), EXPIRED(6,"abnormal order");
```
## GET /open/api/order_info   get order details
* parameter

| parameter | type | remark |
| --- | --- | --- |
| order_id | essential | order ID|
| symbol | essential | market mark，btcusdt，see below for details |
| api_key | essential | api_key|
| time | essential | timestamp|
| sign | essential | signature|
* return


| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data | {“order_info”:{},"trade_list":[]} |  as blow：|
```
{
    "order_info":{
        "id":343,
        "side":"sell",
        "side_msg":"sell", 
        "created_at":"09-22 12:22", 
        "price":222.33, 
        "volume":222.33, 
        "deal_volume":222.33, 
        "total_price":222.33,
        "fee":222.33,
        "avg_price":222.33
        }
    "trade_list":[
        {
             "id":343,
            "created_at":"09-22 12:22",
            "price":222.33,
            "volume":222.33,
            "deal_price":222.33,
            "deal_fee":222.33
        },
        {
            "id":345,
            "created_at":"09-22 12:22",
            "price":222.33,
            "volume":222.33,
            "deal_price":222.33,
            "deal_fee":222.33
        }
    ]
}
```
## GET /open/api/user/account   asset balance
* parameter

| parameter | type | remark |
| --- | --- | --- |
| api_key | essential | api_key|
| time | essential | timestamp|
| sign | essential | signature|
* return


| Numeric field | instance | remark |
| --- | --- | --- |
| code | 0 | code>0 failure |
| msg | “Suc” |  |
| data |  |  as blow：|
```
{
    "total_asset":432323.23, //total assets
    "coin_list":[ 
        {
            "coin":"btc",   
            "normal":32323.233, //balance account
            "locked":32323.233, //freezen account
            "btcValuatin":112.33//BTC valuation
        }, 
        {
            "coin":"ltc",
            "normal":32323.233,
            "locked":32323.233,
            "btcValuatin":112.33
        }, 
        {
            "coin":"bch",
            "normal":32323.233,
            "locked":32323.233,
            "btcValuatin":112.33
        },
    ]
}

```
