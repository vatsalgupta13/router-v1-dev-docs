---
description: Function to receive an NFT transferred from another chain.
---

# 6⃣ receiveCrossChain function

**Disclaimer**: Developer need not worry about this function as it works internally inside the library and has been mentioned here just for understanding.

Also this function works differently for fee chain and minting chain contracts. While it mints the NFT with the token ID on the fee chain system, it unlocks the NFT and sends it to the user on the minting chain. This is because it was already locked when the user transferred that token from the minting chain to another chain.

This function handles cross-chain NFT transfer request received from another chain and just mints the NFT of the specified token ID to the user.

```solidity
function receiveCrossChain(address recipient, uint256 tokenId)
    external
    isSelf
  {
    require(
      _exists(tokenId) && ownerOf(tokenId) == address(this),
      "invalid request"
    );
    require(recipient != address(0), "recipient != address(0)");

    _safeTransfer(address(this), recipient, tokenId, "");
  }
```
