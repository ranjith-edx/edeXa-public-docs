# Deploy a smart contract with Hardhat

### How to create and deploy a smart contract with Hardhat

_6 min read_

### Overview[​](broken-reference) <a href="#overview" id="overview"></a>

Ethereum development environments like [Truffle](https://www.trufflesuite.com/) and [Hardhat](https://hardhat.org/) make it easier to work with smart contracts and edeXa nodes. They provide a set of tools to seamlessly write, test, and deploy smart contracts. In this guide, we’ll create a hello world smart contract and deploy it using hardhat via edeXaNode.

**Prerequisites:**&#x20;

* Node.js installed on your system
* CLI/Terminal
* Text Editor

### What is Hardhat?[​](broken-reference) <a href="#what-is-hardhat" id="what-is-hardhat"></a>

[Hardhat](https://hardhat.org/) is a development environment that helps developers compile, deploy, test, and debug their Edexa applications. It has some of the cleanest, most detailed documentation. Hardhat also provides console.log() functionality, similar to javascript for debugging purposes. Hardhat also has many [plugins](https://hardhat.org/plugins/), which further increases its functionality.

### Installing hardhat and other dependencies[​](broken-reference) <a href="#installing-hardhat-and-other-dependencies" id="installing-hardhat-and-other-dependencies"></a>

We’ll install hardhat using [npm](https://www.npmjs.com/package/hardhat), which comes with [node.js](https://nodejs.org/en/).&#x20;

First, create a new project directory and cd into it. Feel free to use your own names here instead:

{% code lineNumbers="true" %}
```
mkdir HardhatTutorial
cd HardhatTutorial
```
{% endcode %}

Open terminal/cmd in your project directory and type the following.

Now that we have hardhat installed let’s start a new hardhat project. We’ll use npx to do so. Npx helps process node.js executables.&#x20;

You’ll be greeted with a CLI hardhat interface. Select the second option, "Create an empty hardhat.config.js", and press enter.

Now, let’s install other dependencies required to work with hardhat.

{% code lineNumbers="true" %}
```
npm install --save-dev @nomiclabs/hardhat-ethers ethers @nomiclabs/hardhat-waffle ethereum-waffle chai
```
{% endcode %}

We need these dependencies to write automated tests for contracts.

The most common issue while installing packages with npm can be an internal failure with \`node-gyp\`. You can follow [node-gyp installation instructions here](https://github.com/nodejs/node-gyp#installation).

> **Note**: You will need to have your python version match one of the compatible versions listed in the instructions above if you encounter the node-gyp issue.&#x20;

Another common issue is a stale cache. Clear your npm cache by simply typing the below into your terminal:

Now, let’s get a private key for our wallet, some test EDX for gas, and an Edexa node in place; all of which we'll need to deploy our smart contract.

### Getting the private key[​](broken-reference) <a href="#getting-the-private-key" id="getting-the-private-key"></a>

We’ll need an Edexa wallet/address to sign the smart contract deployment transaction. To set up the wallet, get its private key and add it to the config file of hardhat.

You can follow edeXa labs guides to generate a private key and an Ethereum address in [JavaScript](generate-a-new-edexa-address-in-javascript.md).

You can get a private key from your [MetaMask](https://metamask.io/) wallet. To do so, open your MetaMask browser plugin, select edeXa Test network and click on the three dots below the favicon.

Now, click on “Account details”

Then click on “Export Private Key”

Enter your password and click on confirm. It’ll display your private key (similar to the above image).

Copy and save the private key; we’ll need it in the later steps.

> It is recommended to store sensitive information like private keys in a .env file and get it into the config file using env variable or make a new secondary MetaMask wallet for development purpose.

### Getting testnet EDX <a href="#getting-testnet-eth" id="getting-testnet-eth"></a>

We’ll deploy our contract on the edeXa testnet. To get started, we will need some test EDX in our [MetaMask](https://metamask.io/) wallet, which we saw in the last step. You can get it by going to the [edeXa testnet faucet](https://faucet.edexa.com/). You'll need to select “Edexa Test Network” on your MetaMask wallet, copy-paste the wallet address into the text field in the faucet, and then click “Send me test tokens”.

### Booting an edeXa node[​](broken-reference) <a href="#booting-an-ethereum-node" id="booting-an-ethereum-node"></a>

To deploy our contract on edeXa’s blockchain we will need access to an edeXa node. For that, we could use pretty much any Ethereum client such as Geth or OpenEthereum (fka Parity). Since that is a bit too involved for deploying a single contract, we'll just grab a free rpc from  [chainlist.org](https://chainlist.org/) to make this easy. After you've created your free edeXa endpoint, copy your HTTP Provider endpoint.

You'll need this later, so copy and save it.

### Setting up the config file[​](broken-reference) <a href="#setting-up-the-config-file" id="setting-up-the-config-file"></a>

Open the hardhat.config.js file and paste the following into it:

{% code overflow="wrap" lineNumbers="true" %}
```
require("@nomiclabs/hardhat-waffle");

/**
 * @type import('hardhat/config').HardhatUserConfig
 */

const Private_Key = "ADD_YOUR_PRIVATE_KEY_HERE"

module.exports = {
  solidity: "0.7.3",
  networks: {
    edexa: {
        url: `ADD_YOUR_RPC_URL_HERE`,
        accounts: [`0x${Private_Key}`]
    }
  }
};
```
{% endcode %}

Replace **ADD\_YOUR\_PRIVATE\_KEY\_HERE** with the private key we obtained in the previous step and replace **ADD\_YOUR\_RPC\_URL\_HERE** with the HTTP node URL we obtained in the previous step.

Explanation of the code above:

**Line 1**: Importing the hardhat-waffle package.

**Line 7**: Storing our private key in the Private\_Key variable.

**Line 9-17 :** Mentioning Solidity version, network type, node URL, and accounts where we are supplying the private key and adding 0x as a prefix.

Save the file.

### Creating contract[​](broken-reference) <a href="#creating-contract" id="creating-contract"></a>

Now, for our contract, create a new directory "contracts" and place a new file "helloworld.sol" inside.

Copy-paste the following into your solidity script file:

{% code overflow="wrap" lineNumbers="true" %}
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.7.3;

contract HelloWorld {

    string saySomething;

    constructor() {
        saySomething = "Hello World!";
    }

    function speak() public view returns(string memory) {
        return saySomething;
    }
}
```
{% endcode %}

Explanation of the code above:

**Line 1**: Specifying [SPDX license](https://spdx.org/licenses/) type, which is an addition after Solidity ^0.6.8.&#x20;

> Whenever the source code of a smart contract is made available to the public, these licenses can help  resolve/avoid copyright issues. If you do not wish to specify any license type, you can use a special license [UNLICENSED](https://spdx.org/licenses/Unlicense.html) or simply skip the whole comment (it won’t result in an error, just a warning).

**Line 2**: Declaring the solidity version.

**Line 4**: Starting our contract named HelloWorld.

**Line 6**: Creating a variable saySomething of type string.

**Line 8-10**: Initiating the constructor and storing the string “Hello World!” in the saySomething variable.

**Line 12-14**: Creating a function called speak of type public, which will return the string stored in the saySomething variable.

Save the file and compile the contract using the following hardhat command.

If your contract compiles successfully, it will give an output like this:

### Deploying contract[​](broken-reference) <a href="#deploying-contract" id="deploying-contract"></a>

Now to deploy our contract, let’s create a deploy.js file in a new directory named scripts.

Copy-paste the following into your deploy.js file:

{% code overflow="wrap" lineNumbers="true" %}
```
async function main() {

    const [deployer] = await ethers.getSigners();

    console.log(
    "Deploying contracts with the account:",
    deployer.address
    );

    console.log("Account balance:", (await deployer.getBalance()).toString());

    const HelloWorld = await ethers.getContractFactory("HelloWorld");
    const contract = await HelloWorld.deploy();

    console.log("Contract deployed at:", contract.address);
}

main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
```
{% endcode %}

Explanation of the code above.

**Line 1**: Starting an async function.

**Line 3**: Getting the edeXa address to sign the transaction and storing it in the deployer address.&#x20;

**Line 5-8**: Printing the edeXa address along with a string to the console.

**Line 10**: Printing the balance of the edeXa address in wei to console. Wei is the smallest unit of edexa, one Wei = 10^−18 EDX.

**Line 12**: Calling the ethers.js method [ContractFactory](https://docs.ethers.io/v5/api/contract/contract-factory/). This will look for the "HelloWorld.sol" file, and return an instance that we can use ContractFactory methods on.

**Line 13**: Calling deploy on ContractFactory to deploy the contract.&#x20;

**Line 15**: Printing the address of the deployed contract to the console.

**Line 18**: Invoking the function "main".

**Line 19-22**: Checking for error, printing if any error, and exiting the process.

Save the file and run the following to deploy the contract.

{% code lineNumbers="true" %}
```
npx hardhat run scripts/deploy.js --network edexa
```
{% endcode %}

> **Note:** The network flag can be skipped if deploying to the mainnet.

On the successful deployment of the contract you will see an output containing the deployed contract’s address.

We can verify the deployment by copying and pasting the deployed contract’s address in [edexa explorer](https://explorer.edexa.com/) search bar. This will display information about the deployed contract.

### Conclusion[​](broken-reference) <a href="#conclusion" id="conclusion"></a>

Here we saw how to work with hardhat. Using hardhat, you can write tests for your contracts and debug them without sweating too much. Refer to [hardhat’s official documentation](https://hardhat.org/getting-started/) for more information.

If you have any feedback,  You can always chat with us on our [Discord](https://discord.gg/Pa523yUk) community server, featuring some of the coolest developers you’ll ever meet :)
