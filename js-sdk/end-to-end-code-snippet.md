# End-to-end Code Snippet

Here is how the entire script will look plugged together:

```javascript
import { RouterProtocol } from "@routerprotocol/router-js-sdk"
import { ethers } from "ethers";

const main = async() => {

// initialize a RouterProtocol instance
let SDK_ID = 24 // get your unique sdk id by contacting us on Telegram
let chainId = 137
const provider = new ethers.providers.JsonRpcProvider("https://polygon-rpc.com", chainId)
const routerprotocol = new RouterProtocol(SDK_ID, chainId, provider)
await routerprotocol.initialize()

// get a quote for USDC transfer from Polygon to Fantom
let args = {
    amount: (ethers.utils.parseUnits("10.0", 6)).toString(), // 10 USDC
    dest_chain_id: 250, // Fantom
    src_token_address: "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174", // USDC on Polygon
    dest_token_address: "0x04068DA6C83AFCFA0e13ba15A6696662335D5B75", // USDC on Fantom
    user_address: "YOUR_WALLET_ADDRESS",
    receiver_address: "RECEIVER_WALLET_ADDRESS", // same as user_address if swapping to self.
    fee_token_address: "0x16ECCfDbb4eE1A85A33f3A9B21175Cd7Ae753dB4", // ROUTE on Polygon
    slippage_tolerance: 2.0
}

const quote = await routerprotocol.getQuote(args.amount, args.dest_chain_id, args.src_token_address, args.dest_token_address, args.user_address, args.receiver_address, args.fee_token_address, args.slippage_tolerance)

// get allowance and give the relevant approvals
const wallet = new ethers.Wallet("YOUR_PRIVATE_KEY", provider) // provider was set up while initializing an instance of RouterProtocol

let src_token_allowance = await routerprotocol.getSourceTokenAllowance(args.src_token_address, args.dest_chain_id, args.user_address)
if(src_token_allowance.lt(ethers.constants.MaxUint256)){
        await routerprotocol.approveSourceToken(args.src_token_address, args.user_address, ethers.constants.MaxUint256, args.dest_chain_id, wallet)
}
if(ethers.utils.getAddress(args.src_token_address) !== ethers.utils.getAddress(args.fee_token_address)){
    let fee_token_allowance = await routerprotocol.getFeeTokenAllowance(args.fee_token_address, args.dest_chain_id, args.user_address)
    if(fee_token_allowance.lt(ethers.constants.MaxUint256)){
        await routerprotocol.approveFeeToken(args.fee_token_address, args.user_address, ethers.constants.MaxUint256, wallet)
    }
}

/**
 * If feeTokenAddress or fromTokenAddress is native address give allowance to its wrapped address.
 * For wrapper addresses, chainMapping object is maintained.
 * 
 * args.feeTokenAddress === chainMapping[chainId].NATIVE.address ?
 *      chainMapping[chainId].NATIVE.wrapped_address : args.feeTokenAddress;
 * 
 * 
 * args.fromTokenAddress === chainMapping[chainId].NATIVE.address ? 
 *      chainMapping[chainId].NATIVE.wrapped_address : args.fromTokenAddress,  
 */

// execute the transaction
let tx;
try{
    tx = await routerprotocol.swap(quote,wallet)
    console.log(`Transaction successfully completed. Tx hash: ${tx.hash}`)
}
catch(e){
    console.log(`Transaction failed with error ${e}`)
    return
}

// fetching the status of the transaction
setTimeout(async function() {
    let status = await routerprotocol.getTransactionStatus(tx.hash) 
    console.log(status)
    if (status.tx_status_code === 1) {
        console.log("Transaction completed")
      // handle the case where the transaction is complete 
    }
    else if (status.tx_status_code === 0) {
        console.log("Transaction still pending")
    // handle the case where the transaction is still pending
    }
  }, 180000); // waiting for sometime before fetching the status of the transaction because it may take some time for the transaction to get indexed

}

main()
```

Even though the private key has been hardcoded in this example - to use this code for a UI, define the provider and signer using the following code snippet:

```javascript
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    await provider.send('eth_requestAccounts', []) // connects MetaMask
    const signer = provider.getSigner()
```

