# 3⃣ Understanding Administrative Functions

* **Linker address:** Linker is an admin address which can link and unlink your contracts across chains. In order to make the cross-chain functionality work, you need to link your contracts from both sides.
* **Fee Token Address:** It is the address of the fee token you want to use for your cross-chain calls. It is recommended to use the ‘Route’ token as the fee token as you get around  50% discount on the USD value of the fees. Please find the list of tokens that can be used as a fee token [here](../../../important-parameters/supported-fee-tokens-for-crosstalk.md).
* **Cross-Chain Gas Limit:** It is the gas that is to be sent with the **`“routerSend”`** function of CrossTalk which generates a cross-chain communication request. This gas limit will be used while executing the required transaction on the destination chain.
* **Approve fees:** It takes the address of the fee token you want to use for your cross-chain calls and the amount you want to approve. Since its your contract which pays the fees for the cross-chain services, it has to provide the approval to the handler to deduct fee tokens from the contract.
* **Fee token for NFT:** It refers to the token address in which the fee for the NFT has to be collected.&#x20;
* **Fee in token for NFT:** It is the amount of the **feeTokenForNFT** that needs to be charged for the NFT.

Developer has to set the administrative functions in the manner specified below.

```solidity
  function setLinker(address _linker) external onlyOwner {
    setLink(_linker);
  }
  
  function setFeesToken(address _feeToken) external onlyOwner {
    setFeeToken(_feeToken);
  }
  
  function _approveFees(address _feeToken, uint256 _amount) external onlyOwner {
    approveFees(_feeToken, _amount);
  }
  
  function setCrossChainGasLimit(uint256 _gasLimit) external onlyOwner {
    _setCrossChainGasLimit(_gasLimit);
  }

  function setFeeTokenForNFT(address _feeToken) external onlyOwner {
    _setFeeTokenForNFT(_feeToken);
  }

  function setFeeInTokenForNFT(uint256 _price) external onlyOwner {
    _setFeeInTokenForNFT(_price);
  }

```
