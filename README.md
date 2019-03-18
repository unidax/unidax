# UniDax REST行情交易接口
# 基本信息
* 本篇列出REST接口的baseurl https://api.unidax.com
* 所有接口的响应都是JSON格式 

# 通用规则
## 签名
请求参数按照字典排序，然后以keyvalue的形式拼接成字符串string，最后sign=MD5(string+secretKey)。注意：如果请求参数中value为NULL的
情况，则在拼接字符串时不计入签名字符串。
例如：
参数如下：
```
{
'symbol': 'ltcbtc',
'api_key': '0816016bb06417f50327e2b557d39aaa',
'sign': 'a169740d0588141ef70b71cf11ff8bf3',
'time': '1522055680'
}
```
拼接完成后：
string=api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680
sign = MD5(string+secretKey) = MD5(api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680xxxxxxxxxxxxxxxxx)
最终：sign=48105baf432a6626051a72fb160b617e

* post请求参数采用表单格式提交数据
  content-type:application/x-www-form-urlencoded
  
## 错误码

| 错误码 | 说明 | 备注 |
| --- | --- | --- |
| 0  |成功|   |    
| 5 | 下单失败 |  |
| 6 | 数量小于最小值 |  |
| 7 | 数量大于最大值 |  |
| 8 | 订单取消失败 |  |
| 9 | 交易被冻结 |  |
| 13 | 对不起，程序出现系统错误，请和网站管理员联系 |  |
| 19 | 可用余额不足 |  |
| 22 | 订单不存在 |  |
| 23 | 缺少交易数量参数 |  |
| 24 | 缺少交易价格参数 |  |
| 100001 | 系统异常 |  |
| 100002 | 系统升级 |  |
| 100004 | 请求参数不合法 |  |
| 100005 | 参数签名错误 |  |
| 100007 | 非法IP |  |
| 110002 | 未知的货币代号 |  |
| 110003 | 资金密码错误 |  |
| 110004 | 提现被冻结 |  |
| 110005 | 可用余额不足 |  |
| 110020 | 用户名不存在 |  |
| 110023 | 手机号已注册 |  |
| 110024 | 邮箱已注册 |  |
| 110025 | 帐户被后台管理员锁定 |  |
| 110032 | 该用户无权限进行此操作 |  |
| 110033 | 充值失败 |  |
| 110034 | 提现失败 |  |

# 接口信息
## 行情

| 接口 | 描述 |
| --- | --- |
| /open/api/get_records | 获取K线数据 |
| /open/api/get_ticker | 成交记录 |
| /open/api/get_trades | 获取行情成交记录 |
| /open/api/market_dept | 查询买卖盘深度 |

## 交易

| 接口 | 描述 |
| --- | --- |
| /open/api/all_order | 获取全部委托 |
| /open/api/all_trade | 获取全部成交记录 |
| /open/api/cancel_order | 取消委托单 |
| /open/api/common/symbols | 查询系统支持的所有交易对及精度 |
| /open/api/create_order | 创建订单 |
| /open/api/market | 获取各个币对的最新成交价 |
| /open/api/new_order | 获取当前委托 |
| /open/api/order_info | 获取订单详情 |
| /open/api/user/account | 资产余额 |

# 接口明细
## GET /open/api/get_records 获取K线数据
该接口不进行签名校验
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| symbol | 必填 | 市场标记，bchbtc，详情看下面 |
| period | 必填 | 单位为分钟，比如1分钟则为1，一天则为1440 |
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  如下：|
```
[
    [
      1514445780, //时间戳 
      1.12, //开盘价 
      1.12, //最高 
      1.12, //最低 
      1.12, //收盘价 
      0 //成交量
    ],
    [
      1514445780, //时间戳 
      1.12, //开盘价 
      1.12, //最高 
      1.12, //最低 
      1.12, //收盘价 
      0 //成交量
    ],
    [
      1514445780, //时间戳 
      1.12, //开盘价 
      1.12, //最高 
      1.12, //最低 
      1.12, //收盘价 
      0 //成交量
    ]
]
```
## GET /open/api/get_ticker 获取当前行情
* 参数


| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| symbol | 必填 | 市场标记，btcusdt，详情看下面 |

* 返回

| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  如下：|
```
{
    "high": 1,//最高值
    "vol": 10232.26315789,//交易量 "last": 173.60263169,//最新成交价 "low": 0.01,//最低值
    "buy": "0.01000000",//买一价
    "sell": "1.12345680",//卖一价 "time": 1514448473626
}

```
## GET /open/api/get_trades 获取行情成交记录
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| symbol | 必填 | 市场标记，btcusdt，详情看下面 |

* 返回

| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  如下：|
```
[
    {
        "amount": 0.55,//成交量
        "price": 0.18519949,//成交价
        "id": 447121,
        "type": "buy"//买卖type，买为buy，买sell
    },
    {
        "amount": 16.45,
        "price": 0.18335468,
        "id": 447120,
        "type": "buy"
    }
]
```

## GET /open/api/market_dept 查询买卖盘深度
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| symbol | 必填 | 市场标记，btcusdt，详情看下面 |
| type | 必填 | 深度类型，step0, step1, step2(合并深度0-2);step0时，精度最高 |
* 返回

| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  如下：|
```
{
    "tick":{
            "asks":[//卖盘
                        {22112.22,0.9332}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2},
                    ],
            "bids":[//买盘
                        {22112.22,0.9332}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2}, 
                        {22112.21,0.2},
                    ]
            }
}
```
## GET /open/api/all_order 获取全部委托
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| symbol | 必填 | 市场标记，btcusdt，详情看下面 |
| pageSize | 选填 | |
| page | 选填 | |
| api_key | 必填 |api_key |
| time | 必填 | 时间戳|
| sign | 必填 | 签名|
* 返回

| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  如下：|
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
            "side_msg":"买入", 
            "volume":"1.000", 
            "price":"0.10000000",
            "source_msg":"WEB", 
            "status_msg":"完全成交", 
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
                    "type":"买入"
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
            "side_msg":"卖出", 
            "volume":"1.000", 
            "price":"0.09900000", 
            "source_msg":"WEB", 
            "status_msg":"完全成交", 
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
                    "type":"卖出"
                } 
            ],
            "status":2
         } 
    ]
}
```
## GET /open/api/all_trade 获取全部成交记录
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| symbol | 必填 | 市场标记，btcusdt，详情看下面 |
| pageSize | 选填 | |
| page | 选填 | |
| api_key | 必填 |api_key |
| time | 必填 | 时间戳|
| sort | 选填 | 1表示倒序|
| sign | 必填 | 签名|
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  如下：|
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
            "type":"买入", 
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
            "type":"买入", 
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
            "type":"买入", 
            "bid_user_id":10001, 
            "ask_user_id":10001
        }
    ]
}
```
## GET /open/api/cancel_order 取消委托单
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| order_id | 必填 | 订单ID |
| symbol | 必填 | 市场标记，btcusdt，详情看下面 |
| api_key | 必填 |api_key |
| time | 必填 | 时间戳|
| sign | 必填 | 签名|
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  |
## GET /open/api/common/symbols 查询系统支持的所有交易对及精度
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| 无 | | |
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |   |  如下：|
```
{
    "code": "0",
    "msg": "suc",
    "data": [
        {
            "symbol": "ethbtc", //交易对
            "count_coin": "btc", //计价货币
            "amount_precision": 3, //数量精度位数(0为个位)
            "base_coin": "eth", //基础币种
            "price_precision": 8 //价格精度位数(0为个位)
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
## POST /open/api/create_order 创建订单
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| side | 必填 | 买卖方向BUY、SELL |
| type | 必填 | 挂单类型，1:限价委托、2:市价委托 |
| volume | 必填 |购买数量(多义，复用字段) type=1:表示买卖数量 type=2:买则表示总价格，卖表示总个数 |
| price | 选填 | 委托单价:type=2:不需要此参数|
| fee_is_user_exchange_coin | 选填 | 0，当交易所有平台币时，此参数表示是否使用用平台币支付手续费，0否，1是|
| api_key | 必填 | api_key|
| time | 必填 | 时间戳|
| sign | 必填 | 签名|
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |  {"order_id":34343} |  成功返回交易ID|
## GET /open/api/market 获取各个币对的最新成交价
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| api_key | 必填 | api_key|
| time | 必填 | 时间戳|
| sign | 必填 | 签名|
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |  {"btcusdt":15000,"ethusdt":800}|  |
## GET /open/api/new_order 获取当前委托
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| symbol | 必填 | 市场标记，btcusdt，详情看下面|
| pageSize | 选填 | 页面大小|
| page | 选填 | 页码|
| api_key | 必填 | api_key|
| time | 必填 | 时间戳|
| sign | 必填 | 签名|
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data |  |  如下：|
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
            "side_msg":"买入", 
            "volume":"1.000", 
            "price":"0.10000000", 
            "source_msg":"WEB", 
            "status_msg":"完全成交", 
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
                    "type":"买入"
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
            "side_msg":"卖出", 
            "volume":"1.000", 
            "price":"0.09900000", 
            "source_msg":"WEB", 
            "status_msg":"完全成交", 
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
                    "type":"卖出"
                } 
            ],
            "status":2

                                                                                                                                                                                                                                                            
        }
    ]
}
订单状态(status)说明:
INIT(0,"初始订单，未成交未进入盘口"), NEW_(1,"新订单，未成交进入盘口"), FILLED(2,"完全成交"), PART_FILLED(3,"部分成交"), CANCELED(4,"已撤单"), PENDING_CANCEL(5,"待撤单"), EXPIRED(6,"异常订单");
```
## GET /open/api/order_info 获取订单详情
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| order_id | 必填 | 订单ID|
| symbol | 必填 | 市场标记，btcusdt，详情看下面|
| api_key | 必填 | api_key|
| time | 必填 | 时间戳|
| sign | 必填 | 签名|
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data | {“order_info”:{},"trade_list":[]} |  如下：|
```
{
    "order_info":{
        "id":343,
        "side":"sell",
        "side_msg":"卖出", 
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
## GET /open/api/user/account 资产余额
* 参数

| 参数 | 填写类型 | 说明 |
| --- | --- | --- |
| api_key | 必填 | api_key|
| time | 必填 | 时间戳|
| sign | 必填 | 签名|
* 返回


| 字段 | 实例 | 说明 |
| --- | --- | --- |
| code | 0 | code>0失败 |
| msg | “Suc” |  |
| data | |  如下：|
```
{
    "total_asset":432323.23, //总资产
    "coin_list":[ 
        {
            "coin":"btc",   
            "normal":32323.233, //余额账户
            "locked":32323.233, //冻结账户
            "btcValuatin":112.33//BTC估值
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
