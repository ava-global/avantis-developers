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
* Bid-Ask(TBD)

### GraphQL Schema

```graphql
type SubscriptionRoot {
  stockPrice(stockIdIn: [Int!]!): StockPrice!
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

