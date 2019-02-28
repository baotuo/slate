---
title: KEIBAY API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://www.keibay.com'>KEIBAY.COM</a>

includes:
  - errors

search: true
---

# Intruduction

最专业的古美术拍卖网站API。


# Auth

API使用的时候，需要使用KEIBAY发行的 SecretKey 作为Basic Auth的Username。

```shell
# 需要在每次请求的时候传递SecretKey作为认证信息
curl "API endpoint" \
  -u <KEIBAY发行的 SecretKey>:
```

<aside class="notice">
确认将 <code>KEIBAY发行的 SecretKey</code> 替换成KEIBAY发行的 SecretKey。
</aside>

# Auction API

## GET /api/v1/auctions

返回所有拍品

```shell
curl "https://www.keibay.com/api/v1/auctions" \
  -u <KEIBAY发行的 SecretKey>:
```

> 以上命令会返回以下的结果

```json
{
  "count": 1,
  "page": 1,
  "cur": 0,
  "limit": 20,
  "rows": [
    {
      "aid": "8kiyjsgj",
      "name": "【夢工房】玉川堂造金彩銅製花瓶　共箱　918ｇ 辰-134",
      "desc": "<p>サイズ約　19.6×20㎝</p>",
      "quantity": 1,
      "currency": "JPY",
      "startPrice": 20000,
      "maxPrice": 0,
      "price": 20000,
      "startDate": 1501723566000,
      "endDate": 1503757740000,
      "realEndDate": 1503757740000,
      "delayEnd": 1,
      "taxFlag": 0,
      "status": 0,
      "isTopBidder": false,
      "sellerUid": "obclnj6e",
      "sellerName": "yumekoubou",
      "imageArray": [
        "/files/e1/8dce656aba91d9ee73b3cea1418b50/_.jpg"
      ],
      "coverImage": "/files/e1/8dce656aba91d9ee73b3cea1418b50/_.jpg",
      "finished": false,
      "extraData": {
        "ranking": "A",
        "brand": "Ｇｕｃｃｉ",
        "category": "トートバック"
      }
    }
  ]
}
```

### HTTP Request

`GET https://www.keibay.com/api/v1/auctions`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
from | 0 | 从第n件开始请求，默认0
limit | 20 | 返回最大件数，最大为50

### Result

Key | Description
--- | -----------
count | 总件数
page | 总件数
cur | 当前页数
limit | 返回最大件数
rows | 拍品列表

## GET /api/v1/auctions/AID

返回拍品详细

```shell
curl "https://www.keibay.com/api/v1/auctions/<AID>" \
  -u <KEIBAY发行的 SecretKey>:
```

> 以上命令会返回以下的结果

```json
{
  "aid": "8kiyjsgj",
  "name": "【夢工房】玉川堂造金彩銅製花瓶　共箱　918ｇ 辰-134",
  "desc": "<p>サイズ約　19.6×20㎝</p>",
  "quantity": 1,
  "currency": "JPY",
  "startPrice": 20000,
  "maxPrice": 0,
  "price": 20000,
  "startDate": 1501723566000,
  "endDate": 1503757740000,
  "realEndDate": 1503757740000,
  "delayEnd": 1,
  "taxFlag": 0,
  "status": 0,
  "isTopBidder": false,
  "sellerUid": "obclnj6e",
  "sellerName": "yumekoubou",
  "imageArray": [
    "/files/e1/8dce656aba91d9ee73b3cea1418b50/_.jpg"
  ],
  "coverImage": "/files/e1/8dce656aba91d9ee73b3cea1418b50/_.jpg",
  "finished": false,
  "extraData": {
    "ranking": "A",
    "brand": "Ｇｕｃｃｉ",
    "category": "トートバック"
  }
}
```

### HTTP Request

`GET https://www.keibay.com/api/v1/auctions/<AID>`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
username | 无默认值 | 申请时分配的入札用户名，提供时会返回是否最高出价者的结果

### Result

