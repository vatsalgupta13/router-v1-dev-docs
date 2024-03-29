# Table of contents

* [🛠 Build with Router Protocol](README.md)

## Asset-swap API

* [Overview](asset-swap-api/overview.md)
* [Performing a Cross-chain Transaction](asset-swap-api/performing-a-cross-chain-transaction/README.md)
  * [1⃣ Request a Quote](asset-swap-api/performing-a-cross-chain-transaction/request-a-quote/README.md)
    * [Get Fee Details](asset-swap-api/performing-a-cross-chain-transaction/request-a-quote/get-fee-details.md)
  * [2⃣ Check and Set the Allowance](asset-swap-api/performing-a-cross-chain-transaction/check-and-set-the-allowance/README.md)
    * [Reserve Token Handler Addresses](asset-swap-api/performing-a-cross-chain-transaction/check-and-set-the-allowance/reserve-token-handler-addresses.md)
  * [3⃣ Execute the Transaction](asset-swap-api/performing-a-cross-chain-transaction/execute-the-transaction.md)
  * [4⃣ Check the Status](asset-swap-api/performing-a-cross-chain-transaction/check-the-status.md)
* [End-to-end Code Snippet](asset-swap-api/end-to-end-code-snippet.md)

## JS SDK

* [Overview](js-sdk/overview.md)
* [Installing and Initializing Router SDK](js-sdk/installing-and-initializing-router-sdk.md)
* [Performing a Cross-chain Transaction](js-sdk/performing-a-cross-chain-transaction/README.md)
  * [1⃣ Request a Quote](js-sdk/performing-a-cross-chain-transaction/request-a-quote/README.md)
    * [Fetching Bridge Fee](js-sdk/performing-a-cross-chain-transaction/request-a-quote/fetching-bridge-fee.md)
  * [2⃣ Check and Set the Allowance](js-sdk/performing-a-cross-chain-transaction/check-and-set-the-allowance.md)
  * [3⃣ Execute the Transaction](js-sdk/performing-a-cross-chain-transaction/execute-the-transaction.md)
  * [4⃣ Check the Status](js-sdk/performing-a-cross-chain-transaction/check-the-status.md)
* [End-to-end Code Snippet](js-sdk/end-to-end-code-snippet.md)

## Important Parameters

* [Supported Chains](important-parameters/supported-chains.md)
* [Supported Fee Tokens for ERC20 Transactions](important-parameters/supported-fee-tokens-for-erc20-transactions.md)
* [Supported Fee Tokens for CrossTalk](important-parameters/supported-fee-tokens-for-crosstalk.md)
* [Native Assets](important-parameters/native-assets.md)

## Widget

