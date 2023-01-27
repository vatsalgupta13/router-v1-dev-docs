---
description: How Router CrossTalk library enables cross-chain communication
---

# Workflow

To facilitate cross-chain messaging, Router's CrossTalk library interfaces with the following components of Router Protocol:

### 1) Generic Handler&#x20;

For any cross-chain message transfer, generic handlers are the only piece of code that interacts with the application. Generic handler is used for the following tasks:

* Mapping and unmapping cross-chain contracts&#x20;
* Handle cross-chain requests (both send and sync)

### 2) Bridge Contract&#x20;

The bridge contract is responsible for:

* creating a record of all the incoming cross-chain transactions
* executing all outgoing cross-chain transactions based on the voting process

### 3) Relayer/Validator Module

Relayer module is a GoLang application that constantly listens for incoming deposit records across multiple chains and performs validation and execution of proposals on the specified destination chain.&#x20;

## Working Diagram

![How the CrossTalk library leverages Router's infrastructure to enable cross-chain communication](../../.gitbook/assets/0\_0.png)
