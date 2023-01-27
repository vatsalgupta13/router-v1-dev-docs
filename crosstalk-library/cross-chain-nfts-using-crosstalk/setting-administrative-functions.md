# 3⃣ Setting Administrative functions

**`“_crossChainGasLimit”`** is the gas limit that is to be used while executing the transaction on the destination chain. Administrative functions can only be set by the administrator of the contract.&#x20;

```solidity
 
   /**
     * @notice setLinker Used to set address of linker, this can only be set by Admin
     * @param _linker Address of the linker
     */
    function setLinker(address _linker) external onlyOwner {
        setLink(_linker);
    }

    /**
     * @notice _approveFees To approve handler to deduct fees from source contract, this can only be set by Admin
     * @param _feeToken Address of the feeToken
     * @param _amount Amount to be approved
     */
    function _approveFees(address _feeToken, uint256 _amount)
        external
        onlyOwner
    {
        approveFees(_feeToken, _amount);
    }

    /**
     * @notice setFeesToken To set the fee token in which fee is desired to be charged, this can only be set by Admin
     * @param _feeToken Address of the feeToken
     */
    function setFeesToken(address _feeToken) external onlyOwner {
        setFeeToken(_feeToken);
    }
    
    /**
     * @notice setCrossChainGasLimit Used to set CrossChainGasLimit, this can only be set by Admin
     * @param _gasLimit Amount of gasLimit that is to be set
     */
    function setCrossChainGasLimit(uint256 _gasLimit) external onlyOwner {
        _crossChainGasLimit = _gasLimit;
    }

    /**
     * @notice fetchCrossChainGasLimit Used to fetch CrossChainGasLimit
     * @return crossChainGasLimit that is set
     */
    function fetchCrossChainGasLimit() external view returns (uint256) {
        return _crossChainGasLimit;
    }
```

{% hint style="info" %}
**Note:**\
****Administrative functions, such as **`“setLinker”`**`,`**`"approveFees"`** and **`“setFeeToken”`** are to be mandatorily incorporated into your contract. Please refer to this [link ](../overview/components.md#2-administrative-components)for more information on administrative components.
{% endhint %}
