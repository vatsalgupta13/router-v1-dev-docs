---
description: An internal function to control the handling of various selectors and data.
---

# 4⃣ \_sendCrossChain Function

### What does this Function do?

* It burns the batch of amounts of respective tokens with mentioned tokenIds from the current chain from the caller’s account.
* Creates a selector of the **`“receiveCrossChain”` ** (explained [here](replaytransfercrosschain-function.md)) function and encodes the respective data to be sent with the **`“routerSend”` ** function of CrossTalk.
* Creates a cross-chain communication request using the **`“routerSend”`** function.

```solidity
function _sendCrossChain(
        uint8 _destChainID,
        address _recipient,
        uint256[] memory _ids,
        uint256[] memory _amounts,
        bytes memory _data,
        uint256 _crossChainGasPrice
    ) internal returns (bool, bytes32) {
        _burnBatch(msg.sender, _ids, _amounts);
        bytes4 _selector = bytes4(
            keccak256("receiveCrossChain(address,uint256[],uint256[],bytes)")
        );
        bytes memory data = abi.encode(_recipient, _ids, _amounts, _data);
        (bool success, bytes32 hash) = routerSend(
            _destChainID,
            _selector,
            data,
            _crossChainGasLimit,
            _crossChainGasPrice
        );

        return (success, hash);
    }
```

### Function Parameters

| Parameter                 | Description                                                                                                                |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| **`_chainId`**            | Chain ID of the destination chain on which the tokens are to be minted.                                                    |
| **`_recipient`**          | Address of the recipient of the tokens on the destination chain.                                                           |
| **`_ids`**                | IDs of the tokens which are to be burnt on the current chain and minted on the destination chain to the recipient address. |
| **`_amounts`**            | Amounts of respective tokens with **`“_ids”` ** to be burnt on the current chain and minted on the destination chain.      |
| **`_data`**               | Any additional data with which the tokens will be minted.                                                                  |
| **`_crossChainGasPrice`** | Gas price to be used while executing the transaction on the destination chain.                                             |

{% hint style="info" %}
If you pass a lower gas price than required, the transaction may get stuck on the bridge and you may need to replay it using the replayTransaction function.
{% endhint %}
