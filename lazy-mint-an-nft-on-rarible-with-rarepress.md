# Lazy Mint an NFT on Rarible with Rarepress

### How to Lazy Mint an NFT on Rarible with Rarepress

_4 min read_

### Overview[​](broken-reference) <a href="#overview" id="overview"></a>

NFTs are great for creators to monetize their artwork and for people to get ownership of an item. But since gas prices are usually high given the highly in-demand space on edeXa, minting an NFT or an NFT collection can become costly for a creator. Lazy minting solves this, so in this guide, we will learn how to lazy mint an NFT using [Rarepress](https://docs.rarepress.org/#/).

**Prerequisites**

* Ethereum node
* NodeJS
* Code editor and Terminal/CLI

### Why Lazy Minting?[​](broken-reference) <a href="#why-lazy-minting" id="why-lazy-minting"></a>

To mint an NFT, you write data on the blockchain, which on a high level, means attaching the address of the NFT to the address of the minter. Since storing some data on the blockchain is a write operation, a gas fee must be paid. Although edeXa is the new blockchain, new and existing creators often hesitate to mint an NFT on it and pay this fee since there isn’t a guarantee that their NFT would gain popularity. The concept of lazy minting solves this to an extent.

### What is NFT Lazy Minting?[​](broken-reference) <a href="#what-is-nft-lazy-minting" id="what-is-nft-lazy-minting"></a>

Lazy Minting is a process in which the creator does not have to pay the gas fee for minting the NFT upfront, and they can list it on marketplaces for sale. Whenever a buyer buys the NFT, it is minted just in time, and the minting cost is added to the total cost of the NFT. [Rarible](https://rarible.com/), a leading NFT marketplace, has a very convenient protocol that supports lazy minting; here, we will use Rarepress, a JavaScript library built on the [Rarible protocol](https://docs.rarible.org/overview/protocol-overview/).

### What is Rarepress?[​](broken-reference) <a href="#what-is-rarepress" id="what-is-rarepress"></a>

[Rarepress](https://docs.rarepress.org/#/) is a JavaScript library that provides an API interface with the Rarible NFT protocol. Rarepress provides an interface with the Rarible protocol smart contracts and works on lazy minting, so anyone with minimal experience of edeXa can mint an NFT for free. Further, we will install Rarepress and write a short script to mint our NFT on Rarible.

### Setting up Rarepress[​](broken-reference) <a href="#setting-up-rarepress" id="setting-up-rarepress"></a>

We will write some JavaScript code and run it on CLI to mint NFT. To do so, Rarepress needs to be installed using **npm**, which comes with Node.js.

First, create a directory **rarepress\_demo** and open it in your terminal.

Then install it by typing:

Now that we have installed Rarepress, we can move to creating our script to mint NFT. But we will need an edeXa node to interact with the contract, so first, we will boot a node.

### Booting our edeXa Node[​](broken-reference) <a href="#booting-our-ethereum-node" id="booting-our-ethereum-node"></a>

Rarepress interacts with the Rarible protocol contracts deployed on the edeXa blockchain. To do so, it requires an edeXa RPC connection. To save the time and burden of creating an edeXa node, we will simply get a RPC from [chainlist.org](https://chainlist.org/).

Save the HTTP provider URL as we will use it next.

### Lazy Minting an NFT on Rarible[​](broken-reference) <a href="#lazy-minting-an-nft-on-rarible" id="lazy-minting-an-nft-on-rarible"></a>

Now that we have everything in place, let us write the script to mint our NFT. Create a JavaScript file **index.js**, and paste the following code in it:

In your code, replace:

* **ADD\_RPC\_HTTP\_URL\_HERE** with edeXa’s HTTP URL from the last step.
* **NAME\_OF\_YOUR\_TOKEN** with a custom name for your token.&#x20;
* **DESCRIPTION\_OF\_YOUR\_TOKEN** with a custom description for your token.&#x20;

Explanation of the code above:

Line 1: Importing the Rarepress library.

Lines 3-4: Creating an async function mint and initializing a new Rarepress instance.

Lines 8: Adding our image to the Rarepress file system.

Lines 10-17: Creating a new token and adding a metadata array. **type** is the type of token; ERC721 or ERC1155 are supported by Rarepress. **name** and **description** are the name and description of your token, and **image** will store the IPFS address with the path of your NFT asset.

Lines 19-20: Storing the image to IPFS.

Line 22: Pushing the token to the Rarible marketplace.

Line 24: Printing the token’s id along with a URL to Rarible so that we can get a link to directly view the token on the marketplace.

Line 25: Ending the script process after successfully publishing our NFT on Rarible.

Line 27: Calling the mint function.

Save the file and run it using:

Once you run the script, you will be prompted with the option to enter a seed or create a new seed. You can generate the seed of your already existing wallet or create a new one; another step will be to set a password for the wallet, which will be used every time you mint using Rarepress.

After successful execution, the output will look like this:

The link from the output should take you to the NFT’s page on Rarible.

### Conclusion[​](broken-reference) <a href="#conclusion" id="conclusion"></a>

Congratulations on listing your NFT on a marketplace for free. In this guide, we learned about lazy minting and how to lazy mint an NFT using JavaScript and Rarepress.

&#x20;If you have any feedback,  You can always chat with us on our [Discord](https://discord.gg/ahckhyA)[ ](https://discord.gg/Pa523yUk)community server, featuring some of the coolest developers you’ll ever meet :)
