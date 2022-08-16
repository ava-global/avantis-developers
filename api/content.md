# 🗄 Contents

An API for creating and query contents.

## Publish a content

> :warning: We recommend using an api-gateway endpoint for programatic client. Playground endpoint is for development and debugging only.

> ⚠️ Api Key is required in order to access service via an api-gateway, please request an Api Key by contacting us.

### DEV Environment Endpoints

A playground endpoint for publishing a content is [https://dev.api.avantis.finance/playground/content/publish](https://dev.api.avantis.finance/playground/content/publish)

A api-gateway endpoint for publishing a content is [https://dev.api.avantis.finance/v1/content/publish](https://dev.api.avantis.finance/v1/content/publish)

### PROD Environment Endpoints

> ℹ️ There is no playground content api endpoint in _PROD_.

An api-gateway endpoint for publishing a content is [https://api.avantis.finance/v1/content/publish](https://api.avantis.finance/v1/content/publish)

An example of a request to create a content (_POST_).

Fields explanation.

`content_type_id`

* `1` is `NEWS`
* `2` is `RESEARCH`

`source_id`

* `1` is `KS`

```json
{
    "published_when": "2021-01-01T11:11:11+07:00",
    "content_type_id": 2,
    "source_id": 1,
    "source_news_id": "T1111",
    "thumbnail_url": "https://example.com/image.png",
    "content_stocks": [
        {"symbol": "TSLA", "exchange_id": 8}
    ],
    "content_tags": [
        "VEHICLE"
    ],
    "content_details": [
        {
            "content_url": "https://example.com",
            "headline": "TEST HEADLINE2",
            "short_description": "TEST",
            "locale_iso639": "en",
            "mime_type": "text/html"
        }
    ]
}
```
## Delete a content

> :warning: We recommend using an api-gateway endpoint for programatic client. Playground endpoint is for development and debugging only.

> ⚠️ Api Key is required in order to access service via an api-gateway, please request an Api Key by contacting us.

Path parameter is `content_id`

Use `DELETE` method

### DEV Environment Endpoints

X-Auth-Request-Groups header is required. An example header.

```graphql
{
  "X-Auth-Request-Groups": 2
}
```

A playground endpoint for delete a content is [https://dev.api.avantis.finance/playground/content/{content_id}](https://dev.api.avantis.finance/playground/content/{content_id})

A api-gateway endpoint for delete a content is [https://dev.api.avantis.finance/v1/content/{content_id}](https://dev.api.avantis.finance/v1/content/{content_id})

### PROD Environment Endpoints

> ℹ️ There is no playground content api endpoint in _PROD_.

An api-gateway endpoint for delete a content is [https://api.avantis.finance/v1/content/{content_id}](https://api.avantis.finance/v1/content/{contend_id})

## Content Graphql

### DEV Environment

This is a base url for quering mentioned data via a Graphql Playground. [https://dev.api.avantis.finance/playground/content/graphiql](https://dev.api.avantis.finance/playground/content/graphiql)

An endpoint for querying data via an programatic client. [https://dev.api.avantis.finance/playground/content/graphql](https://dev.api.avantis.finance/playground/content/graphql)

_Requirement_

* A client have to set a `X-Auth-Request-Groups` HTTP header value to a comma seperated values of group ids before access the graphql endpoint. Please contact Admin to find out which is a proper `X-Auth-Request-Groups`  value for your request a Graphiql playground.

### PROD Environment

> ℹ️ There is no playground content api endpoint in _PROD_.


An api-gateway endpoint for querying data via an programatic client. [https://api.avantis.finance/v1/content/graphql](https://api.avantis.finance/v1/content/graphql)

### Filter

This Graphql query operation allows a client to get a list of contents by provided criteria, which consists of

* `sourceIdsIn` Filter by a list of `source_id`. _Optional_
* `stockIdsIn` Filter by a list of `stock_id`. _Optional_
* `contentTypeIdEq` Filter by `content_type_id`. _Optional_
* `contentTagsIn` Filter by a list of stock code. _Optional_
* `orderBy` Order by field. (Currently, support only `published_when`). _Optional_
* `orderDirection` _Optional_
* `offset`
* `limit`

If you're using a DEV Environment, X-Auth-Request-Groups header is required. An example header.

```graphql
{
  "X-Auth-Request-Groups": 2
}
```

An example query by content tags.

```graphql
query {
  contents(
    input: {
      filter: {
        sourceIdsIn: 4
        contentTagsIn: ["NEWS", "TECHNICAL_ANALYSIS"]
        limit: 2
        offset: 0
      }
    }
  ) {
    contentId
    contentDetails {
      contentId
      contentUrl
      headline
      shortDescription
      mimeType
    }
    contentSource {
      sourceId
      name
    }
    contentStocks {
      stockSymbol
    }
    contentTags {
      tag
    }
    publishedTimestampUtc
    thumbnailUrl
  }
}
```

A result.

```json
{
  "data": {
    "contents": [
      {
        "contentId": 20407,
        "contentDetails": [
          {
            "contentId": 20407,
            "contentUrl": "https://finance.yahoo.com/news/apples-airtags-tracking-capabilities-143609999.html",
            "headline": "Apple’s AirTags are being used to stalk people, here’s how to prevent that",
            "shortDescription": "Apple’s (AAPL) AirTags can serve as a great tool for people who lose things. My wife and I have attached AirTags to our house keys in case we misplace them either out in the world or, more likely, in our apartment.",
            "mimeType": "text/html"
          }
        ],
        "contentSource": {
          "sourceId": 4,
          "name": "Yahoo Finance"
        },
        "contentStocks": [
          {
            "stockSymbol": "AAPL"
          }
        ],
        "contentTags": [
          {
            "tag": "NEWS"
          }
        ],
        "publishedTimestampUtc": 1642386960,
        "thumbnailUrl": "https://media.zenfs.com/en/reuters-finance.com/e54c817036bf78a9c90c8605b33324ba"
      },
      {
        "contentId": 20408,
        "contentDetails": [
          {
            "contentId": 20408,
            "contentUrl": "https://finance.yahoo.com/news/u-senate-panel-debate-app-025638871.html",
            "headline": "U.S. Senate panel to debate app store reform bill",
            "shortDescription": "WASHINGTON (Reuters) - A U.S. Senate panel is set on Thursday to debate a bill that aims to rein in app stores of companies that some lawmakers say exert too much market control, including Apple Inc and Alphabet Inc's Google.",
            "mimeType": "text/html"
          }
        ],
        "contentSource": {
          "sourceId": 4,
          "name": "Yahoo Finance"
        },
        "contentStocks": [
          {
            "stockSymbol": "GOOG"
          },
          {
            "stockSymbol": "AAPL"
          }
        ],
        "contentTags": [
          {
            "tag": "NEWS"
          }
        ],
        "publishedTimestampUtc": 1642474560,
        "thumbnailUrl": "https://s.yimg.com/ny/api/res/1.2/efFz2dKzpxe_m0JLtYDR1Q--/YXBwaWQ9aGlnaGxhbmRlcjt3PTk2MDtoPTY0MDtjZj13ZWJw/https://s.yimg.com/uu/api/res/1.2/6_m7UlIhlT2m84D1oYaaXg--~B/aD01MzM7dz04MDA7YXBwaWQ9eXRhY2h5b24-/https://media.zenfs.com/en/reuters-finance.com/aa654fe0f14de6ad2052cbc8f8120942"
      }
    ]
  }
}
```

An example query by stock ids.

```graphql
query {
  contents(
    input: {
      filter: { sourceIdsIn: 4, stockIdsIn: [148531], limit: 2, offset: 0 }
    }
  ) {
    contentId
    contentDetails {
      contentId
      contentUrl
      headline
      shortDescription
      mimeType
    }
    contentSource {
      sourceId
      name
    }
    contentStocks {
      stockSymbol
    }
    contentTags {
      tag
    }
    publishedTimestampUtc
    thumbnailUrl
  }
}
```

A result.

```json
{
  "data": {
    "contents": [
      {
        "contentId": 20407,
        "contentDetails": [
          {
            "contentId": 20407,
            "contentUrl": "https://finance.yahoo.com/news/apples-airtags-tracking-capabilities-143609999.html",
            "headline": "Apple’s AirTags are being used to stalk people, here’s how to prevent that",
            "shortDescription": "Apple’s (AAPL) AirTags can serve as a great tool for people who lose things. My wife and I have attached AirTags to our house keys in case we misplace them either out in the world or, more likely, in our apartment.",
            "mimeType": "text/html"
          }
        ],
        "contentSource": {
          "sourceId": 4,
          "name": "Yahoo Finance"
        },
        "contentStocks": [
          {
            "stockSymbol": "AAPL"
          }
        ],
        "contentTags": [
          {
            "tag": "NEWS"
          }
        ],
        "publishedTimestampUtc": 1642386960,
        "thumbnailUrl": "https://media.zenfs.com/en/reuters-finance.com/e54c817036bf78a9c90c8605b33324ba"
      },
      {
        "contentId": 20408,
        "contentDetails": [
          {
            "contentId": 20408,
            "contentUrl": "https://finance.yahoo.com/news/u-senate-panel-debate-app-025638871.html",
            "headline": "U.S. Senate panel to debate app store reform bill",
            "shortDescription": "WASHINGTON (Reuters) - A U.S. Senate panel is set on Thursday to debate a bill that aims to rein in app stores of companies that some lawmakers say exert too much market control, including Apple Inc and Alphabet Inc's Google.",
            "mimeType": "text/html"
          }
        ],
        "contentSource": {
          "sourceId": 4,
          "name": "Yahoo Finance"
        },
        "contentStocks": [
          {
            "stockSymbol": "GOOG"
          },
          {
            "stockSymbol": "AAPL"
          }
        ],
        "contentTags": [
          {
            "tag": "NEWS"
          }
        ],
        "publishedTimestampUtc": 1642474560,
        "thumbnailUrl": "https://s.yimg.com/ny/api/res/1.2/efFz2dKzpxe_m0JLtYDR1Q--/YXBwaWQ9aGlnaGxhbmRlcjt3PTk2MDtoPTY0MDtjZj13ZWJw/https://s.yimg.com/uu/api/res/1.2/6_m7UlIhlT2m84D1oYaaXg--~B/aD01MzM7dz04MDA7YXBwaWQ9eXRhY2h5b24-/https://media.zenfs.com/en/reuters-finance.com/aa654fe0f14de6ad2052cbc8f8120942"
      }
    ]
  }
}
```

An example query by stock symbol.

```graphql
query {
  contents(
    input: {
      filter: {
        stockIdsIn: [148531]
        limit: 2
        offset: 0
        stockSymbolsIn: ["AAPL"]
      }
    }
  ) {
    contentId
    contentDetails {
      contentId
      contentUrl
      headline
      shortDescription
      mimeType
    }
    contentSource {
      sourceId
      name
    }
    contentStocks {
      stockSymbol
    }
    contentTags {
      tag
    }
    publishedTimestampUtc
    thumbnailUrl
  }
}
```

A result.

```json
{
  "data": {
    "contents": [
      {
        "contentId": 20407,
        "contentDetails": [
          {
            "contentId": 20407,
            "contentUrl": "https://finance.yahoo.com/news/apples-airtags-tracking-capabilities-143609999.html",
            "headline": "Apple’s AirTags are being used to stalk people, here’s how to prevent that",
            "shortDescription": "Apple’s (AAPL) AirTags can serve as a great tool for people who lose things. My wife and I have attached AirTags to our house keys in case we misplace them either out in the world or, more likely, in our apartment.",
            "mimeType": "text/html"
          }
        ],
        "contentSource": {
          "sourceId": 4,
          "name": "Yahoo Finance"
        },
        "contentStocks": [
          {
            "stockSymbol": "AAPL"
          }
        ],
        "contentTags": [
          {
            "tag": "NEWS"
          }
        ],
        "publishedTimestampUtc": 1642386960,
        "thumbnailUrl": "https://media.zenfs.com/en/reuters-finance.com/e54c817036bf78a9c90c8605b33324ba"
      },
      {
        "contentId": 20408,
        "contentDetails": [
          {
            "contentId": 20408,
            "contentUrl": "https://finance.yahoo.com/news/u-senate-panel-debate-app-025638871.html",
            "headline": "U.S. Senate panel to debate app store reform bill",
            "shortDescription": "WASHINGTON (Reuters) - A U.S. Senate panel is set on Thursday to debate a bill that aims to rein in app stores of companies that some lawmakers say exert too much market control, including Apple Inc and Alphabet Inc's Google.",
            "mimeType": "text/html"
          }
        ],
        "contentSource": {
          "sourceId": 4,
          "name": "Yahoo Finance"
        },
        "contentStocks": [
          {
            "stockSymbol": "GOOG"
          },
          {
            "stockSymbol": "AAPL"
          }
        ],
        "contentTags": [
          {
            "tag": "NEWS"
          }
        ],
        "publishedTimestampUtc": 1642474560,
        "thumbnailUrl": "https://s.yimg.com/ny/api/res/1.2/efFz2dKzpxe_m0JLtYDR1Q--/YXBwaWQ9aGlnaGxhbmRlcjt3PTk2MDtoPTY0MDtjZj13ZWJw/https://s.yimg.com/uu/api/res/1.2/6_m7UlIhlT2m84D1oYaaXg--~B/aD01MzM7dz04MDA7YXBwaWQ9eXRhY2h5b24-/https://media.zenfs.com/en/reuters-finance.com/aa654fe0f14de6ad2052cbc8f8120942"
      }
    ]
  }
}
```
