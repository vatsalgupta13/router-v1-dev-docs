---
description: Function to receive an NFT transferred from another chain.
---

# 6⃣ receiveCrossChain Function

**Disclaimer**: Developer need not worry about this function as it works internally inside the library and has been mentioned here just for understanding.

This function handles cross-chain NFT transfer request received from another chain and just mints the NFT of the specified token ID to the user.&#x20;

```solidity
function receiveCrossChain(address recipient, uint256 tokenId)
    external
    isSelf
  {
    require(recipient != address(0), "recipient != address(0)");
    _safeMint(recipient, tokenId);
  }
```
