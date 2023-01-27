# Various Cross-chain Functions

### 1) routerSend function:

This is an internal function of the RouterCrossTalk contract to generate a cross-chain communication request. You can use this function to send any arbitrary data to another chain.

#### How to use this function?

| Parameter   | Description                                   |
| ----------- | --------------------------------------------- |
| destChainId | Chain ID of the destination chain             |
| \_selector  | Selector to interface on the destination side |
| \_data      | Data to be sent on the destination side       |
| \_gasLimit  | Gas Limit to be sent for the transfer         |
| \_gasPrice  | Gas Price to be sent for the transfer         |

```solidity
  function routerSend(
    uint8 destChainId,
    bytes4 _selector,
    bytes memory _data,
    uint256 _gasLimit,
    uint256 _gasPrice
  ) internal isLinkUnSet(destChainId) returns (bool, bytes32) {
    bytes memory data = abi.encode(_selector, _data);
    uint64 nonce = handler.genericDeposit(
      destChainId,
      data,
      _gasLimit,
      _gasPrice,
      feeToken
    );
 
    bytes32 hash = _hash(destChainId, nonce);
 
    executes[hash] = ExecutesStruct(destChainId, nonce);
    emitCrossTalkSendEvent(destChainId, _selector, _data, hash);
 
    return (true, hash);
  }
 
  function emitCrossTalkSendEvent(
    uint8 destChainId,
    bytes4 selector,
    bytes memory data,
    bytes32 hash
  ) private {
    emit CrossTalkSend(
      handler.fetch_chainID(),
      destChainId,
      address(this),
      Chain2Addr[destChainId],
      selector,
      data,
      hash
    );
  }

```

Let us use this function in our code to send a request to another chain. When this function is called, we specify the destination chainID and the value to be set there. Note that the function interface uses a modifier **`isSelf`** which tells the code that it is the contract itself that can call the **`setValue`** function. It has been defined as an external function because even though the call is made by the contract itself, the call is external (from another chain).

{% hint style="info" %}
Make sure to use the **`isSelf` ** modifier on all functions that can be called across chains.
{% endhint %}

```solidity
  function ExternalSetValue(uint8 _chainID , uint256 _value) public returns(bool) 
  {
	   // Encoding the data to send
        bytes memory data = abi.encode( _value);
	   // The function interface to be called on the destination chain 
        bytes4 _interface = bytes4(keccak256("SetValue(uint256)"));
        // ChainID, Selector, Data, Gas Usage(let gas = 1000000), Gas Price(1000000000)
        bool success = routerSend(_chainID, _interface, data, 1000000, 1000000000);
        return success;
  }
 
  function SetValue( uint256 _value ) external isSelf {
        value = _value;
  }

```

### 2) routerSync function

This is an internal function in the RouterCrossTalk contract that can be called only by the generic handler when a cross-chain message is passed. For instance, if a contract from the Polygon chain sends a message to a linked contract on the Fantom chain, the generic handler will call the **`routerSync`** function on the Fantom. You do not have to worry about the execution of this function. The generic handler calls this function on its own.

It uses two modifiers:

* **isHandler modifier:** Checks whether or not the generic handler is calling the function.
* **isLinkSync modifier:** Checks if the contracts are appropriately linked.

**How to use this function?**

| Parameter  | Description                                                                      |
| ---------- | -------------------------------------------------------------------------------- |
| srcChainID | Chain ID of the source chain                                                     |
| srcAddress | Source chain contract address                                                    |
| data       | The selector and the data to be called on that selector in the destination chain |

This function then calls the **`_routerSyncHandler`** function defined in the RouterCrossTalk contract (which will execute the logic provided by the user).

```solidity
  /// @notice _hash This is internal function to generate the hash of all data sent or received by the contract.
  /// @param _destChainId Source ChainID.
  /// @param _nonce Nonce.
  function _hash(uint8 _destChainId, uint64 _nonce)
    internal
    pure
    returns (bytes32)
  {
    return keccak256(abi.encode(_destChainId, _nonce));
  }
```

