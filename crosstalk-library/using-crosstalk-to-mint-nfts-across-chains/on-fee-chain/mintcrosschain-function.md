---
description: An function to generate a cross-chain minting request.
---

# 4⃣ mintCrossChain Function

Create a **mintCrossChain** function as specified below. This function calls the internal **\_mintCrossChain** function of the OnFeeChain library. Developer just has to call the internal function in his contract in the following manner:

```solidity
  function mintCrossChain(
    address _recipient,
    address _refundAddress,
    uint256 _crossChainGasPrice
  ) external returns (bytes32) {
    (bool sent, bytes32 hash) = _mintCrossChain(
      _recipient,
      _refundAddress,
      _crossChainGasPrice
    );
    require(sent == true, "Unsuccessful");
    return hash;
  }
```

### Function Parameters

| Parameter                 | Description                                                                                                                    |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **`_recipient`**          | Address of the recipient of the NFT on the minting chain.                                                                      |
| **`_refundAddress`**      | A refund shall be processed to this address on the minting chain itself in case the NFT already exists or could not be minted. |
| **`_crossChainGasPrice`** | Gas price to be sent for cross chain communication.                                                                            |

### How \_mintCrossChain function works internally?

**Disclaimer**: Developer need not worry about this function as it works internally inside the library and has been mentioned here just for understanding.

* It transfers the **`feeInTokenForNFT`** from the caller’s account to itself.
* Creates a selector of the **`“receiveMintCrossChain”` ** (explained [here](../on-minting-chain/receivemintcrosschain-function.md)) function and encodes the respective data to be sent with the **`“routerSend”` ** function of CrossTalk.
* Creates a cross-chain communication request using the **`“routerSend”`** function. Click [here](../../getting-started/write-your-first-cross-chain-contract/various-cross-chain-functions.md#1-routersend-function) for more information on "**routerSend"** function of Router's CrossTalk library.

```solidity
function _mintCrossChain(
    address _recipient,
    address _refundAddress,
    uint256 _crossChainGasPrice
  ) internal returns (bool, bytes32) {
    require(_recipient != address(0), "FeeChain: Recipient cannot be 0");
    require(
      _refundAddress != address(0),
      "FeeChain: RefundAddress cannot be 0"
    );

    IERC20(feeTokenForNFT).safeTransferFrom(
      msg.sender,
      address(this),
      feeInTokenForNFT
    );

    bytes4 _selector = bytes4(keccak256("receiveMintCrossChain(address,address)"));
    bytes memory _data = abi.encode(_recipient, _refundAddress);
    (bool success, bytes32 hash) = routerSend(
      mintingChainID,
      _selector,
      _data,
      _crossChainGasLimit,
      _crossChainGasPrice
    );
    return (success, hash);
  }
```
