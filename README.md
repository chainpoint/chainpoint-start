# Important: This version of the Chainpoint Network is currently operating in a private testnet.
Please [submit an application](https://forms.gle/YNVM8FZXe5dWdEuf7) if you'd like to participate and provide feedback. The information below is subject to change prior to mainnet launch.

This repo provides an overview of Chainpoint and links to additional resources.

## Chainpoint Overview

[Chainpoint](https://github.com/chainpoint) is a protocol for anchoring data to Bitcoin to generate a timestamp proof. 

The **Chainpoint Network** is a globally distributed network of nodes. Clients submit hashes to Gateways, which aggregate these hashes into a [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree). Cores periodically publish the root of this tree in a Bitcoin transaction. The final **Chainpoint Proof** contains a series of operations that cryptographically link data to the Bitcoin blockchain.

## Chainpoint Network
![Draft Chainpoint Architecture Diagram](https://github.com/chainpoint/chainpoint-start/blob/master/imgs/Chainpoint-Network-Overview-Diagram.png)


The Chainpoint Network is composed of a hierarchy of aggregators that coordinate to create Chainpoint Proofs. Here’s an overview of the process:


1. Clients submit hashes to Gateways. 
2. Gateways aggregate hashes in a Merkle tree, and send a Merkle root to one or more Cores. 
3. Cores perform an additional round of aggregation, and write Merkle roots in transactions on a [Tendermint](https://github.com/tendermint/tendermint)-based blockchain called the Calendar. 
4. Every hour, a single Core is elected to anchor the state of the Calendar to Bitcoin, monitor the blockchain for [six confirmations](https://en.bitcoin.it/wiki/Confirmation), and write the results back to the Calendar. 
5. Gateways and Clients use the new data from the Calendar to construct final Chainpoint Proofs. 

Due to the characteristics of Bitcoin, this asynchronous process takes approximately 90 minutes.


# Clients
## [chainpoint-js](https://github.com/chainpoint/chainpoint-client-js)

A javascript client to submit hashes to Gateways, retrieve final Chainpoint Proofs, and verify proofs.

## [chainpoint-cli](https://github.com/chainpoint/chainpoint-cli)

A Command Line Interface (CLI) for creating and verifying Chainpoint proofs.


# Gateways
## [chainpoint-gateway](http://github.com/chainpoint/chainpoint-Gateway)

Gateways receive hashes from Clients, aggregate hashes in a [merkle tree](https://en.wikipedia.org/wiki/Merkle_tree), and periodically send a merkle root to one or more Cores. Each Gateway has an integrated Lightning Node running [LND](https://github.com/lightningnetwork/lnd_). Gateways use Lighting to pay Cores an Anchor Fee for submitting a merkle root. 


# Core
## [chainpoint-core](http://github.com/chainpoint/chainpoint-core)

Each Core is a member of a distributed network that uses [Tendermint](https://github.com/tendermint/tendermint) to reach consensus. Cores aggregate hashes received from Gateways, maintain the Chainpoint Calendar, and periodically anchor data to the Bitcoin blockchain. Each Core has an integrated Lightning Node running [LND](https://github.com/lightningnetwork/lnd). Cores receive Anchor Fee payments from Gateways via Lightning. The default Anchor Fee is 2 [satoshis](https://en.bitcoin.it/wiki/Satoshi_(unit)). Core Operators can set their Anchor Fee to adapt to changing market conditions.

To join the network, Cores must open Lightning channels with 2/3rds of the existing Cores. Each channel must have a minimum capacity of 1,000,000 satoshis. This provides a measure of Sybil resistance and helps ensure Cores have the liquidity to receive Lightning payments from Gateways. As more Cores join the network, each Core anchors less frequently. This reduces the burden of paying Bitcoin transaction fees.

# Components

The components of the Chainpoint Network and their dependencies are below.


[Chainpoint Core](https://github.com/chainpoint/chainpoint-core/blob/master/README.md)  
| -- [Tendermint Core](https://github.com/chainpoint/tendermint)  
| -- [Lightning Network Daemon](https://github.com/Tierion/lnd)  
| -- [btc-bridge](https://github.com/Tierion/btc-bridge)  
|&nbsp; &nbsp; | -- [lnrpc-node-client](https://github.com/Tierion/lnrpc-node-client)  

[Chainpoint Gateway](https://github.com/chainpoint/chainpoint-gateway)  
| -- [Lightning Network Daemon](https://github.com/Tierion/lnd)  
| -- [lnrpc-node-client](https://github.com/Tierion/lnrpc-node-client)  
| -- [merkle-tools](https://github.com/Tierion/merkle-tools)  
| -- [chainpoint-parse](https://github.com/chainpoint/chainpoint-parse)  
|&nbsp; &nbsp; | -- [chainpoint-binary](https://github.com/chainpoint/chainpoint-binary)  
|&nbsp; &nbsp; | -- [chainpoint-proof-json-schema](https://github.com/chainpoint/chainpoint-proof-json-schema)  

[Chainpoint CLI](https://github.com/chainpoint/chainpoint-cli)  
| -- [chainpoint-js](https://github.com/chainpoint/chainpoint-js)  
|&nbsp; &nbsp; | -- [chainpoint-parse](https://github.com/chainpoint/chainpoint-parse)  
|&nbsp; &nbsp; |&nbsp; &nbsp; | -- [chainpoint-binary](https://github.com/chainpoint/chainpoint-binary)  
|&nbsp; &nbsp; |&nbsp; &nbsp; | -- [chainpoint-proof-json-schema](https://github.com/chainpoint/chainpoint-proof-json-schema) 

# Versions

Stable releases will be tagged in all relevant repos listed in the Components section of this Readme. The `master` branch for these repositories is considered stable. 

# Legacy Software

For the legacy network using the Tierion Network Token, please see the [TNT-Legacy repositories](https://github.com/tnt-legacy). 

# Important Links
- [Chainpoint Website](https://chainpoint.org)
- [Chainpoint JSON Proof Schema](https://chainpoint.org/contexts/chainpoint-v4.jsonld)
- [Boltbox - Lightning Infrastructure](https://github.com/Tierion/boltbox)

 © 2020 Tierion. All Rights Reserved.
