# Using CrossTalk to Mint NFTs Across Chains

In this guide, we'll show you how to use Router's CrossTalk library to mint NFTs across chains.

**Purpose:** Users can pay on any chain for minting the NFT; however, the NFT will be minted to them only on the specified chain. In other words, the minting fee can be taken on any chain, but the minting shall happen only on the specified chain.

As we are all aware, with ERC-721, for one token ID, only one NFT can exist. Hence, to give NFTs cross-chain capabilities while avoiding multiples of instances of the same token ID on different chains, we mint NFTs only on one chain. However, we ensure that the minting fee can be collected on any chain. This workflow gave birth to the concepts of Fee Chains and Minting Chain.

Also after the NFTs are minted on the Minting Chain, it can be transferred to other chains. This  is achieved by the following mechanism:&#x20;

1. Minting Chain to Fee Chain transfer: Lock NFT on the source chain and mint on the destination chain.
2. Fee Chain to Minting Chain transfer: Burn NFT on the source chain and unlock NFT on the destination chain.
3. Fee Chain to Fee Chain transfer: Burn NFT on the source chain and mint on the destination chain.

In this guide, we'll take a look at how we can achieve this using our ERC721-minting-libraries. This guide is divided into two sections -

* Steps that need to be followed on the fee chain (chain where the fee for minting NFTs can be collected):

{% content-ref url="on-fee-chain/" %}
[on-fee-chain](on-fee-chain/)
{% endcontent-ref %}

* Steps that need to be followed on the minting chain (chain where NFTs can be minted):

{% content-ref url="on-minting-chain/" %}
[on-minting-chain](on-minting-chain/)
{% endcontent-ref %}
