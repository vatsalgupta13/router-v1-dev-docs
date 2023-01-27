# How to get data for routerSend function?

## 1) Generic Data

Generic Data consists of two things:

* Selector to the function that is to be called on the destination side.
* Parameters encoded with abi.encode to be passed in that function.

As shown in the example below, the **`receiveStakeCrossChain`** function is the function that is to be called on the destination chain after the transaction on the source chain is completed.\
First, we make the selector of the **`receiveStakeCrossChain`** function and second, we ABI-encode the value of the parameters. Lastly, we ABI-encode, the selector and the encoded data together, which constitutes the generic data.

```solidity
function receiveStakeCrossChain(address _user, uint256 _amount)
        external
        isSelf
    {
        stakingContract.stake(_user, _amount);
    }
 
function getGenericData(address _user, uint256 _amount) external view returns
(bytes memory) {
        bytes4 _selector = bytes4(keccak256("receiveStakeCrossChain(address,uint256)"
));
        bytes memory data = abi.encode(_user, _amount);
        bytes memory _generic = abi.encode(_selector, data);
	  return _generic;
}
```

## 2) Swap Data and ERC Data

The swap data and the erc data depend on the following parameters:

* Source chain id
* Destination chain id
* Source Token address
* Destination Token Address
* Fee token address&#x20;
* Amount to be swapped
* Recipient address
* Slippage tolerance in %
* isDestNative: If the destination token is native token for that chain, then this is true else false.

To make the construction of the swap and erc data easy for the developers we have created an API which can be called using the required parameters to generate the swap data and erc data.&#x20;

Below is an example for getting data for swapping 0.01 USDC from Avalanche chain to USDC on the Polygon chain. Kindly follow the following steps to achieve the same according to your project:

This script will fetch the ERC Data and the Swap Data required for the Params struct when supplied the required arguments.&#x20;

```javascript
import axios from "axios";

// get a quote for USDC transfer from Avalanche to Kava
const baseUrl =
  "https://api.pathfinder.routerprotocol.com/api/quote/sequencer?";

const args = {
  fromTokenAddress: "0xA7D7079b0FEaD91F3e65f86E8915Cb59c1a4C664", // USDC on Avalanche
  toTokenAddress: "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174", // USDC on Polygon
  amount: "10000", // 0.01 USDC as USDC is 6 decimals
  sourceChainId: "43114", // Avalanche
  destChainId: "137", // Polygon
  feeTokenAddress: "0xA7D7079b0FEaD91F3e65f86E8915Cb59c1a4C664", // USDC on Avalanche
  recipientAddress: "YOUR VAULT CONTRACT ON POLYGON", // Receiver on Polygon -> Vault contract address
  isDestNative: "false",
};

const url =
  baseUrl +
  "fromTokenAddress=" +
  args.fromTokenAddress +
  "&toTokenAddress=" +
  args.toTokenAddress +
  "&amount=" +
  args.amount +
  "&fromTokenChainId=" +
  args.sourceChainId +
  "&toTokenChainId=" +
  args.destChainId +
  "&feeTokenAddress=" +
  args.feeTokenAddress +
  "&isDestNative=" +
  args.isDestNative +
  "&recipientAddress=" +
  args.recipientAddress;

async function main() {
  const { data } = await axios.get(url);
  console.log(data);
}

main();
```

