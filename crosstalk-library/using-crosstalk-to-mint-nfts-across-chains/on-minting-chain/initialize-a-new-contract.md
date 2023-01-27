# 2⃣ Initialize a New Contract

We initialize the contract by passing the constructor arguments **`“name_”`, `“symbol_”`, `“MaxTokenId_”`,** and ** `“genericHandler_”`**.

* **name & symbol:** Name and symbol for your NFT.
* **MaxTokenId:** Maximum number of NFTs that can be minted. Note that **maxTokenId** is an immutable variable in our library.
* **genericHandler:** It refers to the address of the generic handler for the chain you are deploying your contract on. Address of the generic handler for different chains can be found [here](../../deployment-addresses/generic-handler-addresses.md).

```solidity
contract SampleMintingChain is OnMintingChain {
  address public owner;

  constructor(
    string memory name_,
    string memory symbol_,
    uint256 MaxTokenId_,
    address genericHandler_
  ) OnMintingChain(name_, symbol_, MaxTokenId_, genericHandler_) {
    owner = msg.sender;
  }

```
