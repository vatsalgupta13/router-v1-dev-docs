# 6⃣ receiveCrossChain Function

This function mints the batch of tokens to the recipient address on the destination chain which were burnt on the source chain. Note that all the functions that are called across chains need to be marked as external and have the **`“isSelf”`** modifier which means that this function can be called by the current contract only.

```solidity
  function receiveCrossChain(
        address _recipient,
        uint256[] memory _ids,
        uint256[] memory _amounts,
        bytes memory _data
    ) external isSelf returns (bool) {
        _mintBatch(_recipient, _ids, _amounts, _data);
        return true;
    }
```