* [Integrating Router's Widget](widget/integrating-routers-widget.md)
* [Projects with Router Widget](widget/projects-with-router-widget.md)

## CrossTalk Library

* [Overview](crosstalk-library/overview/README.md)
  * [Introduction](crosstalk-library/overview/introduction.md)
  * [Workflow](crosstalk-library/overview/workflow.md)
  * [Components](crosstalk-library/overview/components.md)
* [Getting Started](crosstalk-library/getting-started/README.md)
  * [1⃣ Install Router CrossTalk](crosstalk-library/getting-started/install-router-crosstalk.md)
  * [2⃣ Write your First Cross-chain Contract](crosstalk-library/getting-started/write-your-first-cross-chain-contract/README.md)
    * [Various Cross-chain Functions](crosstalk-library/getting-started/write-your-first-cross-chain-contract/various-cross-chain-functions.md)
    * [End-to-end Contract](crosstalk-library/getting-started/write-your-first-cross-chain-contract/end-to-end-contract.md)
  * [3⃣ Deploy your First Cross-chain Contract](crosstalk-library/getting-started/deploy-your-first-cross-chain-contract.md)
* [Cross-chain NFTs using CrossTalk](crosstalk-library/cross-chain-nfts-using-crosstalk/README.md)
  * [1⃣ Import Required Contracts](crosstalk-library/cross-chain-nfts-using-crosstalk/import-required-contracts.md)
  * [2⃣ Initialize a New Contract](crosstalk-library/cross-chain-nfts-using-crosstalk/initialize-a-new-contract.md)
  * [3⃣ Setting Administrative functions](crosstalk-library/cross-chain-nfts-using-crosstalk/setting-administrative-functions.md)
  * [4⃣ \_sendCrossChain Function](crosstalk-library/cross-chain-nfts-using-crosstalk/\_sendcrosschain-function.md)
  * [5⃣ \_routerSyncHandler Function](crosstalk-library/cross-chain-nfts-using-crosstalk/\_routersynchandler-function.md)
  * [6⃣ receiveCrossChain Function](crosstalk-library/cross-chain-nfts-using-crosstalk/receivecrosschain-function.md)
  * [7⃣ replayTransferCrossChain Function](crosstalk-library/cross-chain-nfts-using-crosstalk/replaytransfercrosschain-function.md)
* [Using CrossTalk to Mint NFTs Across Chains](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/README.md)
  * [On Fee Chain](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/README.md)
    * [1⃣ Import Required Contracts](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/import-required-contracts.md)
    * [2⃣ Initialize a New Contract](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/initialize-a-new-contract.md)
    * [3⃣ Understanding Administrative Functions](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/understanding-administrative-functions.md)
    * [4⃣ mintCrossChain Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/mintcrosschain-function.md)
    * [5⃣ transferCrossChain Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/transfercrosschain-function.md)
    * [6⃣ receiveCrossChain Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/receivecrosschain-function.md)
    * [8⃣ replayTransaction function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/replaytransaction-function.md)
    * [9⃣ withdrawFeeTokenForNFT Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/withdrawfeetokenfornft-function.md)
    * [End-to-end Sample Fee Chain Contract](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/end-to-end-sample-fee-chain-contract.md)
    * [Deploy SampleFeeChain contract](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-fee-chain/deploy-samplefeechain-contract.md)
  * [On Minting Chain](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/README.md)
    * [1⃣ Import Required Contracts](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/import-required-contracts.md)
    * [2⃣ Initialize a New Contract](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/initialize-a-new-contract.md)
    * [3⃣ Understanding Administrative Functions](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/understanding-administrative-functions.md)
    * [4⃣ transferCrossChain Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/transfercrosschain-function.md)
    * [5⃣ receiveMintCrossChain Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/receivemintcrosschain-function.md)
    * [6⃣ receiveCrossChain function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/receivecrosschain-function.md)
    * [7⃣ mintSameChain Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/mintsamechain-function.md)
    * [8⃣ replayTransaction Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/replaytransaction-function.md)
    * [9⃣ withdrawFeeTokenForNFT Function](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/withdrawfeetokenfornft-function.md)
    * [End-to-end Sample Minting Chain Contract](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/end-to-end-sample-minting-chain-contract.md)
    * [Deploy SampleMintingChain contract](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/on-minting-chain/deploy-samplemintingchain-contract.md)
  * [Mapping your contracts](crosstalk-library/using-crosstalk-to-mint-nfts-across-chains/mapping-your-contracts.md)
* [OpenZeppelin Contracts using CrossTalk](crosstalk-library/openzeppelin-contracts-using-crosstalk.md)
* [Handling Fees](crosstalk-library/handling-fees.md)
* [Security Considerations](crosstalk-library/security-considerations.md)
* [Examples](crosstalk-library/examples/README.md)
  * [Cross-chain ERC20](crosstalk-library/examples/cross-chain-erc20.md)
  * [Cross-chain ERC1155](crosstalk-library/examples/cross-chain-erc1155.md)
* [Deployment Addresses](crosstalk-library/deployment-addresses/README.md)
  * [Generic Handler Addresses](crosstalk-library/deployment-addresses/generic-handler-addresses.md)
  * [Fee Token Addresses](crosstalk-library/deployment-addresses/fee-token-addresses.md)
* [Security Audit](crosstalk-library/security-audit.md)
* [CrossTalk NPM Library](crosstalk-library/crosstalk-npm-library.md)

## SEQUENCER CROSSTALK LIBRARY

* [Overview](sequencer-crosstalk-library/overview/README.md)
  * [Introduction](sequencer-crosstalk-library/overview/introduction.md)
  * [Workflow](sequencer-crosstalk-library/overview/workflow.md)
  * [Components](sequencer-crosstalk-library/overview/components.md)
* [Getting Started](sequencer-crosstalk-library/getting-started/README.md)
  * [1⃣ Install Router Crosstalk](sequencer-crosstalk-library/getting-started/install-router-crosstalk.md)
  * [2⃣ Write Vault and Stake Contracts](sequencer-crosstalk-library/getting-started/write-vault-and-stake-contracts/README.md)
    * [Vault Contract](sequencer-crosstalk-library/getting-started/write-vault-and-stake-contracts/vault-contract/README.md)
      * [How to get data for routerSend function?](sequencer-crosstalk-library/getting-started/write-vault-and-stake-contracts/vault-contract/how-to-get-data-for-routersend-function.md)
      * [End-to-end Vault contract](sequencer-crosstalk-library/getting-started/write-vault-and-stake-contracts/vault-contract/end-to-end-vault-contract.md)
    * [Stake Contract](sequencer-crosstalk-library/getting-started/write-vault-and-stake-contracts/stake-contract.md)
  * [3⃣ Deploy and map your contracts](sequencer-crosstalk-library/getting-started/deploy-and-map-your-contracts.md)
* [Deployment Addresses](sequencer-crosstalk-library/deployment-addresses/README.md)
  * [Sequencer Handler Addresses](sequencer-crosstalk-library/deployment-addresses/sequencer-handler-addresses.md)
  * [ERC-20 Handler Addresses](sequencer-crosstalk-library/deployment-addresses/erc-20-handler-addresses.md)
  * [Reserve Handler Addresses](sequencer-crosstalk-library/deployment-addresses/reserve-handler-addresses.md)
  * [Onesplit Addresses](sequencer-crosstalk-library/deployment-addresses/onesplit-addresses.md)
