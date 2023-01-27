# 1⃣ Import Required Contracts

If you have not already cloned the repository, we start by cloning the ERC-721 Minting repository from here: [https://github.com/router-protocol/ERC721-Cross-Chain-Minting.git](https://github.com/router-protocol/ERC721-Cross-Chain-Minting.git) which contains **OnMintingChain** library that will be used to create our **SampleMintingChain** contract. Let's start creating our **SampleMintingChain** contract by first importing **OnMintingChain** contract into it.

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;
import "../MintingChain.sol";
```
