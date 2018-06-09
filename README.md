# Devicehive plugin ethereum

This plugin allows you to manipulate your custom smart contract via DeviceHive.

# Before the start

You should have an Ethereum node running somewhere. 
You also need an Ethereum account's address and password.
With this [guide](#Ethereum.docs) you can run the Ethereum node locally.

## Specify Ethereum configuration

Firstly you should specify the [Ethereum configuration](#Ethereum.config).
Add the Ethereum url, account's address and account's password to the config file *(path - ./src/ethereum-node/config.json)*. 
<br/>
*Note* - you can use an example to see how it works, just do not change other fields.

## Specify plugin configuration
In [plugin configuration](#Plugin.config) there are already [playground](https://playground.devicehive.com) urls, however you can run [DeviceHive](https://github.com/devicehive/devicehive-docker) locally and write your own.
Add a plugin topic and an access token. Also, add a user and a password for the [devicehive admin panel](https://github.com/devicehive/devicehive-admin-panel).

# Simple start

1. Create the plugin (locally or on Playground) and specify the [plugin configuration](#Plugin.config) file
2. Create a smart contract and specify the [Ethereum configuration](#Ethereum.config) file
3. Run `npm i`
4. Run `npm start`
5. Send a [message](#Message.model) from device. See the example [here](#Message.example).
6. Wait for the plugin receiving the message and sending the transaction to the blockchain. 

Logger will notify you about the results. You will see the message `Transaction has been passed successfully, hash: ${transactionHash}`.

# Configuration

<a name="Plugin.config"></a>

## Plugin
The plugin part of the configuration you can find [here](https://github.com/devicehive/devicehive-plugin-core-node#configuration).

**_FILTERS_** - filters for your plugin.
**_LOGGER_** - logger configuration.

Examples:

    "FILTERS":{
        "notifications": true,
        "commands": false,
        "command_updates": false,
        "names": "blockchain-record"
    }

    "LOGGER":{
        "level": 0, // 0 - info + error, 1 - error
        "filePath": "./dhe-log.txt"
    }

<a name="Ethereum.config"></a>

## Ethereum smart contract

You can find the configuration in `./src/ethereum-node/config.json`

* **_CONTRACT_PATH_** -  path to your smart contract, <br />
* **_ETHEREUM_URL_** - url to the Ethereum node, <br />
* **_CONTRACT_INITIAL_ARGS_** - arguments for initializing the smart contract, <br />
* **_ACCOUNT_ADDRESS_** - Ethereum account address <br />
* **_ACCOUNT_PASSWORD_** - Ethereum account password <br />
* **_GAS_LIMIT_** - maximum gas limit, which can be used by device <br />
* **_TIME_PERIOD_** - time period in minutes, every `TIME_PERIOD` minutes the gas limit will be updated <br />
* **_CONTRACT_ADDRESS_** - contract's address, which the plugin will use <br />
* **_CREATE_NEW_CONTRACT_** - set it to true, if you want to create a new contract <br />
* **_ALLOWED_METHODS_** - methods, which can be used by a device <br />

<a name="Message.example">

# Message example
Message name: `blockchain-record`
Parameters:
    {
        "method": "increase",
        "args": {
            "amount": 1
        },
        "options": {}
    }

<a name="Message.model"></a>

# Message model
    {
        "method":*method_name*,
        "args":{
            "arg1",
            "arg2",
            ...
        },
        "options":{
            "gas", // optional
            "gasPrice", // optional
            "value"" // optional
        }
    }
*Note*: if gas or gasPrice are not specified then the plugin will use min amount of gas for the transaction.

<a name="Ethereum.docs"></a>

# How to run Ethereum node locally

To start Ethereum locally:

*Note* - with this guide you will run dev node, so you will not be working with real Ethereum.

1. Download and install `geth`.
2. Open the terminal and run `geth --mine --minerthreads=1 --networkid=1999 --rpc --rpcaddr "127.0.0.1"  --rpcapi="admin,debug,miner,shh,txpool,personal,eth,net,web3" --dev`.
3. Open another terminal window and run `geth attach http://localhost:8545`.
4. In the second window run `personal.newAccount()` and enter your password. You will get account's address.
5. Make sure you have enough Ethereum. You can run `eth.accounts.forEach(function(account){if(account!==eth.coinbase){eth.sendTransaction({from:eth.coinbase,to:account,value:web3.toWei(1000000)})}})` to increase it on your account.
