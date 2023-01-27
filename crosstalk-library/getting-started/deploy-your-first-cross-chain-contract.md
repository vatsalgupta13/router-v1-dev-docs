# 3⃣ Deploy your First Cross-chain Contract

Now that we are done writing our first cross-chain contract using Router's CrossTalk library, we can safely deploy them on different blockchains.&#x20;

{% hint style="info" %}
Make sure that you use the correct generic handler addresses while deploying contracts onto different chains. The list of generic handler addresses has been given [here](../deployment-addresses/generic-handler-addresses.md).
{% endhint %}

After you have deployed the contract on different chains, you must be wondering how these contracts know which contract is their counterpart on other blockchains. The solution lies in **linking** all the contracts on different chains.

To set up the linker address and the fee token address, you have to call the **`setLinker`** function, **`setFeeAddress,`** and **`approveFee`** function on your contract from the owner address on all chains.&#x20;

To simplify this process for the developers, we have included some hardhat tasks which can be used to configure the linker & fee token addresses directly and then map your contracts.

```javascript
import { task, types } from 'hardhat/config'
 
task('TASK_CONFIG_SET_LINKER', 'Set Linker Address')
  .addParam<string>('contractName', 'Contract Name', '', types.string)
  .addParam<string>('contractAddress', 'Contract Address', '', types.string)
  .addParam<string>('linkerAddress', 'Linker Address', '', types.string)
  .setAction(async (taskArgs, { ethers }): Promise<null> => {
    const C1 = await ethers.getContractFactory(taskArgs.contractName)
    const C1i = await C1.attach(taskArgs.contractAddress)
    await C1i.setLinker(taskArgs.linkerAddress)
    console.log('Linker Set')
    return null
  })
 
task('TASK_CONFIG_SET_FEETOKEN', 'Set FeeToken for Hurricane')
  .addParam<string>('contractName', 'Contract Name', '', types.string)
  .addParam<string>('contractAddress', 'Contract Address', '', types.string)
  .addParam<string>('feeToken', 'Fee Token Address', '', types.string)
  .setAction(async (taskArgs, { ethers }): Promise<null> => {
    const C1 = await ethers.getContractFactory(taskArgs.contractName);
    const C1i = await C1.attach(taskArgs.contractAddress);
    await C1i.setFeeTokens(taskArgs.feeToken);
    await C1i.approveFees(taskArgs.feeToken, "1000000000000000000000000");
    console.log('Fee Token Set and Approved');
    return null;
  })
  
task(TASK_APPROVE_FEES, "Approves the fees")
  .addParam<string>('contractName', 'Contract Name', '', types.string)
  .addParam<string>('contractAddress', 'Contract Address', '', types.string)
  .addParam<string>('feeToken', 'Fee Token Address', '', types.string)
  .setAction(async (taskArgs, hre): Promise<null> => {
    const C1 = await ethers.getContractFactory(taskArgs.contractName);
    const C1i = await C1.attach(taskArgs.contractAddress);
    await C1i.approveFee(taskArgs.feeToken, "1000000000000000000000000");
    console.log(`Fee approved`);
    return null;
  });
 
// chainid = Destination Chain IDs defined by Router.
// nchainid = Actual Destination Chain ID
// The router defined chain ids can be found at:
// https://dev.routerprotocol.com/important-parameters/supported-chains
 
task("TASK_MAP_CONTRACT", "Map Contracts")
  .addParam<string>("chainid", "Remote ChainID (Router Specs)", "", types.string)
  .addParam<string>("srcHandler", "Generic handler address on src chain", "",   types.string)
  .addParam<string>("srcContract", "Contract address on src chain", "", types.string)
  .addParam<string>("destContract", "Contract address on dest chain", "", types.string)
  .setAction(async (taskArgs, hre): Promise<null> => {
    const handlerABI = require("../build/contracts/genericHandler.json");
 
    const handlerContract: Contract = await hre.ethers.getContractAt(
      handlerABI,
      taskArgs.srcHandler
    );
    await handlerContract.MapContract(
      [
        taskArgs.srcContract,
        taskArgs.chainid,
        taskArgs.destContract
      ]
    );
    console.log("Mapping Done");
    return null;
  });
```

You just have to run these tasks on each network to automatically (a) configure the linker & fee token address and (b) map your contracts on different chains.
