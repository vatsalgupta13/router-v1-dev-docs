---
description: A function to generate a cross-chain transfer request.
---

# 4⃣ transferCrossChain Function

Once you have got the tokens minted on the minting chain, you can then transfer it to any other chain. So we  need a function to be able to transfer the tokens from a chain to any other chain. This is what this function does.&#x20;

Also this function works differently for fee chain and minting chain contracts. While it burns the NFT from user's account in the fee chain system, it just locks it in the contract in case of the minting chain system before sending a cross-chain transfer request. This ensures that every token ever minted will have an instance on the minting chain and cannot be minted again.&#x20;

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



### How \_transferCrossChain function works internally?

**Disclaimer**: Developer need not worry about this function as it works internally inside the library and has been mentioned here just for understanding.

* It checks if the NFT actually belongs to the user. If not, the function will revert.
* It transfers the **`tokenId`** from user's account to the contract itself and locks it there.
* Creates a selector of the **`“receiveCrossChain”` ** (explained [here](receivemintcrosschain-function.md)) function and encodes the respective data to be sent with the **`“routerSend”` ** function of CrossTalk.
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
    safeTransferFrom(msg.sender, address(this), tokenId);

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