Key | Description
--- | -----------
aid | AID
name | 拍品名
desc | 拍品说明
currency | 货币种类，JPY
startPrice | 起拍价
maxPrice | 即决入札价格，0为无即决入札价格
price | 当前价
startDate | 开始时间 UTC timestamp
endDate | 结束时间 UTC timestamp
realEndDate | 实际结束时间 UTC timestamp 有可能被延长
delayEnd | 是否会自动延长
taxFlag | 是否不含税 0:含税 1:不含税
status | 状态 0:拍卖中 1:已结束(无落札) 2:已落札
isTopBidder | 传入的username是否是当前最高出价者
sellerUid | 卖家UID
sellerName | 卖家名称
imageArray | 图片列表
coverImage | 封面图片

## POST /api/v1/auctions/AID/bids

入札

```shell
curl "https://www.keibay.com/api/v1/auctions/<AID>/bids" \
  -u <KEIBAY发行的 SecretKey>: \
  -d username=<入札用户名> \
  -d price=<入札价格> \
  -d delay=<是否预约入札>
```

> 以上命令会返回以下的结果

```json
{
  "result": "ok",
  "aid": "895s31z6",
  "price": 2000,
  "stepPrice": 2100,
  "bidCount": 2,
  "endDate": 1481542920000,
  "isTopBidder": false,
  "status": 0
}
```

### HTTP Request

`GET https://www.keibay.com/api/v1/auctions/<AID>/bids`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
username | 无默认值 | 申请时分配的入札用户名
price | 无默认值 | 入札价格
delay | false | true:预约入札 false:普通入札

### Result

Key | Description
--- | -----------
result | 结果，是否正常入札
aid | 出品ID
price | 当前价格
stepPrice | 下一个出价单位
bidCount | 当前入札数
endDate | 结束时间 UTC timestamp
isTopBidder | 是否当前最高出价者
status | 出品状态

### Error Message

当HTTP STATUS返回400的时候，返回结果如右

```json
{
  "code": "client",
  "message": "<ERROR MESSAGE>"
}
```

ERROR MESSAGE如下

Message | Description
--- | -----------
BID.IS_SELLER | 賣家不可以競拍
BID.AUCTION_NOT_STARTED | 本出品已經不能競拍
BID.USER_RESTRICTED | 競拍受到了限制，如有疑問，請聯系KEIBAY
BID.YAN_ABOVE_TO_BID | 請出價XXX日元
BID.YOU_ARE_TOP | 您是最高出價者
BID.OVER_LEVEL | 超過了競拍的限制。想要繼續競拍的話，請追加保證金

# Event API

## GET /api/v1/events

返回拍卖会列表

```shell
curl "https://www.keibay.com/api/v1/events" \
  -u <KEIBAY发行的 SecretKey>:
```

> 以上命令会返回以下的结果

```json
{
  "count": 1,
  "page": 1,
  "cur": 0,
  "limit": 20,
  "rows": [
    {
      "id": 8,
      "type": 0,
      "nameJa": "關西美術競賣2017年春季拍賣會",
      "nameTw": "關西美術競賣2017年春季拍賣會",
      "descJa": "關西美術競賣2017年春季拍賣會",
      "descTw": "關西美術競賣2017年春季拍賣會",
      "image": "/files/ed/15c1fcd3c015d6e521d8f7421ad191/_.jpg",
      "startDate": 1492477200000,
      "endDate": 1492477200000,
      "count": 95,
      "state": "pending"
    }
  ]
}
```

### HTTP Request

`GET https://www.keibay.com/api/v1/events`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
from | 0 | 从第n件开始请求，默认0
limit | 20 | 返回最大件数，最大为50

### Result

Key | Description
--- | -----------
count | 总件数
page | 总件数
cur | 当前页数
limit | 返回最大件数
rows | 拍品列表

## GET /api/v1/events/ID

返回单场拍卖会详细信息

```shell
curl "https://www.keibay.com/api/v1/events/<ID>" \
  -u <KEIBAY发行的 SecretKey>:
```

> 以上命令会返回以下的结果

```json
{
  "id": 8,
  "type": 0,
  "nameJa": "關西美術競賣2017年春季拍賣會",
  "nameTw": "關西美術競賣2017年春季拍賣會",
  "descJa": "關西美術競賣2017年春季拍賣會",
  "descTw": "關西美術競賣2017年春季拍賣會",
  "image": "/files/ed/15c1fcd3c015d6e521d8f7421ad191/_.jpg",
  "startDate": 1492477200000,
  "endDate": 1492477200000,
  "count": 95,
  "state": "pending"
}
```

