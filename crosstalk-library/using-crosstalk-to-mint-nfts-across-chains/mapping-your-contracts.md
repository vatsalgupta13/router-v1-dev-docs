# Mapping your contracts

After deployment of your contracts on all the chains, you need to map the contracts on various fee chains to the contract on the minting chain and vice-versa. For this, we have already created a task in the **tasks** folder of the root directory. To run this task, please run the following command:

`npx hardhat MAP_CONTRACTS --network <network> --router-chainid <router chain id*> --actual-chainid <actual chain id*>`

Note: \* router chain id and actual chain id are the chain ids for the destination chain( While mapping the fee chain with the minting chain, the router chain id and actual chain id shall be supplied for the minting chain and the network is to be the fee chain. Reverse this while mapping the minting chain with the fee chain). The router chain ids for different chains can be found [here](../../important-parameters/supported-chains.md).

For example: If we want to map our contract on Polygon chain with the contract on BSC chain, we would run the following command:

`npx hardhat MAP_CONTRACTS --network polygon --router-chainid 2 --actual-chainid 56`
