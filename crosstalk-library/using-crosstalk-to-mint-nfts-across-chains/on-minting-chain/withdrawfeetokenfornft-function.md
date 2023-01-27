# 9⃣ withdrawFeeTokenForNFT Function

This is an external function in the **OnMintingChain** library and needs to be overridden by the developer to manage the fees collected for NFTs in the contract. Developer can implement this function in the following manner:

```solidity
function withdrawFeeTokenForNFT() external override onlyOwner {
    address feeToken = this.fetchFeeTokenForNFT();
    uint256 amount = IERC20(feeToken).balanceOf(address(this));
    IERC20(feeToken).transfer(owner, amount);
  }
```

{% hint style="info" %}
**Note:**

The entire contract for the same can be found [here](end-to-end-sample-minting-chain-contract.md).
{% endhint %}
