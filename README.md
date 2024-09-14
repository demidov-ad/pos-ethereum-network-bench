<div align="center">

# Deploy local PoS Ethereum network with benchmark tool 

[![license](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)
</div>  

![alt text](image.png)

This repository provides a docker-compose file to run a fully-functional, local development network for Ethereum with proof-of-stake enabled. This configuration uses Prysm as a consensus client and go-ethereum for execution. It starts from proof-of-stake and does not go through the Ethereum merge.

This sets up a single node development network with 64 deterministically-generated validator keys to drive the creation of blocks in an Ethereum proof-of-stake chain. Here's how it works:

We initialize a go-ethereum, proof-of-work development node from a genesis config. We initialize a Prysm beacon chain, proof-of-stake development node from a genesis config

The development net is fully functional and allows for the deployment of smart contracts and all the features that also come with the Prysm consensus client such as its rich set of APIs for retrieving data from the blockchain.

## Using
To run ETH network:
```
sudo sh run.sh
```
To stop ETH network:
```
sudo sh stop.sh
```
After running ETH-network you can attach to node:
```
geth attach node1/geth.ipc 

> net.peerCount
> eth.blockNumber
```
You can also interact with a beacon node:
```
curl localhost:3500/eth/v1/node/identity
```

# Available Features
- The network launches with a [Validator Deposit Contract](https://github.com/ethereum/consensus-specs/blob/dev/solidity_deposit_contract/deposit_contract.sol) deployed at address `0x4242424242424242424242424242424242424242`. This can be used to onboard new validators into the network by depositing 32 ETH into the contract
- The default account used in the go-ethereum node is address `0x123463a4b065722e99115d6c222f267d9cabb524` which comes seeded with ETH for use in the network. This can be used to send transactions, deploy contracts, and more
- The default account, `0x123463a4b065722e99115d6c222f267d9cabb524` is also set as the fee recipient for transaction fees proposed validators in Prysm. This address will be receiving the fees of all proposer activity
- The go-ethereum JSON-RPC API is available at http://geth:8545
- The Prysm client's REST APIs are available at http://beacon-chain:3500. For more info on what these APIs are, see [here](https://ethereum.github.io/beacon-APIs/)
- The Prysm client also exposes a gRPC API at http://beacon-chain:4000

## OS Setup for Bech
Install ```python3``` and ```pip3```, in ubuntu that can be done by:
```bash
sudo apt-get install python3 python3-pip
```

Then install requirements:
```bash
pip3 install -r requirements.txt
```


## Profiles setup
Use default ```profiles.json``` to add your own network. Base setup for current network already added.

## Test execution
Minimal test to check everything works:
  ```bash
  python3 bench/bench.py -p testnet --unconfirmed
  ```

Extensive test:
```bash
  python3 bench/bench.py -p testnet -c 10 -t 150 --all
  ```

### Logging
Each execution is incrementally logged to ```bench_{profile_name}.log``` file. You can find there all past results.
```bash
grep Results bench_{profile_name}.log
```
You will also find on the log file all the transaction hashes for all the transactions sent.

### Bench options
Mandatory options:
- ```-p <string>``` To set the profile, must be filled on profiles.json
- ```-c <int>``` How many paralel processes to run, 1 if omitted
- ```-t <int>``` Number of transactions per process, 1 if omitted. We will refer to this number as **t**
  
  Available tests to run:
- ```--allconfirmed``` Each process launch **t** txs and confirm all of them one by one
- ```--confirmed``` Each process lanch **t** txs and confirm just the last one
- ```--unconfirmed``` Each process launch **t** txs but it does not confirm any of them
- ```--erc20``` Each process creates **t** ERC20 tokens + **t** token transfers



## Credits
Thanks for [Prysm](https://github.com/prysmaticlabs/prysm) team and [OffchainLabs](https://github.com/OffchainLabs/eth-pos-devnet) for all work they've made.  
Thanks for [Xavier Romero](https://github.com/xavier-romero) for [great bech-tool](https://github.com/xavier-romero/eth-bench)
