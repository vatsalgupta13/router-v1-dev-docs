# Vault Contract

Vault contract enables the user to first transfer his funds from chain A to chain B and then stake them on chain B. This is the contract where we shall integrate our Sequencer Crosstalk library.

To understand how Router Sequencer CrossTalk works, let’s create Vault contract:

### **Step 1) Importing the library and initialising the Vault contract**

Let’s initialize the Router Sequencer CrossTalk contract with the address of the Sequencer handler, ERC-20 handler and Reserve handler (All deployment addresses can be found [here](../../../deployment-addresses/)). Here, **`_token`** is the address of the token to be staked.

```solidity
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;
 
import "hardhat/console.sol";
import "@routerprotocol/router-crosstalk/contracts/RouterSequencerCrossTalk.sol";

contract Vault is RouterSequencerCrossTalk, AccessControl {
    constructor(
        address _token,
        address _sequencerHandler,
        address _erc20handler,
        address _reservehandler
    ) RouterSequencerCrossTalk(_sequencerHandler, _erc20handler, _reservehandler) {
        token = IERC20(_token);
        _setupRole(DEFAULT_ADMIN_ROLE, msg.sender);
    }
```

Before we go ahead, it is important to note that there are some admin functions that must be called before writing the actual contract logic.

### Step 2) Setting the linker address and fee token address in Vault

**Linker address**: Linker is an admin address which can link and unlink your contracts across chains. In order to make the cross-chain functionality work, you need to link your contracts from both sides.&#x20;

**Fee Token address**: It is the address of the fee token you want to use for your cross-chain calls. It is recommended to use the ‘**Route**’ token as the fee token as you get a discount on the USD value of the fees. Please find the list of tokens that can be used as a fee token [here](../../../../important-parameters/supported-fee-tokens-for-crosstalk.md).&#x20;

Developer has to make two external functions which call two internal functions of the library that are **`setLink`** and **`setFeeToken`** as shown in the code snippet below:

```solidity
function setLinker(address _linker) external onlyRole(DEFAULT_ADMIN_ROLE)
{
        //Calling the setLink function of the Router Sequencer CrossTalk contract
        setLink(_linker);
    }
 
function setFeesToken(address _feeToken) external onlyRole(DEFAULT_ADMIN_ROLE)
{
        //Calling the setFeeToken function of the Router Sequencer CrossTalk contract
        setFeeToken(_feeToken);
    }
```

### Step 3) Approving the fee token

**`approveFees`** function takes the address of the fee token you want to use for your cross-chain calls and the amount you want to approve. It provides the approval to the handler to deduct fees from your source contract.

Developer has to make an external/public function to call the internal function **`approveFees`** as shown in the code snippet below:

```solidity
function _approveFees(address _feeToken, uint256 _value) public {
        //Calling the approveFees function of the Router Sequencer CrossTalk contract
        approveFees(_feeToken, _value);
    }
```

### Step 4) **Initialising cross-chain communication request by calling routerSend function**

**`routerSend`** is an internal function of the Router Sequencer CrossTalk contract to generate a cross chain communication request. You just need to use this function when you want to make a cross-chain call. It takes a struct of required parameters as an argument to proceed with the communication. Struct of parameters includes:

| Parameters        | Description                                                                                                                                                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_destChainId     | Chain Id of the destination chain                                                                                                                                                                                                                       |
| \_erc20           | Data for the ERC20 transaction\*                                                                                                                                                                                                                        |
| \_swapData        | Swap details for ERC20 transaction\*                                                                                                                                                                                                                    |
| \_generic         | Data for generic cross-chain call\*                                                                                                                                                                                                                     |
| \_gasLimit        | Gas Limit set for the call                                                                                                                                                                                                                              |
| \_gasPrice        | Gas price set for the call                                                                                                                                                                                                                              |
| \_feeToken        | Address of fee token for payment of fees                                                                                                                                                                                                                |
| \_isTransferFirst | It is where the sequence for ERC20 transaction and generic call is specified. It is supposed to be a boolean value, true, if ERC20 transaction is to be prioritised over generic call, and false, if generic call is to be prioritised over ERC20 call. |
| \_isOnlyGeneric   | If the transaction does not involve cross chain token transfer or swap, then this should be set as true.                                                                                                                                                |

