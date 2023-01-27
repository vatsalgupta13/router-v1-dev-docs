# Security Considerations

While developing cross-chain applications using Router's CrossTalk library, there are certain security requirements that need to be adhered to in order to ensure secure cross-chain communication:

* Ensure that the setup of the linker address and the fee token address is under the control of the owner.&#x20;
* Ensure that the sync functions within the contract have the **`isSelf`** Modifier set.
* Ensure that **`_routerSyncHandler`** has addressed all the sync functions.
* Ensure that the correct amount of gas is specified in **`routerSend`** functions.
* Ensure that the source contract address has enough amount of fee token balance and has given the required spending approval to the generic handler.&#x20;
