---
description: (PART 1/3 OF NFT TUTORIAL SERIES)
---

# Part 1 : Mint NFT on edeXa chain

EDX-721 Solidity smart contracts

Beginner

With NFTs bringing blockchain into the public eye, now is an excellent opportunity to understand the hype yourself by publishing your own NFT contract (ERC-721 Token) on the edeXa blockchain!

In this tutorial, we will walk through creating and deploying an ERC-721 smart contract on the edeXa test network using [MetaMask(opens in a new tab)](https://metamask.io/), [Solidity(opens in a new tab)](https://docs.soliditylang.org/en/v0.8.0/), [Hardhat(opens in a new tab)](https://hardhat.org/), and  [Pinata(opens in a new tab)](https://pinata.cloud/) (don’t fret if you don’t understand what any of this means yet — we will explain it!).

In Part 2 of this tutorial we’ll go through how we can use our smart contract to mint an NFT, and in Part 3 we’ll explain how to view your NFT on MetaMask.

And of course, if you have questions at any point, don’t hesitate to reach out in the [edeXa discord](https://discord.gg/Pa523yUk)&#x20;

### Step 1: Connect to the edeXa test network

There are a bunch of ways to make requests to the edeXa blockchain, but to make things easy, we’ll use a RPC url from [chainlist.org](https://chainlist.org/chain/1995) a blockchain developer platform and API that allows us to communicate with the edeXa chain without having to run our own nodes.

### Step 2: Create an edeXa account (address)

We need an edeXa account to send and receive transactions. For this tutorial, we’ll use MetaMask, a virtual wallet in the browser used to manage your edeXa account address. If you want to understand more about how transactions on edeXa work, check out this page from the edeXa foundation.

You can download and create a MetaMask account for free [here(opens in a new tab)](https://metamask.io/download.html). When you are creating an account, or if you already have an account, make sure to switch over to the “edeXa Test Network” in the upper right (so that we’re not dealing with real money).

![](<../.gitbook/assets/image (3).png>)

### Step 3: Add ether from a Faucet

In order to deploy our smart contract to the test network, we’ll need some fake EDX. To get EDX you can go to the [edeXa faucet](https://faucet.edexa.com/) hosted by edeXa, log in and enter your account address, click “Send Me EDX”. You should see EDX in your MetaMask account soon after!

### Step 4: Check your Balance

To double check our balance is there, open the metamask  and check the account balance

Phew! Our fake money is all there.

### Step 5: Initialize our project

First, we’ll need to create a folder for our project. Navigate to your command line and type:

<pre><code><strong>1 mkdir my-nft 
</strong>2 cd my-nft
3
</code></pre>

Now that we’re inside our project folder, we’ll use npm init to initialize the project.  (we’ll also need [Node.js(opens in a new tab)](https://nodejs.org/en/download/), so download that too!).

```
1 npm init
2
```

It doesn’t really matter how you answer the installation questions; here is how we did it for reference:

```
1 package name: (my-nft)
2 version: (1.0.0)
3 description: My first NFT!
4 entry point: (index.js)
5 test command:
6 git repository:
7 keywords:
8 author: 
9 license: (ISC) 
10 About to write to /Users/thesuperb1/Desktop/my-nft/package.json:
11
12 {
13  "name": "my-nft", 
14  "version": "1.0.0",
15  "description": "My first NFT!",
16  "main": "index.js",
17  "scripts": {
18    "test": "echo \"Error: no test specified\" && exit 1"
19  },
20  "author": "",
21  "license": "ISC"
22 }
23
```

Approve the package.json, and we’re good to go!

### Step 6: Install [Hardhat(opens in a new tab)](https://hardhat.org/getting-started/#overview)

Hardhat is a development environment to compile, deploy, test, and debug your edeXa software. It helps developers when building smart contracts and dapps locally before deploying to the live chain.

Inside our my-nft project run:

```
1 npm install --save-dev hardhat
2
```

Check out this page for more details on [installation instructions(opens in a new tab)](https://hardhat.org/getting-started/#overview).

### Step 7: Create Hardhat project

Inside our project folder run:

```
1 npx hardhat
2
```

\


You should then see a welcome message and option to select what you want to do. Select “create an empty hardhat.config.js”:

This will generate a hardhat.config.js file for us which is where we’ll specify all of the set up for our project (on step 13).

### Step 8: Add project folders

To keep our project organized, we’ll create two new folders. Navigate to the root directory of your project in your command line and type:

```
1 mkdir contracts
2 mkdir scripts
3
```

* contracts/ is where we’ll keep our NFT smart contract code
* scripts/ is where we’ll keep scripts to deploy and interact with our smart contract

### Step 9: Write our contract

Now that our environment is set up, on to more exciting stuff: _writing our smart contract code!_

Open up the my-nft project in your favorite editor (we like [VSCode(opens in a new tab)](https://code.visualstudio.com/)). Smart contracts are written in a language called Solidity which is what we will use to write our MyNFT.sol smart contract.‌

1. Navigate to the `contracts` folder and create a new file called MyNFT.sol
2.  Below is our NFT smart contract code, which we based on the [OpenZeppelin(opens in a new tab)](https://docs.openzeppelin.com/contracts/3.x/erc721) library’s ERC-721 implementation. Copy and paste the contents below into your MyNFT.sol file.

    ```
    //Contract based on [https://docs.openzeppelin.com/contracts/3.x/erc721](https://docs.openzeppelin.com/contracts/3.x/erc721)
    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
    import "@openzeppelin/contracts/utils/Counters.sol";
    import "@openzeppelin/contracts/access/Ownable.sol";
    import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";

    contract MyNFT is ERC721URIStorage, Ownable {
        using Counters for Counters.Counter;
        Counters.Counter private _tokenIds;

        constructor() ERC721("MyNFT", "NFT") {}

        function mintNFT(address recipient, string memory tokenURI)
            public onlyOwner
            returns (uint256)
        {
            _tokenIds.increment();

            uint256 newItemId = _tokenIds.current();
            _mint(recipient, newItemId);
            _setTokenURI(newItemId, tokenURI);

            return newItemId;
        }
    }

    ```
3. Because we are inheriting classes from the OpenZeppelin contracts library, in your command line run `npm install @openzeppelin/contracts` to install the library into our folder.

So, what does this code _do_ exactly? Let’s break it down, line-by-line.

At the top of our smart contract, we import three [OpenZeppelin(opens in a new tab)](https://openzeppelin.com/) smart contract classes:

* @openzeppelin/contracts/token/ERC721/ERC721.sol contains the implementation of the ERC-721 standard, which our NFT smart contract will inherit. (To be a valid NFT, your smart contract must implement all the methods of the ERC-721 standard.) To learn more about the inherited ERC-721 functions, check out the interface definition [here(opens in a new tab)](https://eips.ethereum.org/EIPS/eip-721).
* @openzeppelin/contracts/utils/Counters.sol provides counters that can only be incremented or decremented by one. Our smart contract uses a counter to keep track of the total number of NFTs minted and set the unique ID on our new NFT. (Each NFT minted using a smart contract must be assigned a unique ID—here our unique ID is just determined by the total number of NFTs in existence. For example, the first NFT we mint with our smart contract has an ID of "1," our second NFT has an ID of "2," etc.)
* @openzeppelin/contracts/access/Ownable.sol sets up [access control(opens in a new tab)](https://docs.openzeppelin.com/contracts/3.x/access-control) on our smart contract, so only the owner of the smart contract (you) can mint NFTs. (Note, including access control is entirely a preference. If you'd like anyone to be able to mint an NFT using your smart contract, remove the word Ownable on line 10 and onlyOwner on line 17.)

After our import statements, we have our custom NFT smart contract, which is surprisingly short — it only contains a counter, a constructor, and single function! This is thanks to our inherited OpenZeppelin contracts, which implement most of the methods we need to create an NFT, such as `ownerOf` which returns the owner of the NFT, and `transferFrom`, which transfers ownership of the NFT from one account to another.

In our ERC-721 constructor, you’ll notice we pass 2 strings, “MyNFT” and “NFT.” The first variable is the smart contract’s name, and the second is its symbol. You can name each of these variables whatever you wish!

Finally, we have our function `mintNFT(address recipient, string memory tokenURI)` that allows us to mint an NFT! You'll notice this function takes in two variables:

* `address recipient` specifies the address that will receive your freshly minted NFT
* `string memory tokenURI` is a string that should resolve to a JSON document that describes the NFT's metadata. An NFT's metadata is really what brings it to life, allowing it to have configurable properties, such as a name, description, image, and other attributes. In part 2 of this tutorial, we will describe how to configure this metadata.

`mintNFT` calls some methods from the inherited ERC-721 library, and ultimately returns a number that represents the ID of the freshly minted NFT.

Now that we’ve created a MetaMask wallet, and written our smart contract, it’s time to connect the three.

Every transaction sent from your virtual wallet requires a signature using your unique private key. To provide our program with this permission, we can safely store our private key (and RPC) in an environment file.

To learn more about sending transactions, check out this tutorial on sending transactions using web3.

First, install the dotenv package in your project directory:

```
1 npm install dotenv --save
2
```

Then, create a `.env` file in the root directory of our project, and add your MetaMask private key and HTTP RPC URL to it.

* Follow [these instructions(opens in a new tab)](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key) to export your private key from MetaMask

Your `.env` should now look like this:

```
1 API_URL="https://testnet.edexa.com/rpc" 
2 PRIVATE_KEY="your-metamask-private-key"
3
```

To actually connect these to our code, we’ll reference these variables in our hardhat.config.js file in step 11.

Don't commit `.env`! Please make sure never to share or expose your `.env` file with anyone, as you are compromising your secrets in doing so. If you are using version control, add your `.env` to a [gitignore(opens in a new tab)](https://git-scm.com/docs/gitignore) file.

### Step 10: Install Ethers.js

Ethers.js is a library that makes it easier to interact and make requests to edeXa by wrapping standard JSON-RPC methods with more user friendly methods.

Hardhat makes it super easy to integrate [Plugins(opens in a new tab)](https://hardhat.org/plugins/) for additional tooling and extended functionality. We’ll be taking advantage of the [Ethers plugin(opens in a new tab)](https://hardhat.org/plugins/nomiclabs-hardhat-ethers.html) for contract deployment ([Ethers.js(opens in a new tab)](https://github.com/ethers-io/ethers.js/) has some super clean contract deployment methods).

In your project directory type:

```
1 npm install --save-dev @nomiclabs/hardhat-ethers ethers@^5.0.02
```

We’ll also require ethers in our hardhat.config.js in the next step.

### Step 11: Update hardhat.config.js

We’ve added several dependencies and plugins so far, now we need to update hardhat.config.js so that our project knows about all of them.

Update your hardhat.config.js to look like this:

```
1 /**
2 * @type import('hardhat/config').HardhatUserConfig
3 */
4 require('dotenv').config();
5 require("@nomiclabs/hardhat-ethers");
6 const { API_URL, PRIVATE_KEY } = process.env;
7 module.exports = { 
8   solidity: "0.8.1",
9   defaultNetwork: "goerli",
10   networks: {
11      hardhat: {},
12      edexa: {
13         url: API_URL,
14         accounts: [`0x${PRIVATE_KEY}`]
15      }
16   },
17 }
18
```

### Step 12: Compile our contract

To make sure everything is working so far, let’s compile our contract. The compile task is one of the built-in hardhat tasks.

From the command line run:

```
1 npx hardhat compile
2
```

You might get a warning about SPDX license identifier not provided in source file , but no need to worry about that — hopefully everything else looks good! If not, you can always message in the [edeXa discord](https://discord.gg/Pa523yUk) .

### Step 13: Write our deploy script

Now that our contract is written and our configuration file is good to go, it’s time to write our contract deploy script.

Navigate to the `scripts/` folder and create a new file called `deploy.js`, adding the following contents to it:

```
async function main() {
  const MyNFT = await ethers.getContractFactory("MyNFT")

  // Start deployment, returning a promise that resolves to a contract object
  const myNFT = await MyNFT.deploy()
  await myNFT.deployed()
  console.log("Contract deployed to address:", myNFT.address)
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error)
    process.exit(1)
  })
```

Hardhat does an amazing job of explaining what each of these lines of code does in their [Contracts tutorial(opens in a new tab)](https://hardhat.org/tutorial/testing-contracts.html#writing-tests), we’ve adopted their explanations here.

```
1 const MyNFT = await ethers.getContractFactory("MyNFT");
2
```

A ContractFactory in ethers.js is an abstraction used to deploy new smart contracts, so MyNFT here is a factory for instances of our NFT contract. When using the hardhat-ethers plugin ContractFactory and Contract instances are connected to the first signer by default.

```
1 const myNFT = await MyNFT.deploy();
2
```

Calling deploy() on a ContractFactory will start the deployment, and return a Promise that resolves to a Contract. This is the object that has a method for each of our smart contract functions.

### Step 14: Deploy our contract

We’re finally ready to deploy our smart contract! Navigate back to the root of your project directory, and in the command line run:

```
1 npx hardhat --network edexa run scripts/deploy.js
2
```

You should then see something like:

```
1 Contract deployed to address: 0xF243A6FA9f3Dc19D533Ed0eee2158F3dC1c8E1B9
2
```

If we go to the [edeXa explorer](https://explorer.edexa.com/) and search for our contract address we should be able to see that it has been deployed successfully. If you can't see it immediately, please wait a while as it can take some time. The transaction will look something like this:

![](<../.gitbook/assets/image (6).png>)

The From address should match your MetaMask account address and the To address will say “Contract Creation.” If we click into the transaction, we’ll see our contract address in the To field:

![](<../.gitbook/assets/image (9).png>)

Yasssss! You just deployed your NFT smart contract to the edeXa (testnet) chain!

That’s all for Part 1 of this tutorial. In Part 2, we’ll actually interact with our smart contract by minting an NFT, and in Part 3 we’ll show you how to view your NFT in your edeXa wallet!

#### Was this page helpful?
