# 📈 Fundamental

To get data with Fundamental core API, simply connect to this graphql url

[https://dev.api.avantis.finance/playground/fundamental/graphql](https://dev.api.avantis.finance/playground/fundamental/graphql)

or you can use playground to play around with data first

[https://dev.api.avantis.finance/playground/fundamental/graphiql](https://dev.api.avantis.finance/playground/fundamental/graphiql)

## Fundamental data item

### get single fundamental data item

example

```graphql
query {
  fundamentalDataItem(id: 137) {
    fundamentalDataItemId
    dataItemName
    dataItemDescription
  }
}
```

This should get you the result

```json
{
  "data": {
    "fundamentalDataItem": {
      "fundamentalDataItemId": 137,
      "dataItemName": "Dividend Yield %",
      "dataItemDescription": "This Ratio represents Dividend Per Share DIVIDED by Share Price as on Closing Date. This is multiplied by 100. It gives the Percentage of the return (in the form of dividend) on investment (in the form of share price).\r\n\r\nFormula=(DPS/SPCL)*100\r\n\r\nDPS=Dividend Per Share\r\nSPCL=Financial Period Share Price Close"
    }
  }
}
```

### get fundamental data item by id

example

```graphql
query {
  fundamentalDataItem(id: 1752) {
    fundamentalDataItemId
    dataItemName
    dataItemDescription
  }
}
```

This should get you the result

```json
{
  "data": {
    "fundamentalDataItem": {
      "fundamentalDataItemId": 1752,
      "dataItemName": "Interest On Deposits",
      "dataItemDescription": "Excel Formula: IQ_INT_DEPOSITS\r\n\r\nInterest on Deposits [205] is a line item in the Banks template and represents the interest paid on deposits.\r\n\r\nThis item includes:\r\nInterest on customer deposits\r\nInterest on foreign deposits\r\nInterest on money market deposits\r\nInterest on time deposits in denominations of $100,000 or more\r\nInterest on demand deposits\r\nInterest-bearing transaction accounts\r\nInterest on money market and savings account\r\nInterest on NOW accounts\r\nInterest on other time deposits\r\nInterest on passbook accounts\r\nInterest on savings deposits\r\nInterest on insured money funds\r\nInterest on time CD of $100,000 and above\r\nInterest on CDs\r\nInterest on checking \r\nInterest on thrift accounts\r\nInterest on amounts due to banks\r\n\r\nThis items excludes:\r\nInterest on Borrowings\r\nInterest on Short-term FHLB debt\r\nInterest on Long-term FHLB debt\r\n"
    }
  }
}
```

### get filtered fundamental data Items

example

```graphql
query {
  fundamentalDataItems(
    input: { filter: { dataItemDescriptionLike: "Excel Formula: IQ_EBIT_INT" } }
  ) {
    fundamentalDataItemId
    dataItemName
    dataItemDescription
  }
}
```

this should get the result

```json
{
  "data": {
    "fundamentalDataItems": [
      {
        "fundamentalDataItemId": 2821,
        "dataItemName": "EBIT / Interest Expense",
        "dataItemDescription": "Excel Formula: IQ_EBIT_INT\r\n\r\nEBIT [400] / Interest Expense, Total [82]\r\n\r\nNotes:\r\n(1) If the numerator is less than or equal to zero then the ratio will be shown as NM"
      }
    ]
  }
}
```

## Period Type Name

| period type id | period type name |
| -------------- | ---------------- |
| 1              | Annual           |
| 2              | Quarterly        |
| 3              | YTD              |
| 4              | Semi             |
| 5              | Interim          |
| 6              | LTM              |

## Fundamental data

### get list of filtered of fundamental data

example

```graphql
query Fundamental {
  fundamentalData(
    input: {
      filter: {
        companyIdEq: 43874
        periodTypeNameEq: "Quarterly"
        fundamentalDataItemIdsIn: [1742]
      }
    }
  ) {
    companyId
    periodTypeId
    calendarYear
    calendarQuarter
    fundamentalDataItemId
    fiscalYear
    fiscalQuarter
    filingDate
    value
    periodType
  }
}
```

this should get the result

```json
{
  "data": {
    "fundamentalData": [
      {
        "companyId": 43874,
        "periodTypeId": 2,
        "calendarYear": 2020,
        "calendarQuarter": 4,
        "fundamentalDataItemId": 1742,
        "fiscalYear": 2021,
        "fiscalQuarter": 1,
        "filingDate": "2021-02-10",
        "value": "2177.489179",
        "periodType": "Quarterly"
      },
      {
        "companyId": 43874,
        "periodTypeId": 2,
        "calendarYear": 2020,
        "calendarQuarter": 3,
        "fundamentalDataItemId": 1742,
        "fiscalYear": 2020,
        "fiscalQuarter": 4,
        "filingDate": "2020-11-24",
        "value": "1857.656906",
        "periodType": "Quarterly"
      },
      {
        "companyId": 43874,
        "periodTypeId": 2,
        "calendarYear": 2020,
        "calendarQuarter": 2,
        "fundamentalDataItemId": 1742,
        "fiscalYear": 2020,
        "fiscalQuarter": 3,
        "filingDate": "2021-08-11",
        "value": "1319.461501",
        "periodType": "Quarterly"
      },
      {
        "companyId": 43874,
        "periodTypeId": 2,
        "calendarYear": 2020,
        "calendarQuarter": 1,
        "fundamentalDataItemId": 1742,
        "fiscalYear": 2020,
        "fiscalQuarter": 2,
        "filingDate": "2021-05-13",
        "value": "11902.096632",
        "periodType": "Quarterly"
      }
    ]
  }
}
```

## Mutual fund historical dividend

### get list of filtered of mutual fund historical dividiends

example

```graphql
query {
  fundHistoricalDividends(
    input: {
      filter: { fundIdEq: 3, startDate: "2020-11-06", endDate: "2021-11-08" }
    }
  ) {
    fundId
    dividendDate
    dividendAmount
  }
}
```

this should get the result

```json
{
  "data": {
    "fundHistoricalDividends": [
      {
        "fundId": 3,
        "dividendDate": "2020-11-06",
        "dividendAmount": "0.500000"
      },
      {
        "fundId": 3,
        "dividendDate": "2021-11-08",
        "dividendAmount": "0.750000"
      }
    ]
  }
}
```
