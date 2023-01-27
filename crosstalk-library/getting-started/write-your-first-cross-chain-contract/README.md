# 2⃣ Write your First Cross-chain Contract

To understand how Router CrossTalk works, let’s create a new cross-chain contract that allows us to change or update a value of a variable on different chains.

### **Step 1) Initializing the contract**

Let’s start off with the pragma statement and initialize the RouterCrossTalk contract with the address of the generic handler:

```solidity
pragma solidity ^0.8.0;
import "@routerprotocol/router-crosstalk/contracts/nonupgradeable/RouterCrossTalk.sol";

contract CrossChainValueTransfer is RouterCrossTalk{	
	uint public value;		  
	//Initializing Router's CrossTalk contract
	constructor(address _genericHandler) RouterCrossTalk(_genericHandler){}										
}
```

Before we go ahead, it is important to note that there are some admin functions that must be called before writing the actual contract logic.

### Step 2) Setting the linker address

The linker is an admin address that can link contracts across chains. Consider that you have created a cross-chain contract and deployed it onto two chains (say Polygon and Binance Smart Chain). In order to make the cross-chain functionality using Router CrossTalk work, you need to link those contracts using a **`RouterLinker`** object by using the **`MapContract`** function of the **`Generic Handler`**. The **`MapContract`** function takes 1 argument as input: a **`RouterLinker`** object.

You can link the contract using the **`MapContract`** function, and you can also unlink the contract by using the **`UnMapContract`** function of the Generic Handler by passing the **`RouterLinker`** object.

```solidity
    // _rSyncContract is the address of the source contract
    // _chainId is the destination chain Id
    // _linkedContract is the address of the same contract on the destination chain
     
    struct RouterLinker {
            address _rSyncContract;
            uint8 _chainID;
            address _linkedContract;
        }
 
    /// @notice Function Maps the two contracts on cross-chain environment
    /// @param linker Linker object to be verified
    function MapContract(RouterLinker calldata linker) external {
        iRouterCrossTalk crossTalk = iRouterCrossTalk(linker._rSyncContract);
        require(
            msg.sender == crossTalk.fetchLinkSetter(),
            "Router Generic Handler : Only Link Setter can map contracts"
        );
        crossTalk.Link{ gas: 57786 }(linker._chainID, linker._linkedContract);
    }
```

To set a linker address, the RouterCrossTalk contract has an internal function called **`setLink`**. To demonstrate how this function works, let us create an external function that further calls the **`setLink`** function of the RouterCrossTalk contract. Note that this function can only be called by the owner of the contract.

```solidity
address public owner;
 
modifier onlyOwner(){
    require(msg.sender == owner, "Not owner");
    _;
}
 
function setLinker ( address _linker ) external onlyOwner() {
    // Calling the setLink function of the RouterCrossTalk Contract  
    setLink(_linker);
}
```

You may call this function after deployment and set an admin address as a linker.

### Step 3) Setting the fee token address

The RouterCrossTalk contract has an internal function called the **`setFeeToken`** function that takes the address of the fee token you want to use for your cross-chain calls. The token has to be whitelisted on the generic handler; otherwise, you won’t be able to use it. It is recommended to use the ROUTE token as the fee token as you get almost a 50% discount on the USD value of the fees on using the ROUTE token.

```solidity
// Internal function of the RouterCrossTalk contract
  function setFeeToken( address _addr ) internal {
      feeToken = _addr;
  }
```

Let us create a new external function that can set the fee token and can only be called by the owner of the contract.

```solidity
  function setFeeAddress ( address _feeAddress ) external onlyOwner(){
        setFeeToken(_feeAddress);
  }
```

### Step 4) Approving the fee token

The RouterCrossTalk contract has an internal function called the **`approveFees`** function, which takes the address of the fee token you want to use for your cross-chain calls and the amount you want to approve. The function basically provides the approval to the Generic Handler contract to deduct the fee for cross-chain calls from the contract. The token has to be whitelisted on the Generic Handler; otherwise, you won’t be able to use it. It is recommended to use ROUTE token as the fee token as you get a 50% discount on the USD value of the fees.

```solidity
// Internal function of the RouterCrossTalk contract
  function approveFees(address _feeToken, uint256 _value) internal {
    IERC20 token = IERC20(_feeToken);
    token.approve(address(handler), _value);
  }
```

Let us create a new external function to approve the Generic Handler contract for the fee token. Note that this function can only be called by the owner of the contract.

```solidity
  function approveFee(address _feeToken, uint256 _value) external{
    approveFees(_feeToken, _value);
  }
```

For the next few steps, the user has to call a few functions from the RouterCrossTalk contract. These functions have been explained in detail on the next [page](various-cross-chain-functions.md).
