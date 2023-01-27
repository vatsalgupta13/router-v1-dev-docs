# Handling Fees

Fees are calculated on basis of the following factors:&#x20;

1. Amount of gas allotted to a **`routerSend`** function
2. Fee token address specified in the contract
3. Fee factor of fee token address

The fee is calculated based on the following formula:&#x20;

$$
fees = Gas * FeeFactor ^T  + BridgeFee^T
$$

### Gas&#x20;

Gas allowed for the specified cross-chain function. Gas has to be specified by the user while writing cross-chain contracts. Ideally, the gas to be spent for any function should be kept configurable.

### Fee Token&#x20;

The fee token defines the token in which the gas costs will be paid. The fees are deducted on the source side and hence the source contract must have the defined amount of fee token present within the contract to execute the cross-chain contract call.  &#x20;

### Fee Factor&#x20;

The fee factor defines the base gas price for the cross-chain transaction and is set administratively for each token. It is important to note that the fee factor can change based on the fluctuation in token prices.

### Bridge Fee&#x20;

The bridge fee defines the base fees in tokens charged by the bridge for the cross-chain transaction. The bridge fee is the fixed component of any cross-chain contract call. &#x20;
