# 5⃣ receiveMintCrossChain Function

**Disclaimer**: Developer need not worry about this function as it works internally inside the library and has been mentioned here just for understanding.

This is the function of **OnMintingChain** library whose selector was used in the internal **\_mintCrossChain** of the **OnFeeChain** library to initiate cross chain communication request. Find its reference [here](../on-fee-chain/mintcrosschain-function.md#how-\_mintcrosschain-function-works-internally). If the NFT is available on the minting chain, this function will mint the NFT to the recipient address on the minting chain for which the fee was collected on the fee chain. Otherwise, it will refund the fee to the refund address on the minting chain.&#x20;

```solidity
function receiveMintCrossChain(address recipient, address refundAddress)
    external
    isSelf
    returns (bool)
  {
    CurrentTokenId = CurrentTokenId + 1;
    if (CurrentTokenId > MaxTokenId || _exists(CurrentTokenId)) {
      IERC20(feeTokenForNFT).safeTransfer(refundAddress, feeInTokenForNFT);
      return true;
    }
    _mint(recipient, CurrentTokenId);
    return true;
  }

```
