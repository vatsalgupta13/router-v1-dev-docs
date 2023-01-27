# 1⃣ Import Required Contracts

We start by cloning the ERC-721 Minting repository from here: [https://github.com/router-protocol/ERC721-Cross-Chain-Minting.git](https://github.com/router-protocol/ERC721-Cross-Chain-Minting.git) which contains **OnFeeChain** library that will be used to create our **SampleFeeChain** contract. Let's start creating our **SampleFeeChain** contract by first importing **OnFeeChain** contract into it.&#x20;

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

import "../FeeChain.sol";
```
