# bc-sample-dapp

A sample Ethereum smart contract built and deployed on a private test network

## Prerequisites

I am using Truffle and the Garnache client for this sample. The tools can installed as follows:

```
npm install -g truffle
npm install -g ganache-cli
```

I have trialled the sample on Windows. If you're on Linux you need to rename the file
`truffle-config.js` to `truffle.js`. This is, because on Windows there is a naming 
problem, which causes the config file “truffle.js” to be opened instead of reading what’s inside 
when truffle commands are executed.

In a second step I have setup an private Ethereum node using the `geth` client which you need to
 [install for your platform](https://geth.ethereum.org/downloads/). On Windows, you may also need 
 to add geth’s installation folder to your PATH variable. We will also a need a wallet. I have been using 
 the Mist Wallet, but the Ethereum wallet should also work fine. Both of them can be found in the 
 [download section of the Mist project](https://github.com/ethereum/mist/releases). Ethereum wallet can be see 
 as subset of Mistt, which only opens one single dapp. Mist provide more flexibilty, but is still considered
 insecure (beta) should not be used with untrusted dapps as of yet.
 


## Build

The Smart Contract is compiled to Ethereum Virtual Machine byte code as follows:

```
truffle compile
```

## Testing with Garnache

First of all we need to start the Garnache client on port 7534 as this is the port I have set
in `truffle-config.js` for the "development" network.

```
ganache-cli -p 7545
```

Now, we need to deploy our code on the "development" network.

```
truffle migrate --network development
```

If the latter succeeds you should get some output similar to the following:

```
C:\Users\marcus\bc-sample-dapp>truffle migrate --network development
Using network 'development'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0x3770c0c708a1ca4ad7650b5ee36eb6f6c8ceb1bf8febce3d990de3e61da8558e
  Migrations: 0x150b10459935ff8a86beb8acc5f35c23cf428525
Saving successful migration to network...
  ... 0xb3088586ac3d9751d666fbf3ca42e33fc63c87aaaa7578fe50b9c6a7868b897e
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying Greeter...
  ... 0x1b449b9de01ccad06b02a37b92012bb75ee726f69cd3d6c723e02a3dbfbf17c7
  Greeter: 0x8a6389fda085927c404ccc4ae374f9a99e850c44
Saving successful migration to network...
  ... 0xdc2702e3a0cd386373b8359a3f7af7d463335aa9181658ef8dc40c2c0e90fc1b
Saving artifacts...```
```

On the Garnache console, you should see output for four new transactions where the 
penultimate contains the hash of the Greeter contract created. This output corresponds 
with the reference for the Greeter output by `truffle migrate` command sequence above.

```
  Transaction: 0x1b449b9de01ccad06b02a37b92012bb75ee726f69cd3d6c723e02a3dbfbf17c7
  Contract created: 0x8a6389fda085927c404ccc4ae374f9a99e850c44
  Gas usage: 273189
  Block Number: 3
  Block Time: Thu Jul 05 2018 16:36:38 GMT+0200 (Mitteleuropäische Sommerzeit)
```

## Using the contract

Next, we will invoke the `greet` function on the contract. For this, we use the Truffle 
console which allows for executing transactions interactively for testing and 
debugging purposes.

```
truffle console --network development
```

This will start an interactive shell where we will execute the following commands. The 
latter command calls the `greet`function on the deployed `Greeter`contract.

```
Greeter.deployed().then(inst => { GreeterInstance = inst })
GreeterInstance.greet.call()
```

If this succeeded you should see some output similar to the following. 
When the `greet` function is called, it should output "Hello world!!!".

```
C:\Users\marcus\bc-sample-dapp>truffle console --network development
truffle(development)>  Greeter.deployed().then(inst => { GreeterInstance = inst })
undefined
truffle(development)> GreeterInstance.greet.call()
'Hello world!!!'
truffle(development)>
``` 

## Using a Private Test Network

As an alternative to using Garnache, I have setup a private test network using `geth` (see also Prerequisites 
section above). As part of the Truffle configuration I have create a network configuration for this called "testNet".

### Creating a Network Node

In the `node` directory a configuration file can be found which I have used to create a node on the local host. To 
initialize the node the following command needs to invoked-

```
geth --datadir=./node/chaindata/ init ./node/genesis.json
```

If the command succeeded you should get output similar to the following.

```
C:\Users\marcus\bc-sample-dapp>geth --datadir=./geth/chaindata/ init ./geth/genesis.json
WARN [07-06|16:04:34] No etherbase set and no accounts found as default
INFO [07-06|16:04:35] Allocated cache and file handles         database=C:\\Users\\marcus\\bc-sample-dapp\\geth\\chaindata\\geth\\chaindata cache=16 handles=16
INFO [07-06|16:04:35] Writing custom genesis block
INFO [07-06|16:04:35] Successfully wrote genesis state         database=chaindata                                                           hash=0613eb…9a64e7
INFO [07-06|16:04:35] Allocated cache and file handles         database=C:\\Users\\marcus\\bc-sample-dapp\\geth\\chaindata\\geth\\lightchaindata cache=16 handles=16
INFO [07-06|16:04:35] Writing custom genesis block
INFO [07-06|16:04:35] Successfully wrote genesis state         database=lightchaindata                                                           hash=0613eb…9a64e7
```

### Running the node

After the node setup has been initialized we can run the node as shown in the following. The `--rpc` option
instruct Geth to accept RPC connections, it’s needed so that Truffle can connect to Geth.

```
geth --datadir=./node/chaindata/ --rpc
```

If the command succeeded you should get output similar to the following.

``` 
C:\Users\marcus\bc-sample-dapp>geth --datadir=./geth/chaindata/ --rpc
WARN [07-06|16:21:05] No etherbase set and no accounts found as default
INFO [07-06|16:21:05] Starting peer-to-peer node               instance=Geth/v1.7.2-stable-1db4ecdc/windows-amd64/go1.9
INFO [07-06|16:21:05] Allocated cache and file handles         database=C:\\Users\\marcus\\bc-sample-dapp\\geth\\chaindata\\geth\\chaindata cache=128 handles=1024
WARN [07-06|16:21:05] Upgrading database to use lookup entries
INFO [07-06|16:21:05] Database deduplication successful        deduped=0
INFO [07-06|16:21:05] Initialised chain configuration          config="{ChainID: 15 Homestead: 0 DAO: <nil> DAOSupport: false EIP150: <nil> EIP155: 0 EIP158: 0 Byzantium: <nil> Engine: unknown}"
INFO [07-06|16:21:05] Disk storage enabled for ethash caches   dir=C:\\Users\\marcus\\bc-sample-dapp\\geth\\chaindata\\geth\\ethash count=3
INFO [07-06|16:21:05] Disk storage enabled for ethash DAGs     dir=C:\\Users\\marcus\\AppData\\Ethash                               count=2
INFO [07-06|16:21:05] Initialising Ethereum protocol           versions="[63 62]" network=1
INFO [07-06|16:21:05] Loaded most recent local header          number=0 hash=0613eb…9a64e7 td=131072
INFO [07-06|16:21:05] Loaded most recent local full block      number=0 hash=0613eb…9a64e7 td=131072
INFO [07-06|16:21:05] Loaded most recent local fast block      number=0 hash=0613eb…9a64e7 td=131072
INFO [07-06|16:21:05] Regenerated local transaction journal    transactions=0 accounts=0
INFO [07-06|16:21:05] Starting P2P networking
INFO [07-06|16:21:07] UDP listener up                          self=enode://9162924c446c45d47c23194ba341623a6d2011a550587ddb49e65b05c819de62c26c89629d9abaed2822173c1cc6e43a5eb9faa68f3349da8cbc38ab9dec3fa4@[::]:30303
INFO [07-06|16:21:07] RLPx listener up                         self=enode://9162924c446c45d47c23194ba341623a6d2011a550587ddb49e65b05c819de62c26c89629d9abaed2822173c1cc6e43a5eb9faa68f3349da8cbc38ab9dec3fa4@[::]:30303
INFO [07-06|16:21:07] IPC endpoint opened: \\.\pipe\geth.ipc
INFO [07-06|16:21:07] HTTP endpoint opened: http://127.0.0.1:8545
```


```
mist –rpc "http://127.0.0.1:8545"
```


```
geth attach http://127.0.0.1:8545
```