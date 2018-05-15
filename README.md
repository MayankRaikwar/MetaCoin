# MetaCoin 
### Source
1. [http://truffleframework.com]()
2. [http://truffleframework.com/tutorials/]()

### Overview
This tutorial will take you through the basics of creating a Truffle project and deploying a smart contract to a blockchain. In this project, We'll use the MetaCoin box, which creates a token that can be transferred between accounts.
 
#### Creating the Project
1. Install Truffle.
```sh
$ npm install -g truffle
```
2. Install metacoin truffle box in a directory `MetaCoin`. 
```sh
$ mkdir MetaCoin
$ cd MetaCoin
$ truffle unbox MetaCoin
```
Once this operation is completed, you'll now have a project structure with the following items:
- contracts/: Directory for Solidity contracts
- migrations/: Directory for scriptable deployment files
- test/: Directory for test files for testing your application and contracts
- truffle.js: Truffle configuration file

#### Exploring the Project
1. Open the contracts/MetaCoin.sol file in a text editor. This is a smart contract (written in Solidity) that creates a MetaCoin token. Note that this also references another Solidity file contracts/ConvertLib.sol in the same directory.
2. Open the contracts/Migrations.sol file. This is a separate Solidity file that manages and updates the status of your deployed smart contract. This file comes with every Truffle project, and is usually not edited.
3. Open the migrations/1_initial_deployment.js file. This file is the migration (deployment) script for the Migrations contract found in the Migrations.sol file.
4. Open the migrations/2_deploy_contracts.js file. This file is the migration script for the MetaCoin contract. (Migration scripts are run in order, so the file beginning with 2 will be run after the file beginning with 1.)
5. Open the test/TestMetacoin.sol file. This is a test file written in Solidity which ensures that your contract is working as expected.
6. Open the test/metacoin.js file. This is a test file written in JavaScript which performs a similar function to the Solidity test above.
7. Open the truffle.js file. This is the Truffle configuration file, for setting network information and other project-related settings. The file is blank, but this is okay, as we'll be using a Truffle command that has some defaults built-in.

#### Compileation and Migration of the project
1. In the `Migrations.sol` file in the `/contracts` directory, modify the **Old code** with the **New code**.

**Old code**
```sh
  function Migrations() public {
    owner = msg.sender;
  }
```
**New code**
```sh
  constructor() public {
    owner = msg.sender;
  }
```
2. For testing the MetaCoin smart Contracts, run the follwing command.
```sh
truffle test TestMetacoin.sol
```
You will see the following output:
```sh
TestMetacoin
    √ testInitialBalanceUsingDeployedContract (71ms)
    √ testInitialBalanceWithNewMetaCoin (59ms)

  2 passing (794ms)
```
3. Go back to the `MetaCoin` root directory `/` run the truffle development console.
```sh
truffle develop
```
4. Compile the smart contracts. Note inside the development console we don't need to preface commands with `truffle`.
```sh
compile
```
You should see output similar to the following:
```sh
Compiling .\contracts\ConvertLib.sol...
Compiling .\contracts\MetaCoin.sol...
Compiling .\contracts\Migrations.sol...

Writing artifacts to .\build\contracts
 
```
5. Now that we've successfully compiled our contracts, it's time to **migrate** them to the blockchain! To know about migration read the [migration documentation](http://truffleframework.com/docs/getting_started/migrations). You will see one JavaScript file already in the `migrations/` directory: `1_initial_migration.js`. This handles deploying the `Migrations.sol` contract. However, `2_deploy_contracts.js` handles deploying `MetaCoin.sol`. 
- Before we can migrate our contract to the blockchain, we need to have a blockchain running. For this tutorial, we're going to use [Ganache](http://truffleframework.com/ganache), a personal blockchain for Ethereum development you can use to deploy contracts, develop applications, and run tests. If you haven't already, [download Ganache](http://truffleframework.com/ganache) and double click the icon to launch the application. This will generate a blockchain running locally on port **7545**. Read the ganache documentation [here](http://truffleframework.com/docs/ganache/using).

6. As contracts are already compiled. deploy them to the blockchain
```sh
migrate
```
You will see the output similar to this
```sh
Using network 'development'.

Running migration: 1_initial_migration.js
  Replacing Migrations...
  ... 0x63b393bd50251ec5aa3e159070609ee7c61da55531ff5dea5b869e762263cb90
  Migrations: 0xd6d1ea53b3a7dae2424a0525d6b1754045a0df9f
Saving successful migration to network...
  ... 0xe463b4cb6a3bbba06ab36ac4d7ce04e2a220abd186c8d2bde092c3d5b2217ed6
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Replacing ConvertLib...
  ... 0xa59221bc26a24f1a2ee7838c36abdf3231a2954b96d28dd7def7b98bbb8a7f35
  ConvertLib: 0x33b217190208f7b8d2b14d7a30ec3de7bd722ac6
  Replacing MetaCoin...
  ... 0x5d51f5dc05e5d926323d580559354ad39035f16db268b91b6db5c7baddef5de5
  MetaCoin: 0xcd2c65cc0b498cb7a3835cfb1e283ccd25862086
Saving successful migration to network...
  ... 0xeca6515f3fb47a477df99c3389d3452a48dfe507980bfd29a3c57837d6ef55c5
Saving artifacts...
```
Ganache will also list these transactions as well.
##### Interacting with the contract
To interact with the contract, you can use the Truffle console. The Truffle console is similar to Truffle Develop, except it connects to an existing blockchain (in this case, the one generated by Ganache).
```sh
truffle console
```
You will see the following prompt:
```sh
truffle(development)>
```
Now we can interact with the contract using the above console in the following ways:
- Check the metacoin balance of the account that deployed the contract:
```sh
MetaCoin.deployed().then(function(instance){return instance.getBalance(web3.eth.accounts[0]);}).then(function(value){return value.toNumber()});
```
- See how much ether that balance is worth (and note that the contract defines a metacoin to be worth 2 ether):
```sh
MetaCoin.deployed().then(function(instance){return instance.getBalanceInEth(web3.eth.accounts[0]);}).then(function(value){return value.toNumber()});
```
- Transfer some metacoin from one account to another:
```sh
MetaCoin.deployed().then(function(instance){return instance.sendCoin(web3.eth.accounts[1], 500);});
```
- Check the balance of the account that received the metacoin (accounts[1]) (or the sent account (accounts[0])):
```sh
MetaCoin.deployed().then(function(instance){return instance.getBalance(web3.eth.accounts[1]);}).then(function(value){return value.toNumber()});
```
Cheers!!

