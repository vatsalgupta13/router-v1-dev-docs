# 7⃣ replayTransferCrossChain Function

This function is used to enable replay mechanism, that is to execute a transaction that could not be completed on the destination chain due to insufficient gasLimit or gasPrice provided in the **`routerSend`** function.

```solidity
    /**
     * @notice replayTransaction Used to replay the transaction if it failed due to low gaslimit or gasprice
     * @param hash Hash returned by transferCrossChain function
     * @param crossChainGasLimit new crossChainGasLimit
     * @param crossChainGasPrice new crossChainGasPrice
     * NOTE gasLimit and gasPrice passed in this function should be greater than what was passed earlier
     */
    function replayTransaction(
        bytes32 hash,
        uint256 crossChainGasLimit,
        uint256 crossChainGasPrice
    ) internal {
        routerReplay(hash, crossChainGasLimit, crossChainGasPrice);
    }
```

{% hint style="info" %}
**Note:**

The entire contract for the same can be found [here](../examples/cross-chain-erc1155.md).
{% endhint %}