{% hint style="info" %}
\*Please refer [here](how-to-get-data-for-routersend-function.md) to know how to fetch generic data, erc data and swap data.
{% endhint %}

Developer can call the routerSend function in the following way:

```solidity
function receiveStakeCrossChain(address _user, uint256 _amount)
        external
        isSelf
    {
        stakingContract.stake(_user, _amount);
    }
 
function stakeCrossChain(
        uint8 _chainID,
        uint256 _crossChainGasLimit,
        uint256 _crossChainGasPrice,
        bytes memory _ercData,
        bytes calldata _swapData
    ) external returns (bytes32) {
        bytes4 _selector = bytes4(
            keccak256("receiveStakeCrossChain(address,uint256)")
        );
        uint256 amount = abi.decode(_swapData, (uint256));
        bytes memory _data = abi.encode(msg.sender, amount);
        bytes memory _genericData = abi.encode(_selector, _data);
        Params memory params = Params(
            _chainID,
            _ercData,
            _swapData,
            _genericData,
            _crossChainGasLimit,
            _crossChainGasPrice,
            this.fetchFeeToken(),
            true,
            false
        );
        (bool success, bytes32 hash) = routerSend(params);
        require(success, "Unsuccessful");
        return hash;
    }

```

{% hint style="info" %}
Make sure to use the **`isSelf` ** modifier on all functions that can be called across chains.
{% endhint %}

### Step 5) Defining \_routerSyncHandler Function

It is an internal virtual function which is defined in the Router Sequencer CrossTalk contract but has to be overridden by the user according to what functionality the user might want when a cross-chain request is received from another chain. Note that this function is called by an internal function “routerSync” of the Router Sequencer CrossTalk contract. It takes the following parameters:

* Function selector which should be called when a cross-chain request is received.
* The encoded data which has to be decoded in this function in order to be used.

Developer can define this function in the following way. Here, the data that was encoded in generic data on the source side while initialising the cross chain communication request, has been decoded first. After that the data has been used as a parameter with the selector of the function that has to be called on the destination side. In this example, the **`receiveStakeCrossChain`** is the function that we want to call on the destination side.

```solidity
function _routerSyncHandler(bytes4 _selector, bytes memory _data)
        internal
        override
        returns (bool, bytes memory)
    {
        if (
            _selector ==
            bytes4(keccak256("receiveStakeCrossChain(address,uint256)"))
        ) {
            (address user, uint256 amount) = abi.decode(
                _data,
                (address, uint256)
            );
            (bool success, bytes memory data) = address(this).call(
                abi.encodeWithSelector(_selector, user, amount)
            );
            return (success, data);
        }
        return (true, "");
    }

```

{% hint style="info" %}
Note: You just have to handle the logic for your generic call in the routerSyncHandler function. The token transfer is handled by ERC-20 handler itself. For example: here receiveStakeCrossChain function is not involved in any token transfer but it only updates the stakes of user in the Stake contract on the destination chain (the generic call) only which is why it has been handled by routerSyncHandler function.
{% endhint %}

### Step 6) **Enabling Replay mechanism**

**`routerReplay`** function: This function is used to execute a transaction which could not be completed on the destination chain due to insufficient gasLimit or gasPrice provided in the routerSend function. This function takes 3 arguments:

| Parameters | Description                                                                                     |
| ---------- | ----------------------------------------------------------------------------------------------- |
| \_hash     | The hash returned from routerSend function                                                      |
| \_gasLimit | This gas limit has to be higher than or equal to the amount provided in the routerSend function |
| \_gasPrice | This gas price has to be higher than the amount provided in the routerSend function             |

Developer has to call **`routerReplay`** function from an external function in order to make this mechanism work in the following way:

```solidity
function replayTx(
        bytes32 _hash,
        uint256 _crossChainGasLimit,
        uint256 _crossChainGasPrice
    ) external onlyRole(DEFAULT_ADMIN_ROLE)
{
        routerReplay(
            _hash,
            _crossChainGasLimit,
            _crossChainGasPrice
        );
    }

```

Now that our Vault contract is ready, find the end-to-end code [here](end-to-end-vault-contract.md).
