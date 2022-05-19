# 🏠 Profile

This API provides a `Company`, `Stock` and `Mutual Fund` (Currently, Only Thailand Mutual Fund data is available.) informations via Graphql query.

This is a base url for quering mentioned data via a Graphql Playground. [https://dev.api.avantis.finance/playground/profile/graphiql](https://dev.api.avantis.finance/playground/profile/graphiql)

And please use this endpoint for querying data via an programatic client. [https://dev.api.avantis.finance/playground/profile/graphql](https://dev.api.avantis.finance/playground/profile/graphql)

* [Profiles Core API](profile.md#profiles-core-api)
  * [Company](profile.md#company)
    * [Identity](profile.md#identity)
    * [Filter](profile.md#filter)
  * [Stock](profile.md#stock)
    * [Identity](profile.md#identity-1)
    * [Filter](profile.md#filter-1)
  * [Mutual Fund](profile.md#mutual-fund)
    * [Identity](profile.md#identity-2)
    * [Advance Search](profile.md#advance-search)

## Company

The following list are the available usecases that a `Company` Graphql comes up with.

* Get a company information by a `company_id`
* Retrieve a list of companies where the companies's name like a criteria.
* Retrieve a list of companies where the companies are located in.

### Identity

This Graphql query operation allows a client to get a company information by `company_id`.

An example query.

```graphql
{
  company(companyId: 46229) {
    companyId
    name
    countryId
    country {
      countryId
      name
      isoAlpha2
      isoAlpha3
    }
  }
}
```

A result.

```json
{
  "data": {
    "company": {
      "companyId": 46229,
      "name": "Apple Inc.",
      "countryId": 11,
      "country": {
        "countryId": 11,
        "name": "United States",
        "isoAlpha2": "US",
        "isoAlpha3": "USA"
      }
    }
  }
}
```

### Filter

This Graphql query operation allows a client to get a list of companies information by provided criteria, which consists of

* `filter`. _Required_
  * `nameLike` Filter companies by a part of their own name (_case-insensitive_). _Optional_
  * `countryIdEq` Filter companies by a country that they are located in. _Optional_
* `limit`. _Optional_
* `offset`. _Optional_
* `order`. _Optional_
  * `orderBy`. _Required_
  * `orderDirection`. _Required_

Example:

Request:

```graphql
{
  companies(input: {
    filter: {
      nameLike: "tesla, inc"
      countryIdEq: 11
    }
    limit: 20
    offset: 0
    order: {
      orderBy: MARKET_CAP
      orderDirection: ASCENDING
    }
  }) {
    companyId
    name
    marketCap {
      valueMillion
    }
    country {
      countryId
      name
      isoAlpha2
      isoAlpha3
    }
  }
}
```

```json
{
  "data": {
    "companies": [
      {
        "companyId": 14837,
        "name": "Tesla, Inc.",
        "marketCap": {
          "valueMillion": "1120932.019814"
        },
        "country": {
          "countryId": 11,
          "name": "United States",
          "isoAlpha2": "US",
          "isoAlpha3": "USA"
        }
      },
      {
        "companyId": 8944450,
        "name": "TESLA, Inc.",
        "marketCap": null,
        "country": {
          "countryId": 11,
          "name": "United States",
          "isoAlpha2": "US",
          "isoAlpha3": "USA"
        }
      }
    ]
  }
}
```

## Stock

The following list are the the non-exhaustive usecases that a `Stock` Graphql comes up with.

* Get a stock information by a `stock_id`
* Retrieve a list of stocks that their company name match a string in your query.
* Retrieve a list of stocks in a specified exchange.

### Identity

This Graphql query operation allows a client to get a stock information by a `stock_id`.

An example query.

```graphql
{
  stock(stockId: 148531) {
    stockId
    isin
    symbol
    normalizedSymbol
    companyId
    exchangeId
    company {
      companyId
      name
    }
    exchange {
      name
    }
  }
}
```

A result.

```json
{
  "data": {
    "stock": {
      "stockId": 148531,
      "isin": "US0378331005",
      "symbol": "AAPL",
      "normalizedSymbol": "AAPL",
      "companyId": 46229,
      "exchangeId": 8,
      "company": {
        "companyId": 46229,
        "name": "Apple Inc."
      },
      "exchange": {
        "name": "Nasdaq Global Select"
      }
    }
  }
}
```

### Filter

This Graphql query operation allows a client to get a list of stock information by provided criteria, which consists of

* `filter` _Required_
  * `isinEq` Filter by a stock isin. _Optional_
  * `symbolStartWith` Filter by an symbol prefix (_case-insensitive_). _Optional_
  * `companyIdEq` Filter by a `company_id`. _Optional_
  * `exchangeIdEq` Filter by an `exchange_id`. _Optional_
  * `companyNameLike` Filter by part of a company name (_case-insensitive_). _Optional_
  * `symbolIn` Filter by a list of stock symbol. (_case-sensitive_) _Optional_
  * `symbolLike` Filter by part of a stock name (_case-insensitive_). _Optional_
* `limit` _Optional_
* `offset` _Optional_

An example query to retrieve all stock that its own name starts with `BrK` (_case-insensitive_) and all of them are listed in NYSE.

```graphql
{
  stocks(input: { filter: { symbolStartWith: "BrK", exchangeIdEq: 1 } }) {
    stockId
    normalizedSymbol
    isin
    company {
      companyId
      name
    }
    exchange {
      name
      country {
        name
      }
    }
  }
}
```

```json
{
  "data": {
    "stocks": [
      {
        "stockId": 8001,
        "normalizedSymbol": "BRKA",
        "isin": "US0846701086",
        "company": {
          "companyId": 54890,
          "name": "Berkshire Hathaway Inc."
        },
        "exchange": {
          "name": "New York Stock Exchange",
          "country": {
            "name": "United States"
          }
        }
      },
      {
        "stockId": 218703,
        "normalizedSymbol": "BRKB",
        "isin": "US0846702076",
        "company": {
          "companyId": 54890,
          "name": "Berkshire Hathaway Inc."
        },
        "exchange": {
          "name": "New York Stock Exchange",
          "country": {
            "name": "United States"
          }
        }
      }
    ]
  }
}
```

## Mutual Fund

The following list are the the non-exhaustive usecases that a `MutualFund` Graphql comes up with.

* Get a mutual fund information by a `fund_id`
* Retrieve a list of mutual funds that has 3 years return higher than 5% and sharpe ratio is higher than zero.

### Identity

This Graphql query operation allows a client to get a mutual fund information by a `fund_id`.

An example query.

```graphql
{
  mutualFund(mutualFundId: 13) {
    fundName
    fundCode
    fundStatistics {
      return3Month
    }
  }
}
```

A result.

```json
{
  "data": {
    "mutualFund": {
      "fundName": "TMB Eastspring Dynamic Leis & Entertain",
      "fundCode": "TMB-ES-CHILL",
      "fundStatistics": {
        "return3Month": "-5.356980"
      }
    }
  }
}
```

### Advance Search

This Graphql query operation allows a client to get a list of mutual fund information by provided criteria, below is an non-exhaustive list of currently available criteria.

* `fundStatisticsReturnDay` Filter by a range of fund's daily return. _Optional_
* `fundStatisticsReturnYear` Filter by a range of fund's annually return. _Optional_
* `fundInfoRiskSpectrumNumber` Filter by a range of a risk spectrum number. _Optional_
* `limit` _Optional_
* `offset` _Optional_
* `orderBys` A list of `OrderBy` Graphql input, which consist of order direction and order by field.

An example query to retrieve all mutual fund that has

* 3 months return is greater than 2.5 percent
* 1 year sharpe ratio is greater than 0 percent

and sort a result by net expense ratio in ascending order, then take only 3 of them as a result.

```graphql
{
  mutualFunds(
    input: {
      filter: {
        fundStatisticsReturn3Month: { lowerBound: "2.5" }
        fundStatisticsSharpeRatio1Year: { lowerBound: "0" }
        orderBys: [{ orderDirection: ASCENDING, field: NET_EXPENSE_RATIO }]
        limit: 3
        offset: 0
      }
    }
  ) {
    fundName
    fundCode
    netExpenseRatio
    fundStatistics {
      return3Month
      sharpeRatio1Year
    }
  }
}
```

Result.

```json
{
  "data": {
    "mutualFunds": [
      {
        "fundName": "K Banking Sector Index",
        "fundCode": "K-BANKING",
        "netExpenseRatio": "0.340000",
        "fundStatistics": {
          "return3Month": "5.571330",
          "sharpeRatio1Year": "1.016000"
        }
      },
      {
        "fundName": "K European Equity Index",
        "fundCode": "K-EUX",
        "netExpenseRatio": "0.350000",
        "fundStatistics": {
          "return3Month": "3.009460",
          "sharpeRatio1Year": "1.583000"
        }
      },
      {
        "fundName": "Bangkok Metropolitan",
        "fundCode": "BMBF",
        "netExpenseRatio": "0.423600",
        "fundStatistics": {
          "return3Month": "2.909900",
          "sharpeRatio1Year": "0.880000"
        }
      }
    ]
  }
}
```
