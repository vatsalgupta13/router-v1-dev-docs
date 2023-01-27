---
description: Function to replay a transaction stuck on the bridge.
---

# 8⃣ replayTransaction function

Create a **replayTransaction** function as specified below. This function is used to enable replay mechanism, that is to re-execute a transaction that could not be executed on the destination chain due to insufficient gasLimit or gasPrice provided in the **`mintCrossChain`** function.

```solidity
// The hash returned from mintCrossChain function should be used to replay a tx.
// These gas limit and gas price should be higher than one entered in the original tx.
function relpayTransaction(
    bytes32 hash,
    uint256 gasLimit,
    uint256 gasPrice
  ) external onlyOwner {
    replayTx(hash, gasLimit, gasPrice);
  }
```
