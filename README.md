# 📘 5paisa Xtream Open APIs Documentation Part3

---

## Table of Contents

### 5. 💰 Margin  
- [Margin](#margin)  


### 6. 📊 Market Data  
- [MarketSnapshot](#marketsnapshot)
- [MarketFeed](#marketfeed)  
- [MarketDepth](#marketdepth)
  
---

# Margin

## Overview of MarginV4 API

The `MarginV4` API provides detailed margin information for a specified client code <user> . It retrieves equity and mutual fund margin details by querying the backend database and returning structured financial data. This API is essential for stock trading platforms needing to check real-time margin status for users.

---

## Endpoint

```
POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V4/Margin
```

---

## Authentication

- Requires Bearer Token authorization.
- Include the token in the `Authorization` header as:  
  `Authorization: Bearer <JWT_Token>`
- Cookie header may be required with valid session cookie:  
  `Cookie: 5paisacookie=<session_token>`

---

## Request Headers

| Header           | Description                 | Required |
|------------------|-----------------------------|----------|
| Content-Type     | Must be `application/json`  | Yes      |
| Authorization    | Bearer JWT token            | Yes      |
| Cookie           | Session cookie              | Optional |

---

## Request Body

```json
{
  "head": {
    "key": "dVOtp6A6mRzrBO6SdAs7Vf"
  },
  "body": {
    "ClientCode": "50"
  }
}
```

| Parameter  | Type   | Description                           | Required |
|------------|--------|-------------------------------------|----------|
| head.key   | string | API access key                      | Yes      |
| body.ClientCode | string | Unique client identifier for margin data | Yes      |

---

## cURL Example

```bash
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V4/Margin' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...' \
--header 'Cookie: 5paisacookie=kmfrozkfxvest' \
--data '{
    "head": {
        "key": "dfgh34567fgh
    },
    "body": {
        "ClientCode": "456789"
    }
}'
```

---

## Response Body

```json
{
  "head": {
    "responseCode": "5PMarginV4",
    "status": 0,
    "statusDescription": "Success"
  },
  "body": {
    "ClientCode": "50",
    "Status": 0,
    "Message": "",
    "EquityMargin": [
      {
        "AdhocMargin": 0,
        "CollateralValueAfterHairCut": 0,
        "NetAvailableMargin": 0,
        "MarginUtilized": 0,
        "TotalCollateralValue": 0,
        "DerivativeMargin": 0,
        "Ledgerbalance": 0,
        "DPFreeStockValue": 0,
        "GrossHoldingValue": 0,
        "GrossHoldingValueCoverPercentage": 0,
        "HairCut": 0,
        "MarginBlockedforOpenPostion_Cash": 0,
        "MarginBlockedforOpenPostion_Collateral": 0,
        "MarginBlockedforPendingOrder_Cash": 0,
        "MarginBlockedforPendingOrder_Collateral": 0,
        "MFCollateralValueAfterHaircut": 0,
        "OptionsPremium": 0,
        "FundsWithdrawal": 0,
        "MarginBlockedForPendingOrders": 0,
        "FundsPayln": 0,
        "TodaysLoss": 0,
        "Unsettled_Credits": 0
      }
    ],
    "MFMargin": [
      {
        "MFCollateralValue": 0,
        "MFFreeStockValue": 0,
        "MFHaircutValue": 0
      }
    ],
    "TimeStamp": "2025-05-19T12:00:00"
  }
}
```

---

## Response Fields Description

| Field                             | Type     | Description                                 |
|----------------------------------|----------|---------------------------------------------|
| head.responseCode                | string   | Internal API request code                    |
| head.status                      | int      | Status code (0 = Success, others indicate error) |
| head.statusDescription           | string   | Status message                              |
| body.ClientCode                 | string   | Client code reflected back                  |
| body.Status                     | int      | Business logic status (0 = Success)          |
| body.Message                    | string   | Error or success message                     |
| body.EquityMargin               | array    | Array of equity margin details               |
| body.EquityMargin[].AdhocMargin | decimal  | Adhoc margin amount                           |
| body.EquityMargin[].CollateralValueAfterHairCut | decimal | Collateral value after haircut               |
| body.EquityMargin[].NetAvailableMargin | decimal | Net margin available                          |
| ...                            | ...      | Other margin fields as shown above           |
| body.MFMargin                  | array    | Array of mutual fund margin details          |
| body.MFMargin[].MFCollateralValue | decimal | Mutual fund collateral value                   |
| body.MFMargin[].MFFreeStockValue | decimal  | Mutual fund free stock value                   |
| body.MFMargin[].MFHaircutValue   | decimal  | Mutual fund haircut value                      |
| body.TimeStamp                 | datetime | Timestamp of the response                     |

---

## Error Handling

| Status Code | Meaning                  | Description                    |
|-------------|--------------------------|--------------------------------|
| 9           | Invalid Session          | Authentication failed or token expired |
| 2           | Input Validation Error   | Missing or invalid ClientCode or head key |
| 1           | Data Not Found           | No margin info available for the ClientCode |

---


## Additional Notes

- The API supports multi-layer validation: header key, authorization JWT, and client session cookies.
- The margin data returned is consolidated from multiple tables and stored procedures.
- This version (V4) enhances margin details compared to earlier versions, including derivatives and mutual fund margins.
- Always ensure your client code is validated to avoid unauthorized data access.

---

# MarketSnapshot

## Overview of MarketSnapshotV1 API 

This API provides a comprehensive full market snapshot for one or multiple financial instruments (scrips). It delivers essential real-time and historical data such as Last Traded Price (LTP), High, Low, Previous Close, Open Interest, 52-week High/Low, Volume, and other key metrics. This detailed market data is crucial for algorithmic trading systems, enabling informed decision-making and strategic automation.

---

## Endpoint

```

POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/v1/MarketSnapshot

````

---

## Description

The Market Snapshot API fetches a full market snapshot for the requested scrip(s). It returns detailed market data required to analyze the current and historical state of each instrument, supporting trading strategies with timely and precise information.

- Supports multiple scrips in a single request (up to 50).
- Covers various exchanges and exchange types.
- Useful for live monitoring and backtesting trading algorithms.
- Provides snapshot fields such as LTP, High, Low, Previous Close, Open Interest, 52-week High/Low, Volume, Upper and Lower Circuit Limits, etc.

---

## Request Structure

### Headers

| Header         | Description                              |
|----------------|------------------------------------------|
| Authorization  | Bearer token for API authentication     |
| Content-Type   | application/json                         |

### Body Parameters

| Parameter          | Type              | Required | Description                                                                                  |
|--------------------|-------------------|----------|----------------------------------------------------------------------------------------------|
| ClientCode         | string            | Yes      | Unique client identifier for authentication and request validation.                          |
| Data               | array of objects  | Yes      | List of scrip details to fetch snapshots for.                                               |

### Each Data Object contains:

| Field          | Type    | Required | Description                                                      |
|----------------|---------|----------|------------------------------------------------------------------|
| Exchange       | string  | Yes      | Exchange code (e.g., N for NSE, B for BSE).                                  |
| ExchangeType   | string  | Yes      | Exchange segment/type (e.g., 'C' for Cash, 'F' for Futures).     |
| ScripCode      | integer | Conditionally | Numeric code for the scrip. If missing, ScripData must be used.  |
| ScripData      | string  | Conditionally | Symbol or name of the scrip. Used to resolve ScripCode internally.|


---
## 📘 Exchange & Segment Codes

### Exchange (`Exch`)

| Code | Exchange |
| ---- | -------- |
| N    | NSE      |
| B    | BSE      |
| M    | MCX      |
| X    | NCDEX    |

---

### Segment (`ExchType`)

| Code | Segment Description             |
| ---- | ------------------------------- |
| C    | Cash Segment (Equity)           |
| D    | Derivatives (Futures & Options) |
| U    | Currency Derivatives            |
| Y    | NSE/BSE Commodities             |
| X    | NCDEX Commodities               |

---
## Response Structure

### General Fields

| Field           | Type    | Description                           |
|-----------------|---------|-------------------------------------|
| Status          | integer | API execution status code (0=Success, others=Error) |
| Message         | string  | Human-readable status message       |
| Data            | array   | List of market snapshot objects     |

### Each Data Object contains:

| Field               | Type    | Description                                         |
|---------------------|---------|-----------------------------------------------------|
| Exchange            | string  | Exchange code                                      |
| ExchangeType        | string  | Exchange segment/type                              |
| ScripCode           | integer | Numeric scrip code                                |
| LastTradedPrice     | decimal | Last traded price (LTP)                            |
| High                | decimal | Highest price during the session                    |
| Low                 | decimal | Lowest price during the session                     |
| PreviousClose       | decimal | Previous trading day's closing price                 |
| OpenInterest        | integer | Open interest at the time of snapshot                |
| Volume              | integer | Total traded volume for the session                   |
| FiftyTwoWeekHigh    | decimal | 52-week highest price                               |
| FiftyTwoWeekLow     | decimal | 52-week lowest price                                |
| UpperCircuitLimit   | decimal | Upper circuit price limit                            |
| LowerCircuitLimit   | decimal | Lower circuit price limit                            |
| PrevOpenInterest    | integer | Previous day open interest (if applicable)           |

---

## Usage Notes

- The API validates the request for maximum 50 scrips per call to ensure performance and stability.
- ScripCode or ScripData must be provided per scrip object; if ScripCode is missing, the API internally resolves it using ScripData.
- The exchange and exchange type must be specified accurately for correct data retrieval.
- The API requires a valid authorization token.
- The response includes status codes and messages to handle common issues such as invalid client codes, scrip limits exceeded, or data retrieval errors.

---

## Sample Request Body

```json
{
  "body": {
    "ClientCode": "your_client_code",
    "Data": [
      {
        "Exchange": "N",
        "ExchangeType": "C",
        "ScripCode": 12345
      },
      {
        "Exchange": "B",
        "ExchangeType": "C",
        "ScripData": "RELIANCE"
      }
    ]
  }
}
````

---

## Sample Response

```json
{
  "head": {
    "status": 0,
    "statusDescription": "Success"
  },
  "body": {
    "Status": 0,
    "Message": "Success",
    "Data": [
      {
        "Exchange": "NSE",
        "ExchangeType": "C",
        "ScripCode": 12345,
        "LastTradedPrice": 3456.78,
        "High": 3500.00,
        "Low": 3400.00,
        "PreviousClose": 3420.50,
        "OpenInterest": 15000,
        "Volume": 200000,
        "FiftyTwoWeekHigh": 4000.00,
        "FiftyTwoWeekLow": 2800.00,
        "UpperCircuitLimit": 3700.00,
        "LowerCircuitLimit": 3200.00,
        "PrevOpenInterest": 14500
      },
      {
        "Exchange": "BSE",
        "ExchangeType": "C",
        "ScripCode": 54321,
        "LastTradedPrice": 2540.10,
        "High": 2600.00,
        "Low": 2500.00,
        "PreviousClose": 2525.00,
        "OpenInterest": 0,
        "Volume": 100000,
        "FiftyTwoWeekHigh": 2700.00,
        "FiftyTwoWeekLow": 1900.00,
        "UpperCircuitLimit": 2650.00,
        "LowerCircuitLimit": 2400.00,
        "PrevOpenInterest": 0
      }
    ]
  }
}
```

---

## Integration Tips for Algo Trading

* Use this API to periodically poll market conditions of your watchlist or portfolio scrips.
* Integrate snapshot data into your trading algorithms for dynamic position sizing, stop loss adjustment, or signal generation.
* Leverage volume and open interest data to assess liquidity and market sentiment.
* Combine with historical data APIs for enhanced trend analysis.
* Monitor circuit limits and 52-week highs/lows to avoid trading outside permissible price bands.

---

## Summary of Market Snapshot API

The Market Snapshot API is an essential building block for any automated trading system targeting Indian markets. It provides timely, rich data across multiple instruments with efficient batch support and secure authorization. Use it to empower your algo trading strategies with real-time market intelligence.

---
# MarketFeed

## Overview of MarketFeedV1 API

This API fetches real-time market feed data for specified financial instruments (scrips). The response includes key market details such as:

- Last Traded Price (LTP)  
- High and Low prices of the day  
- Previous Close price  
- Volume and other market metrics  

The API supports batch requests for multiple scrips and delivers data suited for trading algorithms, decision-making, and market analysis.

---

## Endpoint

```

POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V1/MarketFeed

````

---

## Request Format

The request consists of:

- **Header:** Contains general metadata (authentication, client info, etc.)  
- **Body:** Includes a list of scrips with their exchange details, last request timestamp, and refresh rate.

### JSON Structure

```json
{
  "head": {
    // Generic request header fields
  },
  "body": {
    "MarketFeedData": [
      {
        "Exch": "string",       // Exchange identifier (e.g.,N for NSE, B for BSE)
        "ExchType": "string",   // Market segment/type (e.g., C for equity, D for futures)
        "ScripCode": int,       // Unique identifier for the scrip
        "ScripData": "string"   // Optional scrip symbol or additional data
      }
      // Can include multiple scrip objects, max 50
    ],
    "LastRequestTime": "datetime",  // Timestamp of the last request (for incremental updates)
    "RefreshRate": "string"          // Desired refresh interval (optional)
  }
}
````

---
## 📘 Exchange & Segment Codes

### Exchange (`Exch`)

| Code | Exchange |
| ---- | -------- |
| N    | NSE      |
| B    | BSE      |
| M    | MCX      |
| X    | NCDEX    |

---

### Segment (`ExchType`)

| Code | Segment Description             |
| ---- | ------------------------------- |
| C    | Cash Segment (Equity)           |
| D    | Derivatives (Futures & Options) |
| U    | Currency Derivatives            |
| Y    | NSE/BSE Commodities             |
| X    | NCDEX Commodities               |

---

## Response Format

The response contains:

* **Status:** Execution status code (0 = success, others indicate errors)
* **Message:** Status message or error details
* **CacheTime:** Cache validity duration in seconds
* **TimeStamp:** Server timestamp for the data snapshot
* **Data:** List of market feed data objects, one per requested scrip

### Sample Response Structure (JSON)

```json
{
  "head": {
    "responseCode": "string",
    "status": int,
    "statusDescription": "string"
  },
  "body": {
    "Status": int,
    "Message": "string",
    "CacheTime": int,
    "TimeStamp": "datetime",
    "Data": [
      {
        "Exch": "char",
        "ExchType": "char",
        "Token": uint,
        "LastRate": double,
        "TotalQty": long,
        "High": double,
        "Low": double,
        "PClose": double,
        "Chg": double,
        "ChgPcnt": double,
        "TickDt": "datetime",
        "Symbol": "string"
      }
      // Multiple scrip data objects
    ]
  }
}
```

---

## Key Points & Usage Notes

* **Scrip Limit:** The maximum number of scrips per request is 50. Exceeding this will return an error status.
* **Incremental Updates:** Using the `LastRequestTime` field helps in fetching only updated market data since the last call, optimizing bandwidth and latency.
* **Market Open/Close Status:** The API internally detects market status and adjusts cache timing accordingly.
* **Error Handling:** Status codes and messages help identify issues such as invalid scrip codes or feed server problems.
* **Data Accuracy:** Calculated fields like price change and percentage change are included for quick algorithmic assessments.

---

## Classes Overview

| Class Name                | Description                            |
| ------------------------- | -------------------------------------- |
| `MarketFeedV1Req`         | Request wrapper with header & body     |
| `MarketFeedV1ReqBody`     | Contains list of scrips and timestamps |
| `MarketFeedV1DataListReq` | Individual scrip request details       |
| `MarketFeedRes`           | API response wrapper                   |
| `MarketFeedResBody`       | Response body with status & data       |
| `MarketFeedDataListRes`   | Market data per scrip                  |

---
### 📦 Response Parameters

#### Head Object

| Field               | Type    | Description                       |
| ------------------- | ------- | --------------------------------- |
| `status`            | Integer | 0 for success, others for errors  |
| `statusDescription` | String  | Status message                    |
| `responseCode`      | String  | Code representing the API version |

#### Body Object

| Field       | Type             | Description                               |
| ----------- | ---------------- | ----------------------------------------- |
| `Status`    | Integer          | 0 for success, others for specific errors |
| `Message`   | String           | Describes the response status             |
| `TimeStamp` | DateTime         | Response timestamp                        |
| `CacheTime` | Integer (ms)     | Suggested cache time in milliseconds      |
| `Data`      | Array of Objects | List of market feed records               |

#### MarketFeedData Object

| Field       | Type     | Description                  |
| ----------- | -------- | ---------------------------- |
| `Exch`      | String   | Exchange                     |
| `ExchType`  | String   | Exchange segment/type        |
| `ScripCode` | Int      | Code of the scrip            |
| `LTP`       | Decimal  | Last traded price            |
| `High`      | Decimal  | Highest price of the session |
| `Low`       | Decimal  | Lowest price of the session  |
| `PClose`    | Decimal  | Previous day close           |
| `Time`      | DateTime | Time when data was fetched   |


---


## Conclusion of Market Feed API

This Market Feed API provides reliable, real-time market data essential for algo trading systems, enabling dynamic decision-making and analysis. The API design supports bulk queries with incremental updates and clear status reporting, making it ideal for integration into automated trading assistants or research platforms.


---
# MarketDepth

## Overview of MarketDepthV3 API

**MarketDepthV3** API provides Level 5 market depth data for one or more stock instruments simultaneously. It returns detailed order book information including price levels, quantities, and number of orders on both the bid and ask sides for the requested scripts.

This API is designed for high-performance algo trading systems to analyze market liquidity and depth efficiently across multiple scripts.

---

## Endpoint

```
POST https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V3/MarketDepth
```

---

## Request Structure

* The API accepts a list of scripts identified by exchange, exchange type, and script code or symbol.
* Supports bulk requests, allowing retrieval of market depth for multiple instruments in one call.

---

## 🧾 Request Body Object

```json
{
  "head": {
    "key": "<YourAppKey>"
  },
  "body": {
    "MarketDepthData": [
      {
        "Exchange": "B",
        "ExchangeType": "C",
        "ScripCode": 0,
        "ScripData": "Reliance_EQ"
      },
      {
        "Exchange": "N",
        "ExchangeType": "C",
        "ScripCode": 11915,
        "ScripData": ""
      }
    ]
  }
}
```

---

## 🧪 Sample cURL Request

```bash
curl --location 'https://Openapi.5paisa.com/VendorsAPI/Service1.svc/V3/MarketDepth' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer <YourAccessToken>' \
--header 'Cookie: 5paisacookie=<YourCookie>' \
--data '{
  "head": {
    "key": "<YourAppKey>"
  },
  "body": {
    "MarketDepthData": [
      {
        "Exchange": "B",
        "ExchangeType": "C",
        "ScripCode": 0,
        "ScripData": "Reliance_EQ"
      },
      {
        "Exchange": "N",
        "ExchangeType": "C",
        "ScripCode": 11915,
        "ScripData": ""
      }
    ]
  }
}'
```

---

## Response Structure

The response contains the following:

### Header

* **responseCode**: API response identifier
* **status**: Numeric status code (0 = Success, 1 = Failure, etc.)
* **statusDescription**: Textual description of the response status

### Body

* **Status**: Status of the market depth data fetch (0 = Success, non-zero indicates error)
* **Message**: Status message or error details
* **MarketDepthData**: List of market depth details per script, each containing:

  * **Exch**: Exchange identifier
  * **ExchType**: Exchange type
  * **ScripCode**: Script identifier code
  * **TimeStamp**: Timestamp of the market depth snapshot
  * **Bids**: Array of bid-level data (up to 5 entries), each with:

    * Price
    * Quantity
    * NumberOfOrders
  * **Asks**: Array of ask-level data (up to 5 entries), each with:

    * Price
    * Quantity
    * NumberOfOrders

---

## Sample Response (Simplified JSON)

```json
{
  "head": {
    "responseCode": "5PMDV3",
    "status": 0,
    "statusDescription": "Success"
  },
  "body": {
    "Status": 0,
    "Message": "Success",
    "MarketDepthData": [
      {
        "Exch": "N",
        "ExchType": "C",
        "ScripCode": 12345,
        "TimeStamp": "2025-05-19T10:15:30Z",
        "Bids": [
          {"Price": 100.5, "Quantity": 200, "NumberOfOrders": 10},
          {"Price": 100.4, "Quantity": 150, "NumberOfOrders": 8}
        ],
        "Asks": [
          {"Price": 100.7, "Quantity": 100, "NumberOfOrders": 6},
          {"Price": 100.8, "Quantity": 120, "NumberOfOrders": 7}
        ]
      }
    ]
  }
}
```

---

## Key Notes

* If a requested script does not have data, the API returns zeroed bid and ask arrays with 5 empty entries each.
* Ensure all script symbols or codes follow the allowed alphanumeric and underscore format.
* The API efficiently aggregates and returns bulk market depth data, reducing the need for multiple API calls.

---

## Usage Tips

* Validate input scripts carefully to avoid invalid characters.
* Use the API to feed market depth data into AI/ML/DL models for enhanced trading strategies.
* The bulk nature of the API helps minimize latency in real-time algo trading systems.

---

## 🧾 Market Depth Request Script Object

| Field          | Type    | Description                                          |
| -------------- | ------- | ---------------------------------------------------- |
| `Exchange`     | String  | Exchange code (e.g., N or NSE, B for BSE)            |
| `ExchangeType` | String  | Exchange segment/type (e.g., C, D)                   |
| `ScripCode`    | Integer | Unique scrip identifier                              |
| `ScripData`    | String  | Script symbol (used if `ScripCode` is not available) |

---

## 📘 Exchange & Segment Codes

### Exchange (`Exch`)

| Code | Exchange |
| ---- | -------- |
| N    | NSE      |
| B    | BSE      |
| M    | MCX      |

---

### Segment (`ExchType`)

| Code | Segment Description             |
| ---- | ------------------------------- |
| C    | Cash Segment (Equity)           |
| D    | Derivatives (Futures & Options) |
| U    | Currency Derivatives            |
| Y    | NSE/BSE Commodities             |

---

## 🧾 Response Head Object

| Field               | Type    | Description                           |
| ------------------- | ------- | ------------------------------------- |
| `responseCode`      | String  | Unique response code (e.g., `5PMDV3`) |
| `status`            | Integer | Status of the response (0 = success)  |
| `statusDescription` | String  | Description of the response status    |

---

## 🧾 Response Body Object

| Field             | Type             | Description                                      |
| ----------------- | ---------------- | ------------------------------------------------ |
| `Status`          | Integer          | Status of the API result (0 = success, 1 = fail) |
| `Message`         | String           | Message about the result                         |
| `MarketDepthData` | Array of Objects | Contains depth data for each requested scrip     |

---

## 🧾 Market Depth Script Response Object

| Field       | Type     | Description                        |
| ----------- | -------- | ---------------------------------- |
| `Exch`      | String   | Exchange code                      |
| `ExchType`  | String   | Exchange segment/type              |
| `ScripCode` | Integer  | Script code for the instrument     |
| `TimeStamp` | DateTime | Timestamp of market depth snapshot |
| `Bids`      | Array    | Array of bid side depth data       |
| `Asks`      | Array    | Array of ask side depth data       |

---

## 🧾 Market Depth Data Object (Bid/Ask Level)

| Field            | Type    | Description                          |
| ---------------- | ------- | ------------------------------------ |
| `Price`          | Decimal | Price at this depth level            |
| `Quantity`       | Integer | Quantity available at this level     |
| `NumberOfOrders` | Integer | Number of orders at this price level |

---

---