```solidity
   function routerSync(
    uint8 srcChainID,
    address srcAddress,
    bytes memory data
  )
    external
    override
    isLinkSync(srcChainID, srcAddress)
    isHandler
    returns (bool, bytes memory)
  {
    uint8 cid = handler.fetch_chainID();
    (bytes4 _selector, bytes memory _data) = abi.decode(data, (bytes4, bytes));
 
    (bool success, bytes memory _returnData) = _routerSyncHandler(
      _selector,
      _data
    );
    emit CrossTalkReceive(srcChainID, cid, srcAddress);
    return (success, _returnData);
  }
```

### 3) \_routerSyncHandler function

It is an internal virtual function that is defined in the RouterCrossTalk contract but whose logic has to be overridden by the users according to what functionality the user might want when a cross-chain request is received from another chain.

**How to use this function?**

| Parameter  | Description                                                                          |
| ---------- | ------------------------------------------------------------------------------------ |
| \_selector | The function selector that needs to be called when a cross-chain request is received |
| \_data     | The encoded data that has to be decoded in this function in order to be used         |

```solidity
 function _routerSyncHandler( bytes4 _selector , bytes memory _data ) 
   internal virtual returns ( bool , bytes memory );
```

This function takes the function selector of the function that will be called when a cross-chain request is received via the **`routerSync`** function. Let’s create a **`_routerSyncHandler`** function for our contract.

```solidity
// Interface is: bytes4(keccak256("SetValue(uint256)"))
// Data is: abi.encode( _value) which was sent by routerSend Function
    function _routerSyncHandler(
        bytes4 _interface ,
        bytes memory _data
        ) internal virtual override  returns ( bool , bytes memory )
    {
 (uint256 _v) = abi.decode(_data, ( uint256 ));
      (bool success, bytes memory returnData) = 
           address(this).call(abi.encodeWithSelector(_interface, _v) );
      return (success, returnData);
    }
```

If there is more than one cross-chain function, you can use **`if`** statements to identify the appropriate decoding of data according to the function parameters. For example, if there are two cross-chain functions - SetA and SetB, and both take different kinds of data as parameters, you can use the template below to decode it and use it.

```solidity
function _routerSyncHandler(
        bytes4 _interface ,
        bytes memory _data
        ) internal virtual override  returns ( bool , bytes memory )
    {
        if( bytes4(keccak256("SetA(uint256)")) == _interface ) {
            (uint256 _v) = abi.decode(_data, ( uint256 ));
            (bool success, bytes memory returnData) = 
          address(this).call( abi.encodeWithSelector(_interface, _v) );
            return (success, returnData);
        }
 
 
       else if ( bytes4(keccak256("SetB(uint64)")) == _interface ) {
            (uint64 _v) = abi.decode(_data, ( uint64 ));
            (bool success, bytes memory returnData) = 
         address(this).call( abi.encodeWithSelector(_interface, _v) );
            return (success, returnData);
        }
    }
```

### 4) routerReplay function

It is an internal function used to execute a transaction that could not be completed on the destination chain due to insufficient gasLimit or gasPrice provided in the **`routerSend`** function.

**How to use this function?**

| Parameter  | Description                                                                                           |
| ---------- | ----------------------------------------------------------------------------------------------------- |
| hash       | The hash returned from the **`routerSend`** function                                                  |
| \_gasLimit | This gas limit has to be higher than or equal to the amount provided in the **`routerSend`** function |
| \_gasPrice | This gas price has to be higher than the amount provided in the **`routerSend`** function             |

```solidity
 function routerReplay(
    bytes32 hash,
    uint256 _gasLimit,
    uint256 _gasPrice
  ) internal {
    handler.replayGenericDeposit(
      executes[hash].chainID,
      executes[hash].nonce,
      _gasLimit,
      _gasPrice
    );
  }
```

Let's create a replay function for our transactions.

```solidity
function replayTransaction(
        bytes32 hash,
        uint256 _crossChainGasLimit,
        uint256 _crossChainGasPrice
    ) external {
        routerReplay(
            hash,
            _crossChainGasLimit,
            _crossChainGasPrice
        );
    }
```

Finally, our whole code is ready, and we can now deploy it on different chains and link those contracts. The final code can be found on the next [page](end-to-end-contract.md).
