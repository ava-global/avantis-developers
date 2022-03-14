# Prices

To get data with Price core API, simply connect to this graphql url

[https://dev.api.avantis.finance/playground/price/graphql](https://dev.api.avantis.finance/playground/price/graphql)

or you can use playground to play around with data first

[https://dev.api.avantis.finance/playground/price/graphiql](https://dev.api.avantis.finance/playground/price/graphiql)

## Realtime Price

In Realtime Price APIs you can
- get latest realtime price
### Get latest realtime price

example

```graphql
query {
  {
    price{
      latest(criteria:{exchangeIdIn:[4], limit: 10}){
        stockId
        exchangedId
        latestPrice
        accumulatedValue
        accumulatedVolume
        
      }
    }
  }
}
```

this will get result

```json
{
  "data": {
    "price": {
      "latest": [
        {
          "stockId": 120628,
          "exchangedId": 4,
          "latestPrice": "195.000000",
          "accumulatedValue": "39.000000",
          "accumulatedVolume": "200.000000"
        },
        {
          "stockId": 36843,
          "exchangedId": 4,
          "latestPrice": "1265.000000",
          "accumulatedValue": "126.000000",
          "accumulatedVolume": "100.000000"
        },
        {
          "stockId": 1387,
          "exchangedId": 4,
          "latestPrice": "28.750000",
          "accumulatedValue": "12.000000",
          "accumulatedVolume": "400.000000"
        },
        {
          "stockId": 208082,
          "exchangedId": 4,
          "latestPrice": "9.100000",
          "accumulatedValue": "1.000000",
          "accumulatedVolume": "100.000000"
        },
        {
          "stockId": 230288,
          "exchangedId": 4,
          "latestPrice": "9.550000",
          "accumulatedValue": "3.000000",
          "accumulatedVolume": "300.000000"
        },
        {
          "stockId": 40198,
          "exchangedId": 4,
          "latestPrice": "13.600000",
          "accumulatedValue": "15.000000",
          "accumulatedVolume": "1100.000000"
        },
        {
          "stockId": 19587,
          "exchangedId": 4,
          "latestPrice": "46.000000",
          "accumulatedValue": "58.000000",
          "accumulatedVolume": "1200.000000"
        },
        {
          "stockId": 178870,
          "exchangedId": 4,
          "latestPrice": "8.000000",
          "accumulatedValue": "7.000000",
          "accumulatedVolume": "900.000000"
        },
        {
          "stockId": 10505,
          "exchangedId": 4,
          "latestPrice": "29.750000",
          "accumulatedValue": "21.000000",
          "accumulatedVolume": "700.000000"
        },
        {
          "stockId": 194767,
          "exchangedId": 4,
          "latestPrice": "63.750000",
          "accumulatedValue": "38.000000",
          "accumulatedVolume": "600.000000"
        }
      ]
    }
  }
}
```

## Eod Price

In EodPrice APIs you can

- get single eod price
- get filtered eod price
- get latest eod price
- get aggregated eod price
- get last close price

### Get single eod price

- you can use field `identity` provided with 2 arguments
  - `stockId`
  - `timestamp` with format "YYYY-mm-dd"

example

```graphql
query {
  eodPrice {
    identity(stockId: 7, timestamp: "2020-02-03") {
      stockId
      timestamp
      exchangeId
      openPrice
      highPrice
      lowPrice
      closePrice
      volumeTrade
      valueTrade
    }
  }
}
```

this should get you the result

```json
{
  "data": {
    "eodPrice": {
      "identity": {
        "stockId": 7,
        "timestamp": "2020-02-03 00:00:00",
        "exchangeId": 6,
        "openPrice": "13.309000",
        "highPrice": "13.249840",
        "lowPrice": "12.989970",
        "closePrice": "12.989970",
        "volumeTrade": "3339.000000",
        "valueTrade": "43765.608600"
      }
    }
  }
}
```

### Get filtered end of the day Price

- you can use field `filter` provided with 1 schema arguments
  - `criteria`
    - mandatory field
      - `limit`
    - optional
      - `stockIdEq`
      - `exchangeIdIn`
      - `startDate` with format "YYYY-mm-dd"
      - `endDate` with format "YYYY-mm-dd"
      - `dateEq` with format "YYYY-mm-dd"
      - `orderBy`
        - `VALUE`
        - `VOLUME`
        - `CHANGE_PERCENTAGE`
        - `TIMESTAMP`
      - `orderDirection`:
        - `ASCENDING`
        - `DESCENDING`
- you can sort by using `orderBy` and its direction wtih `orderDirection`

example

```graphql
query {
  eodPrice {
    filter(
      criteria: {
        stockIdEq: 7
        limit: 2
        orderBy: VALUE
        orderDirection: ASCENDING
      }
    ) {
      stockId
      timestamp
      exchangeId
      openPrice
      highPrice
      lowPrice
      closePrice
      volumeTrade
      valueTrade
    }
  }
}
```

this should get you the result

```json
{
  "data": {
    "eodPrice": {
      "filter": [
        {
          "stockId": 7,
          "timestamp": "2020-10-27 00:00:00",
          "exchangeId": 6,
          "openPrice": "14.075000",
          "highPrice": "14.149460",
          "lowPrice": "13.959740",
          "closePrice": "14.149460",
          "volumeTrade": "225.000000",
          "valueTrade": "3158.392500"
        },
        {
          "stockId": 7,
          "timestamp": "2020-02-13 00:00:00",
          "exchangeId": 6,
          "openPrice": "12.764000",
          "highPrice": "12.950700",
          "lowPrice": "12.829990",
          "closePrice": "12.829990",
          "volumeTrade": "272.000000",
          "valueTrade": "3522.481600"
        }
      ]
    }
  }
}
```

### Get latest of eod price

example

```graphql
query {
  eodPrice {
    latest(criteria: { stockIdEq: 10, exchangeIdIn: [9], limit: 2 }) {
      stockId
      timestamp
      exchangeId
      openPrice
      highPrice
      lowPrice
      closePrice
      volumeTrade
      valueTrade
    }
  }
}
```

this will get result

```json
{
  "data": {
    "eodPrice": {
      "latest": [
        {
          "stockId": 10,
          "timestamp": "2021-10-25 00:00:00",
          "exchangeId": 9,
          "openPrice": "31.010000",
          "highPrice": "31.060000",
          "lowPrice": "30.700000",
          "closePrice": "30.860000",
          "volumeTrade": "398344.000000",
          "valueTrade": "12301261.064000"
        }
      ]
    }
  }
}
```

### Get aggregated eod price

Aggregation type:

- YTD
- WEEK
- MONTH
- QUARTER
- YEAR

example

```graphql
query {
  eodPrice {
    aggregate(
      criteria: {
        aggregationType: MONTH
        stockId: 7
        startDate: "2019-10-20"
        endDate: "2020-10-20"
      }
    ) {
      aggregationValue
      aggregationType
      year
      aggregatedStartDate
      openPrice
      highPrice
      lowPrice
      closePrice
      volumeTrade
      valueTrade
    }
  }
}
```

this will get result

```json
{
  "data": {
    "eodPrice": {
      "aggregate": [
        {
          "aggregationValue": 2,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-02-01",
          "openPrice": "13.309000",
          "highPrice": "13.639800",
          "lowPrice": "9.310150",
          "closePrice": "10.209940",
          "volumeTrade": "111170.000000",
          "valueTrade": "1342275.910850"
        },
        {
          "aggregationValue": 3,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-03-01",
          "openPrice": "10.130000",
          "highPrice": "11.000000",
          "lowPrice": "6.830350",
          "closePrice": "9.281500",
          "volumeTrade": "287795.000000",
          "valueTrade": "2634558.343070"
        },
        {
          "aggregationValue": 4,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-04-01",
          "openPrice": null,
          "highPrice": "11.989930",
          "lowPrice": "8.700220",
          "closePrice": "11.410760",
          "volumeTrade": "238165.000000",
          "valueTrade": "2511726.698390"
        },
        {
          "aggregationValue": 5,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-05-01",
          "openPrice": "11.229000",
          "highPrice": "13.819950",
          "lowPrice": "10.240110",
          "closePrice": "13.819950",
          "volumeTrade": "322117.000000",
          "valueTrade": "4014162.306700"
        },
        {
          "aggregationValue": 6,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-06-01",
          "openPrice": "13.770000",
          "highPrice": "14.610010",
          "lowPrice": "11.479660",
          "closePrice": "13.329970",
          "volumeTrade": "229405.000000",
          "valueTrade": "2974758.677200"
        },
        {
          "aggregationValue": 7,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-07-01",
          "openPrice": null,
          "highPrice": "19.318910",
          "lowPrice": "12.870300",
          "closePrice": "17.761470",
          "volumeTrade": "393518.000000",
          "valueTrade": "6680296.016500"
        },
        {
          "aggregationValue": 8,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-08-01",
          "openPrice": null,
          "highPrice": "19.148780",
          "lowPrice": "14.409460",
          "closePrice": "15.870360",
          "volumeTrade": "218605.000000",
          "valueTrade": "3653464.807200"
        },
        {
          "aggregationValue": 9,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-09-01",
          "openPrice": "16.225000",
          "highPrice": "16.620000",
          "lowPrice": "12.509320",
          "closePrice": "12.760690",
          "volumeTrade": "266081.000000",
          "valueTrade": "3941587.438700"
        },
        {
          "aggregationValue": 10,
          "aggregationType": "MONTH",
          "year": 2020,
          "aggregatedStartDate": "2020-10-01",
          "openPrice": "12.635000",
          "highPrice": "14.380070",
          "lowPrice": "12.580290",
          "closePrice": "14.200070",
          "volumeTrade": "138946.000000",
          "valueTrade": "1863952.124000"
        }
      ]
    }
  }
}
```

### Get last close price

example

```graphql
query {
  eodPrice {
    lastClosePrice(criteria: { stockIdIn: [7] }) {
      exchangeId
      stockId
      yesterdayClosePrice
      lastWeekClosePrice
      lastMonthClosePrice
      lastYearClosePrice
      yearToDateClosePrice
    }
  }
}
```

this will get result

```json
{
  "data": {
    "eodPrice": {
      "lastClosePrice": [
        {
          "exchangeId": 6,
          "stockId": 7,
          "yesterdayClosePrice": "15.350000",
          "lastWeekClosePrice": "15.350000",
          "lastMonthClosePrice": "15.350000",
          "lastYearClosePrice": "17.214000",
          "yearToDateClosePrice": "18.149980"
        }
      ]
    }
  }
}
```

## Exchange id you can query

| exhange id | exchange name                  |
| ---------- | ------------------------------ |
| 1          | New York Stock Exchange        |
| 2          | Nasdaq Capital Market          |
| 3          | Indonesia Stock Exchange       |
| 4          | The Stock Exchange of Thailand |
| 5          | Singapore Exchange             |
| 6          | London Stock Exchange          |
| 7          | NYSE MKT LLC                   |
| 8          | Nasdaq Global Select           |
| 9          | The Toronto Stock Exchange     |
| 10         | Deutsche Boerse AG             |
| 11         | Nasdaq Global Market           |
| 12         | Ho Chi Minh Stock Exchange     |
| 13         | Australian Securities Exchange |
| 14         | The Tokyo Stock Exchange       |
| 15         | XETRA Trading Platform         |

## Historical NAV

In Historical NAV APIs you can

- get single NAV
- get filtered NAV

### Get single NAV

example

```graphql
query {
  fundNav {
    identity(fundId: 7, navDate: "2021-11-10") {
      fundId
      navDate
      navPrice
      navChange
      navChangePercentage
    }
  }
}
```

this will get result

```json
{
  "data": {
    "fundNav": {
      "identity": {
        "fundId": 7,
        "navDate": "2021-11-10",
        "navPrice": "12.439400",
        "navChange": "-0.073300",
        "navChangePercentage": "-0.585805"
      }
    }
  }
}
```

### Get Filtered NAV

example

```grahpql
query {
  fundNav {
    filter(
      criteria: {
        fundIdEqIn: [7]
        startNavDate: "2020-10-20"
        endNavDate: "2020-10-20"
        limit: 3
      }
    ) {
      fundId
      navDate
      navPrice
      navChange
      navChangePercentage
    }
  }
}
```

this will get result

```json
{
  "data": {
    "fundNav": {
      "filter": [
        {
          "fundId": 7,
          "navDate": "2021-09-13",
          "navPrice": "12.382300",
          "navChange": "-0.002600",
          "navChangePercentage": "-0.020993"
        },
        {
          "fundId": 7,
          "navDate": "2021-11-10",
          "navPrice": "12.439400",
          "navChange": "-0.073300",
          "navChangePercentage": "-0.585805"
        },
        {
          "fundId": 7,
          "navDate": "2021-11-12",
          "navPrice": "12.521500",
          "navChange": "0.082100",
          "navChangePercentage": "0.660000"
        }
      ]
    }
  }
}
```

