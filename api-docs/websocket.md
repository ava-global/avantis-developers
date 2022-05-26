---
description: >-
  Graphql websocket API will allow to subscribe to stream of data using graphql
  subscription format
---

# ⚡ Websocket

To connect with Websocket API, simply connect to this graphql url

[wss://dev.api.avantis.finance/v1/websocket/graphql](wss://dev.api.avantis.finance/v1/websocket/graphql)

or you can use playground to play around with data first

[https://dev.api.avantis.finance/playground/websocket/playground](https://dev.api.avantis.finance/playground/websocket/playground)

### Available Subscriptions

* [StockPrice](websocket.md#subscribe-to-get-real-time-stock-price)
* [Bid-Ask](websocket.md#subscribe-to-get-real-time-stock-bid-offer)

### GraphQL Schema

```graphql
type SubscriptionRoot {
  stockPrice(stockIdIn: [Int!]!): StockPrice!
  bidOffer(stockIdIn: [Int!]!): BidOffer!
}

type StockPrice {
  stockId: Int!
  executePrice: BigDecimal!
  exchangeId: Int!
  accVolume: Int!
  accValue: Int!
  highestPrice: BigDecimal!
  lowestPrice: BigDecimal!
  lastOpenPrice: BigDecimal!
  settradeDateTime: DateTime!
  volume: Int!
  value: BigDecimal!
  bidside: Boolean!
  isBid: Boolean!
  offerside: Boolean!
  lastClosePrice: BigDecimal!
  lastCloseDate: DateTime!
  rowNo: Int!
}

type BidOffer {
  stockId: Int!
  action: String!
  bids: [[String!]!]!
  offers: [[String!]!]!
  snapshotChecksum: String!
}
```

### Subscribe to get real time Stock Price

this subscription should get the stream of the results. where `stockIdIn` can be get from [profile api](../api/profile.md)



example of query

```graphql
subscription stock {
  # PTTEP: 236032
  # KBANK: 15594
  stockPrice(stockIdIn: [236032, 15594]) {
    stockId
    executePrice
    exchangeId
    accVolume
    accValue
    highestPrice
    lastOpenPrice
    settradeDateTime
    volume
    bidside
    isBid
    offerside
    lastClosePrice
    lastCloseDate
    rowNo
  }
}
```



example of the results (1)

```json
{
  "data": {
    "stockPrice": {
      "stockId": 236032,
      "executePrice": "157",
      "exchangeId": 4,
      "accVolume": 4120100,
      "accValue": 645879,
      "highestPrice": "158",
      "lastOpenPrice": "157.5",
      "settradeDateTime": "2022-05-18T11:14:53Z",
      "volume": 400,
      "bidside": true,
      "isBid": true,
      "offerside": false,
      "lastClosePrice": "157.5000",
      "lastCloseDate": "2022-05-17T00:00:00Z",
      "rowNo": 2385568
    }
  }
}
```



example of the results (2)

```json
{
  "data": {
    "stockPrice": {
      "stockId": 236032,
      "executePrice": "156.5",
      "exchangeId": 4,
      "accVolume": 4122400,
      "accValue": 646240,
      "highestPrice": "158",
      "lastOpenPrice": "157.5",
      "settradeDateTime": "2022-05-18T11:15:22Z",
      "volume": 100,
      "bidside": false,
      "isBid": false,
      "offerside": true,
      "lastClosePrice": "157.5000",
      "lastCloseDate": "2022-05-17T00:00:00Z",
      "rowNo": 2391183
    }
  }
}
```

### Subscribe to get real time stock Bid/Offer

this subscription should get the stream of the results. where `stockIdIn` can be get from [profile api](../api/profile.md)

example of query&#x20;

```graphql
subscription bidoffer {
  # PTTEP: 236032
  # KBANK: 15594
  bidOffer(stockIdIn: [15594]) {
    stockId
    action
    bids
    offers
    snapshotChecksum
  }
}
```

Result will have 2 types:&#x20;

* initial message
  * action: **S** (Snapshot)
  * contains 5 elements of bid and offer data on each side.
* update message
  * action: **I** (Insert), **D** (Delete), **U** (Update)
  * contains only updated value of bid or offer

example of result - Initial message

```graphql
{
  "data": {
    "bidOffer": {
      "stockId": 15594,
      "action": "S",
      "bids": [
        [
          "B:ATO",
          "30100"
        ],
        [
          "152.5",
          "600"
        ],
        [
          "150.5",
          "9900"
        ],
        [
          "148.5",
          "4300"
        ],
        [
          "147.5",
          "1000"
        ]
      ],
      "offers": [
        [
          "O:ATO",
          "9400"
        ],
        [
          "138.5",
          "2400"
        ],
        [
          "139",
          "19400"
        ],
        [
          "142",
          "3000"
        ],
        [
          "144",
          "9000"
        ]
      ],
      "snapshotChecksum": "3023434458"
    }
  }
}
```

example of result - Update message

```graphql
{
  "data": {
    "bidOffer": {
      "stockId": 15594,
      "action": "U",
      "bids": [
        [
          "B:ATO",
          "50100"
        ]
      ],
      "offers": [],
      "snapshotChecksum": "2263682656"
    }
  }
}
```

### checksum

Every message contains an unsigned 32-bit integer `checksum` of the bid/offer. You can run the same `checksum` on your client bid/offer state and compare it to `checksum` field. If they are the same, your client's state is correct. If not, you have likely lost or mishandled a packet and should re-subscribe to receive the initial snapshot.&#x20;

The checksum operates on a string that represents the first 10 orders on the bid/offer sorted by highest to lowest price. The format of the string is:

`{best_`_`price}|{side}:{volume},{second_price`_`}|`_`{side}:{volume},...`_

If message contains **ATO** or **ATC**, the format will be as follows:

`{side}:{ATO,ATC}|{side}:{volume},{best_`_`price}|{side}:{volume},{second_price`_`}|`_`{side}:{volume},...`_

The final checksum is the `crc32` value of this string

#### Initial message

When client received message from the websocket with following example, then calculate with the string format in the previous section. And the message also contains checksum value for validate state between client and server in next message.

```graphql
...
      "action": "S",
      "bids": [
        [
          "B:ATO",
          "30100"
        ],
        [
          "152.5",
          "600"
        ],
        [
          "150.5",
          "9900"
        ],
        [
          "148.5",
          "4300"
        ],
        [
          "147.5",
          "1000"
        ]
      ],
      "offers": [
        [
          "O:ATO",
          "9400"
        ],
        [
          "138.5",
          "2400"
        ],
        [
          "139",
          "19400"
        ],
        [
          "142",
          "3000"
        ],
        [
          "144",
          "9000"
        ]
      ],
      "snapshotChecksum": "3023434458"
...
```

The example of formatted string of initial snapshot message,

`O:ATO|O:9400,B:ATO|B:30100,152.5|B:600,150.5|B:9900,148.5|B:4300,147.5|B:1000,144|O:9000,142|O:3000,139|O:19400,138.5|O:2400`

#### Update message

```graphql
...
      "action": "U",
      "bids": [
        [
          "B:ATO",
          "50100"
        ]
      ],
...
```

After received the update message, then change current value with new value

For example, the bid side at `B:ATO` changed to `50100`, the value of `B:ATO` in the string from initial snapshot state should be `B:ATO|50100`

New string should be like this example : `O:ATO|O:9400,B:ATO|B:50100,152.5|B:600,150.5|B:9900,148.5|B:4300,147.5|B:1000,144|O:9000,142|O:3000,139|O:19400,138.5|O:2400`

And calculate checksum to validate state between client and server.



### **Authentication**

To connect with our websocket, we require the client to attach key-value of authentication header with connection initial payload.

Either `x-api-key` or `authorization` were required.

```json
{
  "type": "connection_init",
  "payload": {
    "x-api-key": "API_KEY"
  }
}
```

```json
{
  "type": "connection_init",
  "payload": {
    "authorization": "TOKEN"
  }
}
```

Or you can use graphql  client [apollo-link-ws](https://www.apollographql.com/docs/react/v2/data/subscriptions/#authentication-over-websocket) by add authentication to _connectionParams_



&#x20;

