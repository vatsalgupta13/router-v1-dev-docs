# 2⃣ Initialize a New Contract

We initialize the contract by passing the constructor arguments **`“uri_”`** and **`“genericHandler_”.`**

```solidity
contract CrossChainERC1155 is ERC1155, RouterCrossTalk { 
    address public owner;
    uint256 private _crossChainGasLimit;
    
    constructor(string memory uri_, address genericHandler_)
    ERC1155(uri_) RouterCrossTalk(genericHandler_) {
        owner = msg.sender;
    }
```