```javascript
const chainMapping = {
    "137": {
        "chain": "Polygon",
        "rpc": "https://polygon-rpc.com",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0xfEd3c880FF02B195abee916328c5a3953976befD",
        "NATIVE": {
            "address": "0x0000000000000000000000000000000000001010",
            "wrapped_address": "0x4c28f48448720e9000907BC2611F73022fdcE1fA"
        }
    },
    "1": {
        "chain": "Ethereum",
        "rpc": "https://speedy-nodes-nyc.moralis.io/36a3a9840a5f2cc2ea2bbb42/eth/mainnet",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x5e9A385a15cDE1b149Cb215d9cF3151096A37D67",
        "NATIVE": {
            "address": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
            "wrapped_address": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"
        }
    },
    "250": {
        "chain": "Fantom",
        "rpc": "https://rpc.ftm.tools/",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x621F0549102262148f6a7D289D8330adf7CbC09F",
        "NATIVE": {
            "address": "0x0100000000000000000000000000000000000001",
            "wrapped_address": "0x21be370D5312f44cB42ce377BC9b8a0cEF1A4C83"
        }
    },
    "42161": {
        "chain": "Arbitrum",
        "rpc": "https://arb1.arbitrum.io/rpc",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x88b1E0ecaC05b876560eF072d51692F53932b16f",
        "NATIVE": {
            "address": "0x0000000000000000000000000000000000001010",
            "wrapped_address": "0x82aF49447D8a07e3bd95BD0d56f35241523fBab1"
        }
    },
    "56": {
        "chain": "BSC",
        "rpc": "https://bsc-dataseed.binance.org/",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x45d880647Ec9BEF6Bff58ee6bB985C67d7234b0C",
        "NATIVE": {
            "address": "0x0100000000000000000000000000000000000001",
            "wrapped_address": "0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c"
        }
    },
    "43114": {
        "chain": "Avalanche",
        "rpc": "https://api.avax.network/ext/bc/C/rpc",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x5febcA23e97c8ead354318e5A3Ed34ec3704459a",
        "NATIVE": {
            "address": "0x0100000000000000000000000000000000000001",
            "wrapped_address": "0xB31f66AA3C1e785363F0875A1B74E27b85FD66c7"
        }
    },
    "10": {
        "chain": "Optimism",
        "rpc": "https://mainnet.optimism.io",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x88b1E0ecaC05b876560eF072d51692F53932b16f",
        "NATIVE": {
            "address": "0x0000000000000000000000000000000000001010",
            "wrapped_address": "0x4200000000000000000000000000000000000006"
        }
    },
    "25": {
        "chain": "Cronos",
        "rpc": "https://evm.cronos.org",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0xf44Ff799eA2bBFeC96f9A50498209AAc3C2b3b8b",
        "NATIVE": {
            "address": "0x0000000000000000000000000000000000000001",
            "wrapped_address": "0x5C7F8A570d578ED84E63fdFA7b1eE72dEae1AE23"
        }
    },
    "1666600000": {
        "chain": "Harmony",
        "rpc": "https://api.harmony.one",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x8413041a7702603d9d991F2C4ADd29e4e8A241F8",
        "NATIVE": {
            "address": "0x0000000000000000000000000000000000001010",
            "wrapped_address": "0xcF664087a5bB0237a0BAd6742852ec6c8d69A27a"
        }
    },
    "1313161554": {
        "chain": "Aurora",
        "rpc": "https://mainnet.aurora.dev",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0x13538f1450Ca2E1882Df650F87Eb996fF4Ffec34",
        "NATIVE": {
            "address": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
            "wrapped_address": "0xC9BdeEd33CD01541e1eeD10f90519d2C06Fe3feB"
        }
    },
    "2222": {
        "chain": "Kava",
        "rpc": "https://evm.kava.io",
        "reserveHandler_address": "0x6e14f48576265272B6CAA3A7cC500a26050Be64E",
        "oneSplit_address": "0xB065a867a1baa919F0A9a3F5C1543D19768CeFBD",
        "NATIVE": {
            "address": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
            "wrapped_address": "0xc86c7C0eFbd6A49B35E8714C5f59D99De09A225b"
        }
    }
}
```
