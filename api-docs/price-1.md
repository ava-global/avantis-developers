# 💰 Asset

To get data with Asset core API, simply connect to this graphql url

[https://dev.api.avantis.finance/playground/asset/graphiql](https://dev.api.avantis.finance/playground/asset/graphiql)

or you can use playground to play around with data first

[https://dev.api.avantis.finance/playground/asset/graphiql](https://dev.api.avantis.finance/playground/asset/graphiql)

## All Active Asset

### get all active asset

example

```graphql
query {
  allActiveAssets {
    assetId
    avantisStockId
    saxoUic
    symbol
    assetTitle
    iconImageUrl
    contractAddress
    status
  }
}
```

this will get result

```json
{
  "data": {
    "allActiveAssets": [
      {
        "assetId": 1,
        "avantisStockId": 4754,
        "saxoUic": 1111,
        "symbol": "aMSFT",
        "assetTitle": "Microsoft Corp",
        "iconImageUrl": null,
        "contractAddress": "0x3F9f79E6FbBa0A16Bbb8cb18DF27d502AAa88974",
        "status": "ACTIVE"
      },
      {
        "assetId": 2,
        "avantisStockId": 519,
        "saxoUic": 2222,
        "symbol": "aAMZN",
        "assetTitle": "Amazon Inc",
        "iconImageUrl": null,
        "contractAddress": "0x06bc59803b1f2559A512362Ff1E9F0F445146B70",
        "status": "ACTIVE"
      }
    ]
  }
}
```