### HTTP Request

`GET https://www.keibay.com/api/v1/events/<ID>`

### Result

Key | Description
--- | -----------
id | 拍卖会ID
type | 拍卖会类型 0:线下拍卖会 1:线上拍卖会
nameJa | 日语名称
nameTw | 繁体中文名称
descJa | 日语描述
descTw | 繁体中文描述
image | 拍卖会图片
startDate | 开始时间
endDate | 结束时间，线上拍卖会的时候，结束时间为空
count | 拍品数
state | 拍卖会状态 pending:尚未开始 started:已经开始 finished:已经结束

## GET /api/v1/events/ID/auctions

返回拍卖会拍品列表

```shell
curl "https://www.keibay.com/api/v1/events/<ID>/auctions" \
  -u <KEIBAY发行的 SecretKey>:
```

> 以上命令会返回以下的结果

```json
{
  "count": 1,
  "page": 1,
  "cur": 0,
  "limit": 20,
  "rows": [
    {
      "aid": "8kiyjsgj",
      "name": "【夢工房】玉川堂造金彩銅製花瓶　共箱　918ｇ 辰-134",
      "desc": "<p>サイズ約　19.6×20㎝</p>",
      "quantity": 1,
      "currency": "JPY",
      "startPrice": 20000,
      "maxPrice": 0,
      "price": 20000,
      "startDate": 1501723566000,
      "endDate": 1503757740000,
      "realEndDate": 1503757740000,
      "delayEnd": 1,
      "taxFlag": 0,
      "status": 0,
      "isTopBidder": false,
      "sellerUid": "obclnj6e",
      "sellerName": "yumekoubou",
      "imageArray": [
        "/files/e1/8dce656aba91d9ee73b3cea1418b50/_.jpg"
      ],
      "coverImage": "/files/e1/8dce656aba91d9ee73b3cea1418b50/_.jpg",
      "finished": false
    }
  ]
}
```

### HTTP Request

`GET https://www.keibay.com/api/v1/events/ID/auctions`

### Result

Key | Description
--- | -----------
count | 总件数
page | 总件数
cur | 当前页数
limit | 返回最大件数
rows | 拍品列表，拍品信息参考拍品API

## GET /api/v1/events/ID/room

返回现场拍信息

```shell
curl "https://www.keibay.com/api/v1/events/<ID>/room" \
  -u <KEIBAY发行的 SecretKey>:
```

> 以上命令会返回以下的结果

