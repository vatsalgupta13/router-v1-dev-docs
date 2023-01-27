# 1⃣ Request a Quote

{% swagger method="get" path="/quote" baseUrl="https://api.pathfinder.routerprotocol.com/api" summary="Requesting a quote from the Pathfinder algorithm" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="fromTokenAddress" type="String" required="true" %}
Token address of the asset you wish to transfer from the source chain.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="toTokenAddress" type="String" required="true" %}
Token address of the asset you wish to receive on the destination chain.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="amount" required="true" type="String" %}
Decimal-adjusted amount of the token to be transferred (for eg: if you want to transfer 1 USDC, you need to send 

**`1000000`**

). You can use 

**`ethers.utils.parseUnits`**

 function to calculate the decimal-adjusted amount of the source token.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="fromTokenChainId" type="String/Int" required="true" %}
Network ID of the source chain (eg: 

**`1`**

 for Ethereum, 

**`56`**

 for BNB Chain).
{% endswagger-parameter %}

{% swagger-parameter in="query" name="toTokenChainId" type="String/Int" required="true" %}
Network ID of the destination chain (eg: 

**`137`**

 for Polygon, 

**`250`**

 for Fantom).
{% endswagger-parameter %}

{% swagger-parameter in="query" name="userAddress" type="String" required="true" %}
Wallet address initiating the transaction.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="slippageTolerance" type="String/Float" %}
Percentage change in price at the time of confirmation and the actual price that you are willing to accept. By default, this is set to 

**`1.5`**

.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="feeTokenAddress" type="String" %}
Address of the token to use to pay the bridge fees. By default, this is set to the source chain's native gas asset (eg: AVAX on Avalanche).
{% endswagger-parameter %}

{% swagger-parameter in="query" name="destReceiverAddress" type="String" %}
Address of the user on the destination chain. By default, this is set the same as 

**`userAddress`**

. Required only if you want to transfer the funds to a different address on the destination chain.
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="The HTTP request was successful" %}
```javascript
// Sample response (for transferring 1 ROUTE token from Polygon to Fantom):

{
    "isWrappedToken": false,
    "source": {
        "asset": {
            "decimals": 6,
            "symbol": "USDC",
            "name": "USDC",
            "chainId": 137,
            "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
            "resourceID": "0x00000000000000000000002791bca1f2de4661ed88a30c99a7a9449aa8417400",
            "isMintable": false,
            "isWrappedAsset": false,
            "tokenInstance": {
                "decimals": 6,
                "symbol": "USDC",
                "name": "USDC",
                "chainId": 137,
                "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174"
            }
        },
        "stableReserveAsset": {
            "decimals": 6,
            "symbol": "USDC",
            "name": "USDC",
            "chainId": 137,
            "address": "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
            "resourceID": "0x00000000000000000000002791bca1f2de4661ed88a30c99a7a9449aa8417400",
            "isMintable": false,
            "isWrappedAsset": false
        },
        "tokenAmount": "10000000",
        "stableReserveAmount": "10000000",
        "path": [],
        "flags": [
            "17716742144"
        ],
        "priceImpact": "0",
        "bridgeFee": 0,
        "tokenPath": ""
    },
    "destination": {
        "asset": {
            "decimals": 6,
            "symbol": "USDC",
            "name": "USD Coin",
            "chainId": 250,
            "address": "0x04068DA6C83AFCFA0e13ba15A6696662335D5B75",
            "resourceID": "0x00000000000000000000002791bca1f2de4661ed88a30c99a7a9449aa8417400",
            "isMintable": false,
            "isWrappedAsset": false,
            "tokenInstance": {
                "decimals": 6,
                "symbol": "USDC",
                "name": "USD Coin",
                "chainId": 250,
                "address": "0x04068DA6C83AFCFA0e13ba15A6696662335D5B75"
            }
        },
        "stableReserveAsset": {
            "decimals": 6,
            "symbol": "USDC",
            "name": "USD Coin",
            "chainId": 250,
            "address": "0x04068DA6C83AFCFA0e13ba15A6696662335D5B75",
            "resourceID": "0x00000000000000000000002791bca1f2de4661ed88a30c99a7a9449aa8417400",
            "isMintable": false,
            "isWrappedAsset": false
        },
        "tokenAmount": "10000000",
        "stableReserveAmount": "10000000",
        "priceImpact": "0",
        "tokenPath": ""
    },
    "txn": {
        "feeToken": "0x16ECCfDbb4eE1A85A33f3A9B21175Cd7Ae753dB4",
        "execution": {
            "from": "0xD29D5603edc7666dD99328b1e205bD2F33B441f6",
            "to": "0xf18aCC02628009231d7BAAF9a7a24C0860Dda6cb",
            "data": "0x4633f4e4000000000000000000000000000000000000000000000000000000000000000400000000000000000000002791bca1f2de4661ed88a30c99a7a9449aa841740000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000260000000000000000000000000000000000000000000000000000000000000028000000000000000000000000000000000000000000000000000000000000002c000000000000000000000000016eccfdbb4ee1a85a33f3a9b21175cd7ae753db4000000000000000000000000000000000000000000000000000000000000015000000000000000000000000000000000000000000000000000000000009896800000000000000000000000000000000000000000000000000000000000989680000000000000000000000000000000000000000000000000000000000098968000000000000000000000000000000000000000000000000000000000009896800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001400000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000000000000000000000000000014d29d5603edc7666dd99328b1e205bd2f33b441f62791bca1f2de4661ed88a30c99a7a9449aa8417404068da6c83afcfa0e13ba15a6696662335d5b7504068da6c83afcfa0e13ba15a6696662335d5b75000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000004200008000000000000000000000000000000000000000000000000000000000000000000",
            "value": "0",
            "gasLimit": "0x0f4240"
        }
    }
}


```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="One or more required parameters missing or invalid" %}
```javascript
// If a wrong fromTokenChainId/toTokenChainId is passed, the pathfinder will return the following message along with the error code:
"Network not supported"

// If no userAddress or an invalid userAddress is passed, the pathfinder will return the following message along with the error code:
"Invalid user/destination address"

// If a wrong fromTokenAddress/toTokenAddress is passed, the pathfinder will return the following message along with the error code:
"Invalid address"
```
{% endswagger-response %}
{% endswagger %}

