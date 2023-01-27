# End-to-end Contract

```solidity
// SPDX-License-Identifier: MIT
  pragma solidity ^0.8.0;
  import "@routerprotocol/router-crosstalk/contracts/nonupgradeable/RouterCrossTalk.sol";
 
  contract MyContract is RouterCrossTalk {
    uint256 public value;
    address public owner;
    constructor ( address _genericHandler ) RouterCrossTalk(_genericHandler){
        owner = msg.sender;
    }
   
    modifier onlyOwner(){
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }
 
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
 
 
    function _routerSyncHandler(
        bytes4 _interface ,
        bytes memory _data
        ) internal virtual override  returns ( bool , bytes memory )
    {
            (uint256 _v) = abi.decode(_data, ( uint256 ));
            (bool success, bytes memory returnData) =
                address(this).call( abi.encodeWithSelector(_interface, _v) );
            return (success, returnData);
    }
    
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
 
    function SetValue( uint256 _value ) external isSelf  {
        value = _value;
    }
 
   
   function setLinker ( address _linker ) external onlyOwner()  {
        setLink(_linker);
    }
 
    function setFeeAddress ( address _feeAddress ) external onlyOwner() {
        setFeeToken(_feeAddress);
    }
	
   function approveFee(address _feeToken, uint256 _value) external{
     approveFees(_feeToken, _value);
   }
 
}

```
