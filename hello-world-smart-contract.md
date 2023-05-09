# "Hello World" Smart Contract

### How to Create a "Hello World" Smart Contract with Solidity

_9 min read_

#### Overview[​](broken-reference) <a href="#overview" id="overview"></a>

Want to become a Web3 Developer? In this short guide, you will gain an understanding of smart contracts and deploy your first _Hello World_ smart contract to an edeXa testnet blockchain . Let's dive in!

#### What We Will Do[​](broken-reference) <a href="#what-we-will-do" id="what-we-will-do"></a>

* Learn about the basics of Solidity
* Create a _Hello World_ Smart Contract
* Spin up an edeXa Endpoint
* Deploy the _Hello World_ Smart Contract
* Interact with the _Hello World_ Smart Contract

#### What You Will Need[​](broken-reference) <a href="#what-you-will-need" id="what-you-will-need"></a>

* A basic understanding of Ethereum.&#x20;
* A code editor (e.g., [VSCode](https://code.visualstudio.com/))
* [Remix.IDE](https://remix.ethereum.org/)
* A web3 wallet (e.g., [MetaMask](https://metamask.io/), [WalletConnect](https://walletconnect.com/) compatible)
* EDX on edeXa Testnet (e.g., you can get some at the [edeXa faucet](https://faucet.edexa.com/))

### What are Smart Contracts?[​](broken-reference) <a href="#what-are-smart-contracts" id="what-are-smart-contracts"></a>

A smart contract is an immutable digital contract designed to enforce the logic within it automatically. Smart contracts live on the blockchain and are written in Solidity (the native language for the EVM). This code is stored and replicated on the blockchain network, where it is executed upon being called or automatically when certain conditions are met.

### Solidity 101[​](broken-reference) <a href="#solidity-101" id="solidity-101"></a>

Before we build our _Hello Word_ smart contract, let us get a quick primer on Solidity.

In Solidity, smart contracts resemble classes in object-oriented programming languages. State variables, functions, function modifiers, events, errors, structural types, and enum types can all be declared in a contract. Contracts may also be inherited from other contracts. Now, let us recap some of these concepts.

**Global Variables**: You can use several built-in Solidity keywords to reference global information, such as block and transaction information. Let's list a few of the most popular:

* **msg.sender** (address): sender of the message (current call)
* **msg.sig** (bytes4): first four bytes of the calldata (i.e. function identifier)
* **msg.value** (uint): number of wei sent with the message
* **tx.origin** (address): sender of the transaction (full call chain)
* **block.number** (uint): current block number
* **block.timestamp** (uint): current block timestamp as seconds since unix epoch

Feel free to check out this [comprehensive list](https://docs.soliditylang.org/en/v0.8.17/units-and-global-variables.html) of built-in keywords for more information. You can also look at the full edeXa RPC methods available on the edeXa Labs.

You should also be aware of the EVM (i.e., Ethereum Virtual Machine), which is the virtual machine for the Ethereum blockchain. To get a quick overview, check out this [What is the Ethereum Virtual Machine (EVM) guide](ethereum-virtual-machine-evm.md).

**Variables & Types**: Variables have a type, which determines the kind of data they can store, and a name, which is used to reference the variable in the code. There are types such as:

* **uint**: unsigned integer, can store a non-negative whole number (i.e., 0, 1, 2, 3, etc.). They can also be prefixed with a number to limit the bit size (e.g., uint256). Solidity allows uint's from 8 to 256 (in steps of 8; uint8, uint16, uint24, etc.) and defaults to uint256 if not specified.
* **int**: signed integer, can be used to store a whole number with a sign (i.e. -1, 0, 1, 2, etc.)
* **bool**: boolean, can be used to store a true/false value
* **enum**: an enumerable value (i.e., enum Status { started, ended })
* **address**: an edeXa address used to store the address of another contract or account on the blockchain
* **bytes32**: a fixed-length byte array used to store a sequence of 32 bytes (256 bits)
* **string**: a string of characters used to store text
* **struct**: a type that can be made up of multiple types

**Functions**: Functions are used to define a block of code that can be executed multiple times in a smart contract. Functions can take input parameters, perform various operations on these parameters and return a result. Functions are declared using the function keyword, followed by the function name, input parameters, visibility, and the code block that defines the operations of the function. Functions visibility can be _public_, _private_, _internal_, _external_, _pure_ and _view_. Functions declared with the _view_ keyword can read the state but not modify it, while _pure_ functions do not view or access state variables.

Functions also have scopes that can allow different visibility. Here are some:

* **public**: Allows access from inside the contract and can be called from outside the scope of the contract
* **private**: Restricts access to only within the contract
* **internal**: Allows access within the contract or can be inherited within other contracts
* **external**: Allows access within the contract and call from outside the scope of the contract
* **payable**: Not a visibility but marks if a variable/function with _address_ types is a payable address (one that can be sent ether)

> These visibilities can also apply to variables except _external_

**Modifiers**: Modifiers are code that can be executed before and/or after a function call. Modifiers can be used to restrict function access, validate function inputs, and protect against attack vectors. A modifier can be setup such as:

{% code lineNumbers="true" %}
```
contract Test {
    address payable owner;

    constructor() { owner = payable(msg.sender); }
    
    modifier onlyOwner {
        require(
            msg.sender == owner,
            "Only owner can call this function."
        );
        _;
    }
}
```
{% endcode %}

**Events & Logs**: Events in Solidity are similar to logs. They are stored on-chain and placed inside functions you declare. When a function gets executed, the event is emitted and can be queried historically. An event may look something like this:

{% code lineNumbers="true" %}
```
contract Test {

    event LogUser(address _user, bool _status);

}
```
{% endcode %}

> Note you can also add the _indexed_ keyword to an event field to declare it as a special data structure called _topics_ (which we'll cover in another guide).

**Constructor**: A constructor is an optional function that gets executed upon deployment. It can be thought of as an initializer, and its state is added to the contract's bytecode rather than held in contract storage.

{% code lineNumbers="true" %}
```
contract Test {
    address public owner;

    constructor(address _owner) {
        owner = _owner;
    }
}
```
{% endcode %}

**Fallback Function**: Although not present in the HelloWorld contract we will be deploying in this guide, a fallback function is declared with **fallback() {}** function that gets executed when a function call is made on a contract that is not recognized or if no data was sent and there is no receive edeXa function. A test contract with a fallback function can look something like:

{% code lineNumbers="true" %}
```
contract Test {
    address x;

    fallback() external { x = msg.sender; }
}
```
{% endcode %}

### Get an edeXa Endpoint[​](broken-reference) <a href="#creating-an-ethereum-endpoint-on-quicknode" id="creating-an-ethereum-endpoint-on-quicknode"></a>

To deploy a smart contract to edeXa's test blockchain, you'll need an API endpoint to communicate with the network. You're welcome to use public nodes or deploy and manage your own infrastructure; however, if you'd like 8x faster response times, you can leave the heavy lifting to us.&#x20;

Get the edeXa RPC HTTP url from [chainlist.org](https://chainlist.org/chain/1995) .

### Configuring Your Web3 Wallet <a href="#configuring-your-web3-wallet-with-quicknode" id="configuring-your-web3-wallet-with-quicknode"></a>

If you're using MetaMask to deploy this contract, you'll first need to configure your RPC settings. However, note you can also use a WalletConnect compatible wallet for this guide. Some compatible wallets are [Coinbase Wallet](https://www.coinbase.com/es/wallet), [Rainbow Wallet](https://rainbow.me/) and [TrustWallet](https://trustwallet.com/es/).

Open up MetaMask and click the network dropdown at the top. After, click the **Add Network** button.

At the bottom of the page, click **Add a network manually** and fill out the following details:

* Network name: edeXa testnet
* New RPC URL: Enter the RPC HTTP URL you retrieved earlier
* Chain ID: 1995
* Currency symbol: EDX
* Block Explorer URL: [https://explorer.testnet.edexa.com/](https://explorer.testnet.edexa.com/)

It should end up looking similar to this:

### Retrieving Test Tokens via the edeXa Faucet[​](broken-reference) <a href="#retrieving-test-tokens-via-the-quicknode-faucet" id="retrieving-test-tokens-via-the-quicknode-faucet"></a>

Now that we have our edeXa Endpoint configured with our web3 wallet, the next step is retrieving some testnet tokens to pay for transactions on a blockchain.

Navigate to the [edeXa Faucet](https://faucet.edexa.com/), connect your wallet, and request tokens.

You can also verify the test EDX tokens are received by navigating to the explorer URL provided.

### Building the Hello World Smart Contract[​](broken-reference) <a href="#building-the-hello-world-smart-contract" id="building-the-hello-world-smart-contract"></a>

Time to build! Navigate to [Remix.IDE](https://remix.ethereum.org/) and open a new workspace (click the **+** sign, then select a **Blank** template and pick a name).

Create a file called **HelloWorld.sol** and input the following code:

{% code lineNumbers="true" %}
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract HelloWorld {
    string public greet = "Hello World!";
}
```
{% endcode %}

Let's recap the code.

Line 1: Specify the license identifier. In general, you can use MIT licensing to express permission to reuse code for any purpose. This is what we will use for this smart contract.

Line 2: Specify the pragma version. You can use the **^** character to note acceptable versions above the specified version. Also, note that this doesn't change the compiler version but acts as a check and throws an error if it is not met.

Line 4: Declare a **HelloWorld** contract using the **contract** keyword and open bracket (e.g., **{**)

Line 5: Declare a public state variable called **greet** which will contain a string value of "Hello World!".

### Compiling the Hello World Smart Contract[​](broken-reference) <a href="#compiling-the-hello-world-smart-contract" id="compiling-the-hello-world-smart-contract"></a>

This section will cover how to compile the _HelloWorld_ smart contract. Compiling your contract takes the solidity code and turns it into bytecode which can be deployed onto the blockchain. Note that you can also optimize your smart contract, which saves on storage of code size and runtime execution. However, we won't implement this in the contract we build later.

Navigate to the **Solidity Compiler** tab on Remix.IDE and click the Compile button (or use the CMD + S command on your keyword).

> Pro-tip: Use the auto-compile field to speed up your development.

Before moving on to the next step, check that your HelloWorld contract is compiled (i.e., a green checkmark on the left toolbar). However, if the compilation throws an error, check to ensure your code matches the one above.

### Deploying the Hello World Smart Contract[​](broken-reference) <a href="#deploying-the-hello-world-smart-contract" id="deploying-the-hello-world-smart-contract"></a>

Now that our code is created and compiled, it's time to deploy the smart contract to edeXa test network.

Navigate to the **Deploy & Run Transactions** tab, select Injected Provider or Wallet Connect and connect your wallet. Make sure the _HelloWorld.sol_ contract is set in the Contract field, then click **Deploy** and sign the transaction in your wallet.

Once deployed (which can take a minute or so), you should see a confirmed transaction similar to the one below:

### Interacting with the Hello World Smart Contract[​](broken-reference) <a href="#interacting-with-the-hello-world-smart-contract" id="interacting-with-the-hello-world-smart-contract"></a>

With our Hello World contract deployed, we can now interact with it. On [Remix.IDE](https://remix.ethereum.org/), navigate to the **Deployed Contracts** section and expand your _HelloWorld_ contract to click the **greet** button.

You should see a similar response:

### Resources[​](broken-reference) <a href="#resources" id="resources"></a>

If you want to take the next step into smart contract development, check out these other guides, which will help you build on top of the knowledge you just gained.

* [How to Create an ERC-20 Token](deploy-erc-20-in-edexa.md)
* [How to Create an ERC-721 Token](nft/)

### Final Thoughts[​](broken-reference) <a href="#final-thoughts" id="final-thoughts"></a>

We'd love to see what you're creating! Please share your ideas with us on [Discord](https://discord.gg/Pa523yUk) .