```json
{
  "previousLot":null,
  "currentLot":{
    "image":"https://www.keibay.com/cdn/files/8b/c131bee35379ddeaa232787a1cca4f/_.jpg",
    "images":[
      "https://www.keibay.com/cdn/files/8b/c131bee35379ddeaa232787a1cca4f/_.jpg",
      "https://www.keibay.com/cdn/files/3f/6a1d783b8eee9def29dbf1ef36202d/_.jpg",
      "https://www.keibay.com/cdn/files/35/9ef64c0c9b4550025aa624e805dc26/_.jpg",
      "https://www.keibay.com/cdn/files/a9/abd150e006eb3288fc6836694ca389/_.jpg"
    ],
    "nextBidPrice":{
      "currency":"JPY",
      "price":2000
    },
    "description":"【101101】101101",
    "priceList":[
      {
        "currency":"JPY",
        "price":2000
      },
      {
        "currency":"JPY",
        "price":3000
      },
      {
        "currency":"JPY",
        "price":4000
      },
      {
        "currency":"JPY",
        "price":5000
      },
      {
        "currency":"JPY",
        "price":6000
      },
      {
        "currency":"JPY",
        "price":7000
      },
      {
        "currency":"JPY",
        "price":8000
      },
      {
        "currency":"JPY",
        "price":9000
      },
      {
        "currency":"JPY",
        "price":10000
      },
      {
        "currency":"JPY",
        "price":11000
      },
      {
        "currency":"JPY",
        "price":12000
      },
      {
        "currency":"JPY",
        "price":13000
      },
      {
        "currency":"JPY",
        "price":14000
      },
      {
        "currency":"JPY",
        "price":15000
      },
      {
        "currency":"JPY",
        "price":16000
      },
      {
        "currency":"JPY",
        "price":17000
      },
      {
        "currency":"JPY",
        "price":18000
      },
      {
        "currency":"JPY",
        "price":19000
      },
      {
        "currency":"JPY",
        "price":20000
      },
      {
        "currency":"JPY",
        "price":22000
      },
      {
        "currency":"JPY",
        "price":24000
      },
      {
        "currency":"JPY",
        "price":26000
      },
      {
        "currency":"JPY",
        "price":28000
      },
      {
        "currency":"JPY",
        "price":30000
      },
      {
        "currency":"JPY",
        "price":32000
      },
      {
        "currency":"JPY",
        "price":34000
      },
      {
        "currency":"JPY",
        "price":36000
      },
      {
        "currency":"JPY",
        "price":38000
      },
      {
        "currency":"JPY",
        "price":40000
      },
      {
        "currency":"JPY",
        "price":42000
      }
    ],
    "price":{
      "currency":"JPY",
      "price":1000
    },
    "estimateFrom":{
      "currency":"JPY",
      "price":0
    },
    "estimateTo":{
      "currency":"JPY",
      "price":0
    },
    "startTime":1519484878000,
    "id":"8wmlixq6",
    "endTime":1519487878000,
    "topBidderId":"tn6rqdr4",
    "updatedAt":1519484886344,
    "status":1
  },
  "timestamp":1519484945479,
  "biddingList":[
    {
      "lotId":"8wmlixq6",
      "id":"0.1",
      "avatar":"https://www.keibay.com/cdn/files/d9/21fb3cbf3fba4b087cb107914f1296/_.jpeg",
      "message":"\uD83D\uDCE2【101101】開始",
      "userId":"system"
    },
    {
      "lotId":"8wmlixq6",
      "id":"14922",
      "message":"【101101】NO.11✋¥1,000",
      "userId":"tn6rqdr4",
      "paddle":"11"
    }
  ],
  "count":1
}
```

### HTTP Request

`GET https://www.keibay.com/api/v1/events/ID/room`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
since | 0 | 拍卖纪录ID开始，返回的拍卖纪录的第一条ID>since
max | 无默认值 | 拍卖纪录ID结束，当设置的时候，返回的拍卖纪录的最后一条ID<max，max主要用于，拍卖会中途进入画面，拍卖纪录往回滚的时候，取得历史信息

### Result

Key | Description
--- | -----------
previousLot | 最近结束的拍品信息
currentLot | 当前正在进行的拍品信息
currentLot.image | 当前拍品的首图
currentLot.images | 当前拍品的所有图片
currentLot.nextBidPrice | 下一口价信息
currentLot.nextBidPrice.currency | 下一口价的货币种类，现阶段全部都是JPY
currentLot.nextBidPrice.price | 下一口价
currentLot.description | 拍品说明，现在是[Lot] 商品名
currentLot.priceList | 自由出价价格列表
currentLot.price | 当前价格信息
currentLot.estimateFrom | 预估价From，为0为没有预估价
currentLot.estimateTo | 预估价To，为0为没有预估价
currentLot.startTime | 拍品开始时间。在上一件拍品结束后，通常有一段等待时间，需要通过开始时间确定，当前拍品是否已经开拍
currentLot.endTime | 拍品结束时间。再临近结束有人入札时，结束时间会延长。
currentLot.topBidderId | 当前拍品最高出价用户ID
currentLot.status | 拍品状态。0:开拍前等待状态 1:已经开拍
currentLot.id | 当前拍品AID
currentLot.updatedAt | 最终更新时间
timestamp | 服务器timestamp
biddingList | 现场拍拍卖纪录，为数组
biddingList[n].lotId | 拍品AID
biddingList[n].id | 拍卖纪录ID，升顺
biddingList[n].message | 拍卖纪录内容
biddingList[n].userId | 该拍卖纪录发起的用户ID，system为系统消息
biddingList[n].avatar | 头像图片，系统消息的时候会返回头像信息
biddingList[n].paddle | 拍卖号，入札信息的时候会返回拍卖号，画面表示不现实入札用户ID，而显示拍卖号
count | 当前现场拍参加人数
