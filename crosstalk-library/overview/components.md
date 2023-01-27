# Components

Router's CrossTalk contract is a standardized contract that can be inherited into any smart contract to make them truly cross-chain.&#x20;

### 1) System Components&#x20;

* **Generic handler (address of the generic handler of the specific chain)**&#x20;

### 2) Administrative components&#x20;

* **linkSetter**

linkSetter is an administrative address set up by the owners of the cross-chain contract. linkSetter address can link contracts from different chains to the current chain. In order to link/unlink contracts from other chains using the linker address, users must use MapContract and UnMapContract in the generic handler of the respective chain.&#x20;

{% hint style="info" %}
**Note:** Router's CrossTalk contracts work on the principle of rule-based access control, which implies that contracts that are verified can only send sync requests and the linkSetter address has the ability to map and unmap contracts.
{% endhint %}

### 3) Operational Components

* **routerSend**

routerSend is an internal function that creates a cross-chain request to the remote chain specifying the selector and the data to be moved to the remote chain.

* **routerSync**

routerSync is another internal function that handles all incoming cross-chain requests to the cross-chain contract on any chain. In order to interface with the routerSync contract, the source contract must be linked to it via linkSetter.&#x20;

* **\_routerSyncHandler**

\_routerSyncHandler is the function that will include the business logic of the cross-chain application that you are building. By default the function is blank and it needs to be altered as per your needs.

* **routerReplay**

This function is used to execute a transaction which could not be completed on the destination chain due to insufficient gasLimit or gasPrice provided in the routerSend function called on the source chain.
