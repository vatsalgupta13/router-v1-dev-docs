# 7⃣ mintSameChain Function

Create a **mintSameChain** function which will mint the NFT (if it is available) to the recipient address on the minting chain, _**for which the fee is collected on the minting chain itself**_.

```solidity
function mintSameChain(address recipient) external {
    require(recipient != address(0), "recipient != address(0)");
    mint(recipient);
  }
```

### Understanding mint function

**Disclaimer**: Developer need not worry about this function as it works internally inside the library and has been mentioned here just for understanding.

This is an internal function in the **OnMintingChain** library. If the NFT is available, this function will mint the NFT to the recipient address on the minting chain, _**for which the fee is collected on the minting chain itself**_.

#### What does this Function do?

* It transfers the **`feeInTokenForNFT`** from the caller’s account to itself on the minting chain.&#x20;
* If the NFT is available, it mints the NFT to the recipient address; otherwise, the transaction reverts.

```solidity
function mint(address recipient) internal {
    require(recipient != address(0), "MintingChain: Recipient cannot be 0");

    CurrentTokenId = CurrentTokenId + 1;
    require(CurrentTokenId <= MaxTokenId, "ERC721: MaxTokenId reached");

    IERC20(feeTokenForNFT).safeTransferFrom(
      msg.sender,
      address(this),
      feeInTokenForNFT
    );

    _mint(recipient, CurrentTokenId);
  }
```

### Function Parameters

| Parameter        | Description                                               |
| ---------------- | --------------------------------------------------------- |
| **`_recipient`** | Address of the recipient of the NFT on the minting chain. |
