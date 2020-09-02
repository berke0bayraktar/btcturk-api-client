# btcturk-api-client
A promise based javascript client (wrapper) for [BtcTurk API](https://github.com/BTCTrader/broker-api-docs/blob/master/README-pro.md)

## Installation

```
npm install btcturk-api-client
```

## Features
#### Non-auth Features
- Get ticker information
- Get order book data
- Get trade data
- Get daily OHLC data

#### Authentication Required Features

- Get account balance
- Get user transactions
- Get open/all orders
- Submit orders (market, limit, market stop, market limit)
- Cancel orders

## Add to your project 

#### Import for public endpoints
```js
const btcTurkClient = require('btcturk-api-client')
client = btcTurkClient()
```
#### Import for private (auth required) endpoints
```js
const btcTurkClient = require('btcturk-api-client')
client = btcTurkClient(API_KEY, API_SECRET)
```
## Usage
### Non-auth Methods
- #### getPair(pair)
  Gets the exchange information for specified pair. Omit the parameter to get all pairs on the exchange.
  ```js
  client.getPair('BTC-TRY')
  ```
  **Result:**
  ```js
  [
    {
      pair: 'BTCTRY',
      pairNormalized: 'BTC_TRY',
      timestamp: 1598957965982,
      last: 74589,
      high: 79344,
      low: 74589,
      bid: 74589,
      ask: 110000,
      open: 78344,
      volume: 8.14405237,
      average: 77356.202415,
      daily: 31656,
      dailyPercent: -4.79,
      denominatorSymbol: 'TRY',
      numeratorSymbol: 'BTC',
      order: 0
    }
  ]
  ```
- #### getOrderBook(pair, count)
  Retrieves best `count` bids and asks for given pair. Default and maximum for `count` are 10 and 109.
  
  ```js
  client.getOrderBook('BTC-TRY', 3)
  ```
  **Result:**
  ```js
  {
    timestamp: 1598960997324,

    bids: [
        [ '75000.00', '0.05000000' ],
        [ '74589.00', '0.09047336' ],
        [ '74563.00', '0.19428700' ]],

    asks: [
        [ '80154.00', '0.20000000' ],
        [ '80951.00', '0.03000000' ],
        [ '81213.00', '0.04500000' ]]
  }
  ```

- #### getTrades(pair)
  Retrieves the last 30 trades for specifed pair.
  
  ```js
    client.getTrades('BTC-TRY')
  ```
  
  **Result:**
  ```js
  [
    {
      pair: 'BTCTRY',
      pairNormalized: 'BTC_TRY',
      numerator: 'BTC',
      denominator: 'TRY',
      date: 1598890169250,
      tid: '637065432892507020',
      price: '74589.00',
      amount: '0.03470021',
      side: 'buy'
    },
    ...
  ]
  ```
- #### getOHLC(pair, count) 
  Retrieves daily OHLC data of specified pair for the last `count` days. Default for `count` is 10.
  ```js
  client.getOHLC('BTC-TRY', 1)
  ```
  **Result:**
  ```js
  [
    {
      pairNormalized: 'BTC_TRY',
      pairSymbol: 'BTCTRY',
      time: 1598918400000,
      open: '86227',
      high: '88290',
      low: '85500',
      close: '87679',
      volume: '337.6679218335722285',
      average: '87398.56',
      dailyChangeAmount: '1452',
      dailyChangePercentage: '1.68'
    }
  ]
  ```
### Authentication Required Methods
- #### getAccountBalance()
  *Note:* API provides the balance information for an asset even though it is zero.
  
  ```js
  client.getAccountBalance()
  ```
  **Result:** 
  ```js
  [
    {
      asset: 'TRY',
      assetname: 'Türk Lirası',
      balance: '226679.6909093929915539',
      locked: '0',
      free: '226679.6909093929915539',
      orderFund: '0',
      requestFund: '0',
      precision: 2
    },
    {
      asset: 'ETH',
      assetname: 'Ethereum',
      balance: '58.8145217391210377',
      locked: '0',
      free: '58.8145217391210377',
      orderFund: '0',
      requestFund: '0',
      precision: 4
    },
    {
      asset: 'XRP',
      assetname: 'Ripple',
      balance: '0',
      locked: '0',
      free: '0',
      orderFund: '0',
      requestFund: '0',
      precision: 2
    },
    ...
  ]
  ```

- #### getTransactions()
  Gets every user transaction (an order may be filled with multiple transactions).
  
  ```js
  client.getTransactions()
  ```
  **Result:** 
  ```js
  [
    {
        price: '2052.00',
        numeratorSymbol: 'ETH',
        denominatorSymbol: 'TRY',
        orderType: 'sell',
        orderId: 16460186,
        id: 5674109,
        timestamp: 1598976179173,
        amount: '-0.42073315',
        fee: '-1.32',
        tax: '-0.24'
     },
    ...
  ]
  ```

- #### getOpenOrders(pair)
  Fetches the open orders for the specified pair.
  
  ```js
  client.getOpenOrders('BTC-TRY')
  ```
  **Result:** 
  ```js
  {
    asks: [
      {
        id: 16460176,
        price: '80154.00',
        amount: '0.20000000',
        quantity: '0.20000000',
        stopPrice: '0.00',
        pairSymbol: 'BTCTRY',
        pairSymbolNormalized: 'BTC_TRY',
        type: 'sell',
        method: 'limit',
        orderClientId: '069180f1-0e92-8952-8820-d24rf584de5b',
        time: 0,
        updateTime: 1598960986610,
        status: 'Untouched',
        leftAmount: '0.20000000'
      }
    ],
    bids: [
      {
        id: 16460181,
        price: '70000.00',
        amount: '0.23000000',
        quantity: '0.23000000',
        stopPrice: '0.00',
        pairSymbol: 'BTCTRY',
        pairSymbolNormalized: 'BTC_TRY',
        type: 'buy',
        method: 'limit',
        orderClientId: 'basic',
        time: 0,
        updateTime: 1598963566783,
        status: 'Untouched',
        leftAmount: '0.23000000'
      }
    ]
  }
  ```
- #### getAllOrders(pair)
  Fetches all orders for the specified pair.

  ```js
  client.getAllOrders('BTC-TRY')
  ```
  
 ### Submitting Orders
 - #### submitMarketOrder(pair, orderType, quantity)
     Places a market order for specified pair.  
     
     Parameters:
     * `orderType` : `'buy'`, `'sell'`
     * `quantity` : quantity of the trading asset as decimal  
   <br>
   
   ```js
      client.submitMarketOrder('BTC-TRY', 'sell', 0.1)  
   ```  
    **Result:** 
    ```js
    {
      id: 16460182,
      quantity: '0.1',
      price: '0.00',
      stopPrice: null,
      newOrderClientId: '9ff84g5f-6dc8-4ff1-9ae4-4b7b22bc2f58',
      type: 'sell',
      method: 'market',
      pairSymbol: 'BTCTRY',
      pairSymbolNormalized: 'BTC_TRY',
      datetime: 1598964394187
    }
    ```
 - #### submitLimitOrder(pair, orderType, price, quantity)
     Places a limit order for specified pair.  
     
     Parameters:
     * `orderType` : `'buy'`, `'sell'`
     * `price`: limit price for the trade
     * `quantity` : quantity of the trading asset as decimal  
   <br>
   
   ```js
      client.submitLimitOrder('BTC-TRY', 'buy', 45300, 0.53)  
   ```  
    **Result:** 
    ```js
    {
      id: 16460183,
      quantity: '0.53',
      price: '45300.00',
      stopPrice: null,
      newOrderClientId: '9d46h19a-87eb-49a2-b2d7-9c25tym8daa9',
      type: 'buy',
      method: 'limit',
      pairSymbol: 'BTCTRY',
      pairSymbolNormalized: 'BTC_TRY',
      datetime: 1598970149812
    }
    ```
 - #### submitStopMarketOrder(pair, orderType, stopPrice, quantity)
     Places a stop market order for specified pair.  
     
     Parameters:
     * `orderType` : `'buy'`, `'sell'`
     * `stopPrice`: price that will trigger the market order (decimal)
     * `quantity` : quantity of the trading asset (decimal)  
   <br>
   
   ```js
      client.submitStopMarketOrder('BTC-TRY', 'sell', 40000, 0.8)  
   ```  
    **Result:** 
    ```js
    {
      id: 16460184,
      quantity: '0.8',
      price: '40000.00',
      stopPrice: '40000',
      newOrderClientId: 'b7dc8c7a-c578-45bf-b7c8-cc36fab416f6',
      type: 'sell',
      method: 'stopmarket',
      pairSymbol: 'BTCTRY',
      pairSymbolNormalized: 'BTC_TRY',
      datetime: 1598970638664
    }
    ```
 - #### submitStopLimitOrder(pair, orderType, stopPrice, limitPrice, quantity)
     Places a stop limit order for specified pair.  
     
     Parameters:
     * `orderType` : `'buy'`, `'sell'`
     * `stopPrice`: price that will trigger the limit order as decimal
     * `limitPrice`: price for the limit order that will be placed once `stopPrice` is reached
     * `quantity` : quantity of the trading asset as decimal  
   <br>
   
   ```js
      client.submitStopLimitOrder('BTC-TRY', 'sell', 48000, 45000, 0.76)  
   ```  
    **Result:** 
    ```js
    {
      id: 16460185,
      quantity: '0.76',
      price: '45000.00',
      stopPrice: '48000',
      newOrderClientId: '0dac8291-ado1-473a-bb1e-fd80c85cc09e',
      type: 'sell',
      method: 'stoplimit',
      pairSymbol: 'BTCTRY',
      pairSymbolNormalized: 'BTC_TRY',
      datetime: 1598971555847
    }
    ```
 - #### cancelOrder(orderId)
      Cancels the order with id `orderId`.  
      
      Parameters:
      * `orderId` : id of the order to be canceled as string (specified on the response object after every order submission)  
    <br>
    
    ```js
       client.cancelOrder('1621992847')  
    ```  
