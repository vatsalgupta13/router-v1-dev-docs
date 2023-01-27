# 4⃣ Check the Status

After executing a cross-chain transaction, you can also check its status by querying the following API:

{% swagger method="get" path="/status" baseUrl="https://api.stats.routerprotocol.com/api" summary="Checking the status of a cross-chain transaction" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="txHash" type="String" required="true" %}
Transaction hash of the successfully mined source chain transaction.
{% endswagger-parameter %}

{% swagger-parameter in="query" name="networkId" type="String/Int" required="true" %}
Network ID of the source chain, i.e. where the transaction was initiated (eg: 

**`137`**

 for Polygon, 

**`56`**

 for BNB Chain).
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="The HTTP request was successful" %}
```javascript
// Sample response (status for a transaction from Polygon to Fantom):

{
    "tx_status": "Completed",
    "tx_status_code": 1, // 0 for Pending, 1 for Completed
    "src_chain_id": "137",
    "dest_chain_id": "56",
    "src_tx_hash": "0x0d52f66fc13b5d7c1c24d856293effa399bb8189f0a6e2f495400ff5f6eb8966",
    "dest_tx_hash": "0x563f4c5bf6b606b38f7d5e4c906902e95e53da18bc1ee348daec733a720e5cab"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="One or more required parameters missing or invalid" %}
```javascript
// If no txHash is passed, the API will return the following message along with the error code:
"Please provide source transaction hash"

// If a wrong txHash (invalid hash or non Router transaction hash) is passed, the API will return the following message along with the error code:
"Unknown transaction"

// If no networkId is passed, the pathfinder will return the following message along with the error code:
"Please provide networkId"

// If an invalid/unsupported networkId is passed, the API will return the following message along with the error code:
"Unsupported network ID"
```
{% endswagger-response %}
{% endswagger %}

### Code Snippet

```javascript
import axios from "axios"

const STATS_API_URL = "https://api.stats.routerprotocol.com/api"

// calling the status api using axios
const fetchStatus = async (params) => {
    const endpoint = "status"
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
    // sending the transaction using the data prepared by us in step 3
    const tx = await wallet.sendTransaction(pathfinder_response.txn.execution)
    try {
        await tx.wait();
        console.log(`Transaction mined successfully: ${tx.hash}`)
    }
    catch (error) {
        console.log(`Transaction failed with error: ${error}`)
        return
    }
    
    let params = {
    txHash: tx.hash,
    networkId: args.fromTokenChainId // args were defined in step 1 to fetch data from the pathfinder
    }
    
   setTimeout(async function() {
        let status = await fetchStatus(params) 
        console.log(status)
        if (status.tx_status_code === 1) {
            console.log("Transaction completed")
          // handle the case where the transaction is complete 
        }
        else if (status.tx_status_code === 0) {
            console.log("Transaction still pending")
        // handle the case where the transaction is still pending
        }
      }, 180000); // waiting for sometime before fetching the status of the transaction
}

main()
```
