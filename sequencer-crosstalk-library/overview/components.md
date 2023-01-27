# Components

Router's Sequencer CrossTalk contract is a standardized library that can be inherited into any smart contract to make them truly cross-chain and enable sequencing of token transactions and generic calls.

### 1) System Components&#x20;

* **Sequencer handler (address of the sequencer handler of the specific chain)**&#x20;
* **ERC-20 handler(address of the ERC-20 handler of the specific chain)**

### 2) Administrative components&#x20;

* **linkSetter**

linkSetter is an administrative address set up by the owners of the cross-chain contract. linkSetter address can link contracts from different chains to the current chain. In order to link/unlink contracts from other chains using the linker address, users must use MapContract and UnMapContract in the sequencer handler of the respective chain.&#x20;

{% hint style="info" %}
**Note:** Router's Sequencer CrossTalk contracts work on the principle of rule-based access control, which implies that contracts that are verified can only send sync requests and the linkSetter address has the ability to map and unmap contracts.
{% endhint %}

### 3) Operational Components

* **routerSend**

routerSend is an internal function that creates a cross-chain request to the remote chain. It takes a struct of required parameters as an argument to proceed with the communication. (more details about the struct in upcoming sections)

* **\_routerSyncHandler**

\_routerSyncHandler is the function that will include the business logic of the cross-chain application that you are building. By default the function is blank and it needs to be altered as per your needs. Note that in this function, you just have to handle the logic for your generic call. The token transfer is handled by ERC-20 handler itself.

* **routerReplay**

This function is used to execute a transaction which could not be completed on the destination chain due to insufficient gasLimit or gasPrice provided in the routerSend function called on the source chain.

****
