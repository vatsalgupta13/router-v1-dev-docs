# Get Fee Details

You can also fetch the details of all the supported fee tokens between any two chains by querying the following endpoint:

{% swagger method="get" path="/fee" baseUrl="https://api.stats.routerprotocol.com/api" summary="Fetching fee details between the specified chains" %}
{% swagger-description %}
Get details of all the supported fee tokens between the specified source and destination chain
{% endswagger-description %}

{% swagger-parameter in="query" name="srcChainId" required="true" type="String/Int" %}
Network ID of the source chain (eg: 

**`137`**

 for Polygon, 

**`56`**

 for BNB Chain).
{% endswagger-parameter %}

{% swagger-parameter in="query" name="destChainId" type="String/Int" required="true" %}
Network ID of the destination chain (eg: 

**`1`**

 for Ethereum, 

**`42161`**

 for Arbitrum)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="The HTTP request was successful" %}
```javascript
// Sample response (fee tokens for transactions from Polygon to Fantom):

[
  {
    token: '0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174', // USDC on Polygon (6 decimal places)
    transferFee: '1500000', // decimal-adjusted value, do ethers.utils.formatUnits(transferFee, decimals) to get the formatted value 
    exchangeFee: '1500000', // decimal-adjusted value, do ethers.utils.formatUnits(exchangeFee, decimals) to get the format
    isAccepted: true
  },
  {
    token: '0x4c28f48448720e9000907BC2611F73022fdcE1fA', // Wrapped MATIC on Polygon (18 decimal places)
    transferFee: '1500000000000000000',
    exchangeFee: '1500000000000000000',
    isAccepted: true
  },
  {
    token: '0x16ECCfDbb4eE1A85A33f3A9B21175Cd7Ae753dB4', // ROUTE on Polygon (18 decimal places)
    transferFee: '375000000000000000',
    exchangeFee: '375000000000000000',
    isAccepted: true
  },
  {
    token: '0xC168E40227E4ebD8C1caE80F7a55a4F0e6D66C97', // DFYN on Polygon (18 decimal places) 
    transferFee: '34000000000000000000',
    exchangeFee: '34000000000000000000',
    isAccepted: true
  }
]

```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="One or more required parameters missing or invalid" %}
```javascript
// If either or both of the parameters are not passed or are invalid, the API will return the following message along with the error code:
"srcChainId and destChainId required"
```
{% endswagger-response %}
{% endswagger %}

### Code Snippet

```javascript
import axios from "axios"

const STATS_API_URL = "https://api.stats.routerprotocol.com/api"

// calling the pathfinder api using axios
const fetchFeeData = async (params) => {
    const endpoint = "fee"
    const pathUrl = `${STATS_API_URL}/${endpoint}`
    console.log(pathUrl)
    try {
        const res = await axios.get(pathUrl, { params })
        return res.data
    } catch (e) {
        console.error(`Fetching data from API: ${e}`)
    }
}

const main = async () => {
    const args = {
        'srcChainId': 137, // Polygon
        'destChainId': 250, // Fantom
        'widgetId': 24 // '24' can be used for testing. Contact us on Telegram to get a widget ID.
    }
    
    const fee_response = await fetchFeeData(args)
    console.log(fee_response)
}

main()
```
