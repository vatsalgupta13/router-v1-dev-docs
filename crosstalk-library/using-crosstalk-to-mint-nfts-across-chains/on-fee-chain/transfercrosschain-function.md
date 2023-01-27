---
description: A function to generate a cross-chain transfer request.
---

# 5⃣ transferCrossChain Function

Once you have got the tokens minted on the minting chain, you can then transfer it to any other chain. So we  need a function to be able to transfer the tokens from a chain to any other chain. This is what this function does.&#x20;

Create a **transferCrossChain** function as specified below. This function calls the internal **\_transferCrossChain** function of the OnFeeChain library. Developer just has to call the internal function in his contract in the following manner:

```solidity
function transferCrossChain(
    uint8 destChainId,
    address recipient,
    uint256 tokenId,
    uint256 crossChainGasPrice
  ) external returns (bytes32) {
    (bool sent, bytes32 hash) = _transferCrossChain(
      destChainId,
      recipient,
      tokenId,
      crossChainGasPrice
    );

    require(sent == true, "Unsuccessful");
    return hash;
  }
```

### Function Parameters

| Parameter                | Description                                                   |
| ------------------------ | ------------------------------------------------------------- |
| **`destChainId`**        | Chain ID of the destination chain (Router specific).          |
| **`recipient`**          | Address of the recipient of the NFT on the destination chain. |
| **`tokenId`**            | Token ID of the NFT user wants to transfer to another chain.  |
| **`crossChainGasPrice`** | Gas price to be sent for cross chain communication.           |

### How \_transferCrossChain function works internally?

**Disclaimer**: Developer need not worry about this function as it works internally inside the library and has been mentioned here just for understanding.

* It checks if the NFT actually belongs to the user. If not, the function will revert.
* It burns the **`tokenId`** from user's account.
* Creates a selector of the **`“receiveCrossChain”` ** (explained [here](receivecrosschain-function.md)) function and encodes the respective data to be sent with the **`“routerSend”` ** function of CrossTalk.
* Creates a cross-chain communication request using the **`“routerSend”`** function. Click [here](../../getting-started/write-your-first-cross-chain-contract/various-cross-chain-functions.md#1-routersend-function) for more information on "**routerSend"** function of Router's CrossTalk library.

```solidity
function _transferCrossChain(
    uint8 destChainId,
    address recipient,
    uint256 tokenId,
    uint256 crossChainGasPrice
  ) internal returns (bool, bytes32) {
    require(_exists(tokenId) && ownerOf(tokenId) == msg.sender, "not your NFT");
    require(recipient != address(0), "recipient != address(0)");

    _burn(tokenId);

    bytes4 selector = bytes4(keccak256("receiveCrossChain(address,uint256)"));
    bytes memory data = abi.encode(recipient, tokenId);
    (bool success, bytes32 hash) = routerSend(
      destChainId,
      selector,
      data,
      crossChainGasLimit,
      crossChainGasPrice
    );

    return (success, hash);
  }
```