{% content-ref url="../../../important-parameters/supported-chains.md" %}
[supported-chains.md](../../../important-parameters/supported-chains.md)
{% endcontent-ref %}

{% content-ref url="../../../important-parameters/supported-fee-tokens-for-erc20-transactions.md" %}
[supported-fee-tokens-for-erc20-transactions.md](../../../important-parameters/supported-fee-tokens-for-erc20-transactions.md)
{% endcontent-ref %}

{% content-ref url="../../../important-parameters/native-assets.md" %}
[native-assets.md](../../../important-parameters/native-assets.md)
{% endcontent-ref %}

{% content-ref url="get-fee-details.md" %}
[get-fee-details.md](get-fee-details.md)
{% endcontent-ref %}

{% hint style="info" %}
If all the required parameters are valid, the Pathfinder API will always return a path.
{% endhint %}

{% hint style="info" %}
For transferring native assets, just use the native token addresses given [here](../../../important-parameters/native-assets.md). However, you need to provide allowance for the wrapped version of the native asset to perform their transfers/swaps.
{% endhint %}

### Code Snippet

```javascript
import axios from "axios"

const PATH_FINDER_API_URL = "https://api.pathfinder.routerprotocol.com/api"

// calling the pathfinder api using axios
const fetchPathfinderData = async (params) => {
    const endpoint = "quote"
    const pathUrl = `${PATH_FINDER_API_URL}/${endpoint}`
    console.log(pathUrl)
    try {
        const res = await axios.get(pathUrl, { params })
        return res.data
    } catch (e) {
        console.error(`Fetching data from pathfinder: ${e}`)
    }
}

const main = async () => {
    const args = {
        'fromTokenAddress': '0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174', // USDC on Polygon
        'toTokenAddress': '0x04068DA6C83AFCFA0e13ba15A6696662335D5B75', // USDC on Fantom
        'amount': '10000000', // 10 USDC (USDC token contract on Polygon has 6 decimal places)
        'fromTokenChainId': 137, // Polygon
        'toTokenChainId': 250, // Fantom
        'userAddress': 'YOUR_WALLET_ADDRESS',
        'feeTokenAddress': '0x16ECCfDbb4eE1A85A33f3A9B21175Cd7Ae753dB4', // ROUTE on Polygon
        'slippageTolerance': 2,
        'widgetId': 24, // get your unique wdiget id by contacting us on Telegram
    }
    
    const pathfinder_response = await fetchPathfinderData(args)
    console.log(pathfinder_response)
}


main()
```

{% hint style="info" %}
**Important Note:** \
To play around with the API, you can use the Widget ID given in the example above. But for use in any product/protocol, you will be assigned a unique Widget ID. To get your Widget ID, please contact us on [Telegram](https://t.me/SurajChawla).
{% endhint %}
