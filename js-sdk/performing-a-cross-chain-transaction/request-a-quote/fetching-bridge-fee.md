# Fetching Bridge Fee

```javascript
const fees = await routerprotocol.getBridgeFee("DESTINATION_CHAIN_ID");
```

<details>

<summary>Response Type</summary>

```javascript
 {
        address: string, // address of the fee token
        transferFee: BigNumber, // transfer fee amount
        exchangeFee: BigNumber // exchange fee amount
    }[]
```

</details>

### Sample Request and Response

Getting all the fee token details for cross-chain transactions from Polygon to Fantom:

#### Request

```javascript
import { RouterProtocol } from "@routerprotocol/router-js-sdk"
import { ethers } from "ethers";

// initialize a RouterProtocol instance here

// getting fee details
const fees = await routerprotocol.getBridgeFee(250); // 250 is chainId for Fantom
console.log(fees)
```

#### Response

```javascript
[
  {
    address: '0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174', // USDC on Polygon
    transferFee: BigNumber { _hex: '0x16e360', _isBigNumber: true },
    exchangeFee: BigNumber { _hex: '0x16e360', _isBigNumber: true }
  },
  {
    address: '0x4c28f48448720e9000907BC2611F73022fdcE1fA', // WMATIC on Polygon
    transferFee: BigNumber { _hex: '0x14d1120d7b160000', _isBigNumber: true },
    exchangeFee: BigNumber { _hex: '0x14d1120d7b160000', _isBigNumber: true }
  },
  {
    address: '0x16ECCfDbb4eE1A85A33f3A9B21175Cd7Ae753dB4', // ROUTE on Polygon
    transferFee: BigNumber { _hex: '0x053444835ec58000', _isBigNumber: true },
    exchangeFee: BigNumber { _hex: '0x053444835ec58000', _isBigNumber: true }
  },
  {
    address: '0xC168E40227E4ebD8C1caE80F7a55a4F0e6D66C97', // DFYN on Polygon 
    transferFee: BigNumber { _hex: '0x01d7d843dc3b480000', _isBigNumber: true },
    exchangeFee: BigNumber { _hex: '0x01d7d843dc3b480000', _isBigNumber: true }
  }
]
```
