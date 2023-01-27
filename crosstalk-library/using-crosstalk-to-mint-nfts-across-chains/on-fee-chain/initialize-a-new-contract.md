# 2⃣ Initialize a New Contract

We initialize the contract by passing the constructor arguments **`“mintingchainID”`** and **`“genericHandler_”.`**

* **name & symbol:** Name and symbol for your NFT.
* **mintingChainID:** It refers to the chain ID of the chain where the NFT minting takes place. This is an immutable public variable that should be in accordance with Router Protocol's internal chain IDs, which can be found [here](../../../important-parameters/supported-chains.md).
* **genericHandler:** It refers to the address of the generic handler for the chain you are deploying your contract on. Address of the generic handler for different chains can be found [here](../../deployment-addresses/generic-handler-addresses.md).

```solidity
contract SampleFeeChain is OnFeeChain {
  using SafeERC20 for IERC20;
  address public owner;

  constructor(
      string memory name_,
      string memory symbol_,
      uint8 mintingChainID_, 
      address genericHandler_
      )
    OnFeeChain(
       name_, 
       symbol_,
       mintingChainID_, 
       genericHandler_
    )
  {
    owner = msg.sender;
  }
```
