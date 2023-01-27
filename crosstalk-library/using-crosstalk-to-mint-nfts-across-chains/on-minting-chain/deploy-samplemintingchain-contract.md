# Deploy SampleMintingChain contract

There is a **deployments** folder in the root directory. Just create a new file **deployments.json** populate it according to the **sampleDeployments.json** file. After this, run the following command.

`npx hardhat DEPLOY_MINTINGCHAIN --network <network>`&#x20;

This will deploy the contract, set the linker address, the fee token address(for handler), the cross-chain gas limit, the fee token for NFT, the fee amount per NFT and also approve the fee for handler from the contract. This will also store the deployment address of this contract in the **deployments.json** file.

**Note**: Please modify the tasks according to the functionality of your contract. The deployment method stated here will work with the sample contracts already provided in the library.
