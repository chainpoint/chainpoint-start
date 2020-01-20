# Chainpoint

Chainpoint is a protocol for anchoring data to the Bitcoin blockchain. 
It makes the process more cost-effective by creating intermediate, decentralized tiers between the user and Bitcoin blocks. 
The end result of anchoring is a Chainpoint Proof showing how the user's data was cryptographically included in the blockchain.

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)

## Table of Contents

  * [Architecture](#architecture)
  * [Components](#components)
  * [Getting Started](#getting-started)
      + [Chainpoint Core Node](#chainpoint-core-node)
      + [Chainpoint Client Node](#chainpoint-client-node)
      + [Chainpoint Clients](#chainpoint-clients)
  * [Versions](#versions)
  * [Important Links](#important-links)


## Architecture

The story of bitcoin anchoring begins with users installing [Chainpoint-CLI](https://github.com/chainpoint/chainpoint-cli) or [chainpoint-client-js](https://github.com/chainpoint/chainpoint-client-js) to submit hashes to the _Chainpoint Network_.

The first level of the Chainpoint Network is [Chainpoint Client Node](https://github.com/chainpoint/chainpoint-node-src), which _aggregates_ user-submitted hashes into a single datapoint every minute. 

The resulting datapoint is then submitted to the second tier of Chainpoint, a network of [Chainpoint Core Nodes](https://github.com/chainpoint/chainpoint-core).
The Core Network consists of many Cores running in concert to create a Tendermint-based blockchain called the Calendar. 
Every hour, a random Core is elected to anchor the state of the Calendar to Bitcoin and write the result back to the Calendar. 
When there are more Cores in the network, any given Core will be selected to anchor less frequently. This reduces the individual burden of paying Bitcoin fees.

After a Core anchors to Bitcoin, Chainpoint Client Nodes can retrieve the result and construct a Chainpoint Proof. Because the Bitcoin blockchain is viewable by everyone and secured by Bitcoin mining, writing data to Bitcoin constitutes a reliable form of notarization-
it is an efficient method of proving that user-submitted content existed at a particular point in time.

By default, Cores are members of the [Lightning Network](https://lightning.network/). Client Nodes use Lightning to pay Cores for permission to anchor a hash. Additionally, Lightning is used by new Cores to stake bitcoin to the Chainpoint Network as part of an anti-sybil mechanism. 


## Components

The various components of the Chainpoint Network and their dependencies are broken down below.



[Chainpoint Core](https://github.com/chainpoint/chainpoint-core/blob/master/README.md)  
| -- [Tendermint Core](https://github.com/chainpoint/tendermint)  
| -- [Lightning Network Daemon](https://github.com/Tierion/lnd)  
| -- [btc-bridge](https://github.com/Tierion/btc-bridge)  
|&nbsp; &nbsp; | -- [lnrpc-node-client](https://github.com/Tierion/lnrpc-node-client)  

[Chainpoint Node](https://github.com/chainpoint/chainpoint-node-src)  
| -- [Lightning Network Daemon](https://github.com/Tierion/lnd)  
| -- [lnrpc-node-client](https://github.com/Tierion/lnrpc-node-client)  
| -- [merkle-tools](https://github.com/Tierion/merkle-tools)  
| -- [chainpoint-parse](https://github.com/chainpoint/chainpoint-parse)  
|&nbsp; &nbsp; | -- [chainpoint-binary](https://github.com/chainpoint/chainpoint-binary)  
|&nbsp; &nbsp; | -- [chainpoint-proof-json-schema](https://github.com/chainpoint/chainpoint-proof-json-schema)  

[Chainpoint CLI](https://github.com/chainpoint/chainpoint-cli)  
| -- [chainpoint-client-js](https://github.com/chainpoint/chainpoint-client-js)  
|&nbsp; &nbsp; | -- [chainpoint-parse](https://github.com/chainpoint/chainpoint-parse)  
|&nbsp; &nbsp; |&nbsp; &nbsp; | -- [chainpoint-binary](https://github.com/chainpoint/chainpoint-binary)  
|&nbsp; &nbsp; |&nbsp; &nbsp; | -- [chainpoint-proof-json-schema](https://github.com/chainpoint/chainpoint-proof-json-schema)  

## Getting Started

You can choose how you want to use or support the Chainpoint Network by picking from the three options below.

### Chainpoint Core Node
Download Chainpoint Core to join the Chainpoint Calendar Blockchain, help the network anchor to bitcoin, and potentially earn Bitcoin on lightning from Nodes and clients submitting hashes. Follow the directions below to install and run Core:

```$bash
$ sudo apt-get install make git
$ git clone https://github.com/chainpoint/chainpoint-core.git
$ cd chainpoint-core
$ make install-deps

Please logout and login to allow your user to use docker

$ make init

 ██████╗██╗  ██╗ █████╗ ██╗███╗   ██╗██████╗  ██████╗ ██╗███╗   ██╗████████╗     ██████╗ ██████╗ ██████╗ ███████╗
██╔════╝██║  ██║██╔══██╗██║████╗  ██║██╔══██╗██╔═══██╗██║████╗  ██║╚══██╔══╝    ██╔════╝██╔═══██╗██╔══██╗██╔════╝
██║     ███████║███████║██║██╔██╗ ██║██████╔╝██║   ██║██║██╔██╗ ██║   ██║       ██║     ██║   ██║██████╔╝█████╗  
██║     ██╔══██║██╔══██║██║██║╚██╗██║██╔═══╝ ██║   ██║██║██║╚██╗██║   ██║       ██║     ██║   ██║██╔══██╗██╔══╝  
╚██████╗██║  ██║██║  ██║██║██║ ╚████║██║     ╚██████╔╝██║██║ ╚████║   ██║       ╚██████╗╚██████╔╝██║  ██║███████╗
 ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚═╝  ╚═══╝╚═╝      ╚═════╝ ╚═╝╚═╝  ╚═══╝   ╚═╝        ╚═════╝ ╚═════╝ ╚═╝  ╚═╝╚══════╝


? Will this Core use Bitcoin mainnet or testnet? Testnet
? Enter your Instance's Public IP Address: 3.17.78.45

Initializing Lightning wallet...
Create new address for wallet...
Creating Docker secrets...
****************************************************
Lightning initialization has completed successfully.
****************************************************
LND Wallet Password: rjcOgYehDmthuurduuriAMsr
LND Wallet Seed: absorb behind drop safe like herp derp celery galaxy wait orient sign suit castle awake gadget pass pipe sudden ethics hill choose six orphan
LND Wallet Address: tb1qfvjr20txm464fxcr0n9d4j2gkr5w4xpl55kl6u
******************************************************
You should back up this information in a secure place.
******************************************************

Please fund the Lightning Wallet Address above with Bitcoin and wait for 6 confirmation before running 'make deploy'

$ make deploy
```

### Chainpoint Client Node
Download Chainpoint Client Node to aggregate hashes, send them to Core for anchoring, and retrieve proofs. Follow the directions below to install and run a Client Node:

```bash
$ sudo apt-get install make git
$ git clone https://github.com/chainpoint/chainpoint-node-src.git
$ cd chainpoint-node-src
$ make install-deps

Please logout and login to allow your user to use docker


██████╗██╗  ██╗ █████╗ ██╗███╗   ██╗██████╗  ██████╗ ██╗███╗   ██╗████████╗    ███╗   ██╗ ██████╗ ██████╗ ███████╗
██╔════╝██║  ██║██╔══██╗██║████╗  ██║██╔══██╗██╔═══██╗██║████╗  ██║╚══██╔══╝    ████╗  ██║██╔═══██╗██╔══██╗██╔════╝
██║     ███████║███████║██║██╔██╗ ██║██████╔╝██║   ██║██║██╔██╗ ██║   ██║       ██╔██╗ ██║██║   ██║██║  ██║█████╗
██║     ██╔══██║██╔══██║██║██║╚██╗██║██╔═══╝ ██║   ██║██║██║╚██╗██║   ██║       ██║╚██╗██║██║   ██║██║  ██║██╔══╝
╚██████╗██║  ██║██║  ██║██║██║ ╚████║██║     ╚██████╔╝██║██║ ╚████║   ██║       ██║ ╚████║╚██████╔╝██████╔╝███████╗
 ╚═════╝╚═╝  ╚═╝╚═╝  ╚═╝╚═╝╚═╝  ╚═══╝╚═╝      ╚═════╝ ╚═╝╚═╝  ╚═══╝   ╚═╝       ╚═╝  ╚═══╝ ╚═════╝ ╚═════╝ ╚══════╝


? Will this Core use Bitcoin mainnet or testnet? Testnet
? Enter your Node's Public IP Address: 104.154.83.163

Initializing Lightning wallet...
Create new address for wallet...
Creating Docker secrets...
****************************************************
Lightning initialization has completed successfully.
****************************************************
Lightning Wallet Password: kPlIshurrduurSQoXa
LND Wallet Seed: absorb behind drop safe like herp derp celery galaxy wait orient sign suit castle awake gadget pass pipe sudden ethics hill choose six orphan
Lightning Wallet Address:tb1qglvlrlg0velrserjuuy7s4uhrsrhuzwgl8hvgm
******************************************************
You should back up this information in a secure place.
******************************************************

Please fund the Lightning Wallet Address above with Bitcoin and wait for 6 confirmation before running 'make deploy'

How many Cores would you like to connect to? (max 4) 2
Would you like to specify any Core IPs manually? No

You have chosen to connect to 2 Core(s).
You will now need to fund you wallet with a minimum amount of BTC to cover costs of the initial channel creation and future Core submissions.

? How many Satoshi to commit to each channel/Core? (min 120000) 500000
? 500000 per channel will require 1000000 Satoshi total funding. Is this OK? (Y/n) y

**************************************************************************************************************
Please send 1000000 Satoshi (0.01 BTC) to your wallet with address tb1qglvlrlg0velrserjuuy7s4uhrsrhuzwgl8hvgm
**************************************************************************************************************
This initialization process will now wait until your Lightning node is fully synced and your wallet is funded with at least 1000000 Satoshi.
*****************************************
Your lightning node is fully synced.
*****************************************
2020-01-18T04:32:59.361Z> Awaiting funds for wallet... wallet has a current balance of 0

Peer connection established with 03460d821ca4e9a59c8fc9665315ea98ea1960d86ad58e0ca18484dec776f2141c@3.17.78.45:9735
Peer connection established with 03eef6610d26489b897d81eb142f28ad5cd48a6b3e5c4e42a697cd00d5eb059313@3.135.54.225:9735

Channel created with 03460d821ca4e9a59c8fc9665315ea98ea1960d86ad58e0ca18484dec776f2141c@3.17.78.45:9735
Channel created with 03eef6610d26489b897d81eb142f28ad5cd48a6b3e5c4e42a697cd00d5eb059313@3.135.54.225:9735

*********************************************************************************
Chainpoint Node and supporting Lighning node have been successfully initialized.
*********************************************************************************

$ make deploy
```

### Chainpoint Clients
Download Chainpoint CLI or chainpoint-client-js to submit hashes to Client Nodes.

## Versions

## Important Links
