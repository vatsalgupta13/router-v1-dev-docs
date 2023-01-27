---
description: How Sequencer Crosstalk Library functions internally
---

# Workflow

To facilitate cross-chain sequencing, Router's Sequencer CrossTalk library interfaces with the following components of Router Protocol:

### 1) Sequencer Handler&#x20;

For any cross-chain sequencing, sequencer handlers are the only piece of code that interacts with the application. Sequencer handler is used for the following tasks:

* Mapping and unmapping cross-chain contracts&#x20;
* Handle cross-chain requests (both send and sync)
* A flag is used to sequence between token transfers and generic calls

### 2) Bridge Contract&#x20;

The bridge contract is responsible for:

* creating a record of all the incoming cross-chain transactions
* executing all outgoing cross-chain transactions based on the voting process

### 3) Relayer/Validator Module

Relayer module is a GoLang application that constantly listens for incoming deposit records across multiple chains and performs validation and execution of proposals on the specified destination chain.&#x20;
