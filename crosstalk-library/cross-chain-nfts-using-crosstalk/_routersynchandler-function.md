---
description: An internal function to control the handling of various selectors and data.
---

# 5⃣ \_routerSyncHandler Function

```solidity
function _routerSyncHandler(bytes4 _selector, bytes memory _data)
        internal
        virtual
        override
        returns (bool, bytes memory)
    {
        (address _recipient, uint256[] memory _ids, uint256[] memory _amounts, bytes memory data) = abi.decode(
            _data,
            (address, uint256[], uint256[], bytes)
        );
        (bool success, bytes memory returnData) = address(this).call(
            abi.encodeWithSelector(_selector, _recipient, _ids, _amounts, data)
        );
        return (success, returnData);
    }


```

### Function Parameters

| Parameter       | Description                                                                                                                                                                                     |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`_selector`** | Selector that would be matched when a cross-chain request is received. For example, here it will be of the form: **`bytes4(keccak256("receiveCrossChain(address,uint256[],uint256[],bytes)")`** |
| **`_data`**     | Data that is to be decoded and handled.                                                                                                                                                         |

{% hint style="info" %}
**Note:**

For more information regarding this function, check out this [link](../getting-started/write-your-first-cross-chain-contract/various-cross-chain-functions.md#3-\_routersynchandler-function).
{% endhint %}
