# bc-sample-dapp

A sample Ethereum smart contract built and deployed on a private test network

# Prerequisites

I am using Truffle and the Garnache client for this sample. The tools can installed as follows:

```
npm install -g truffle
npm install -g ganache-cli
```

I have trialled the sample on Windows. If you're on Linux you need to rename the file
`truffle-config.js` to `truffle.js`. This is, because on Windows there is a naming 
problem, which causes the config file “truffle.js” to be opened instead of reading what’s inside 
when truffle commands are executed.

# Build

The Smart Contract is compiled to Ethereum Virtual Machine byte code as follows:

```
truffle compile
```

# Testing with Garnache

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

On the Garnache console, you should see four new transactions output where the 
penultimate contains hash of the Greeter contract created. This output corresponds 
with the reference for the Gretter output by `truffle migrate` command sequence above.

```
  Transaction: 0x1b449b9de01ccad06b02a37b92012bb75ee726f69cd3d6c723e02a3dbfbf17c7
  Contract created: 0x8a6389fda085927c404ccc4ae374f9a99e850c44
  Gas usage: 273189
  Block Number: 3
  Block Time: Thu Jul 05 2018 16:36:38 GMT+0200 (Mitteleuropäische Sommerzeit)
```

### Using the contract

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

### What's next

Tbd. Deploy contract on on private blockchain test network using geth. 
