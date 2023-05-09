---
coverY: 0
---

# Deploy an EDX-1155 NFT

### How to Create and Deploy an EDX-1155 NFT

_6 min read_

### Overview[​](broken-reference) <a href="#overview" id="overview"></a>

EDX1155 has emerged as a gold standard to create NFTs; every major marketplace lists new tokens as an EDX1155 standard. In this guide, we will learn about the EDX1155 token standard and how to create an EDX1155 token.

**What we will do:**

1. Create 3 NFT collections
2. Create and deploy an EDX-1155 contract
3. Update the contract to be compatible with OpenSea
4. Deploy our NFT collections

**What you will need:**

* Image assets to create NFTs
* [MetaMask](https://metamask.io/) and some edeXa test ETH.
* Knowledge of [ERC20](https://www.quicknode.com/guides/solidity/how-to-create-and-deploy-an-erc20-token) and [ERC721 (NFT)](https://www.quicknode.com/guides/solidity/how-to-create-and-deploy-an-erc-721-nft) token standards.

### What is EDX1155?[​](broken-reference) <a href="#what-is-erc1155" id="what-is-erc1155"></a>

EDX1155 is a multi-token standard that allows the creation of fungible, non-fungible, and semi-fungible tokens all in one contract. Before ERC1155, if a use case needed both ERC20 (fungible) and ERC721 (non-fungible) tokens, then separate contracts were required to achieve this. EDX1155 also allows for multiple NFT collections to be launched in just one smart contract instead of creating a different contract for each collection; this increases efficiency in smart contract construction and minimizes the transaction count, which is very important as it consumes less blockchain space. With EDX1155, batch transfer of tokens is also possible instead of transferring a token to a single address in previous standards.

A prevalent example of the EDX1155 application is blockchain-based decentralized games, as games need coins and collectibles, so EDX1155 has become a standard there. EDX1155 has also become a standard in the NFT space.

The previous ERC721 had a one-to-one mapping of token id with the address. EDX1155 has a rather complex mapping where the address in a combination of token id is mapped to the balance of the token.

We will create 3 NFT collections (rock, paper, and scissors) with a single NFT in each one. To upload our files to the decentralized storage IPFS, we can [upload them through CLI](https://www.quicknode.com/guides/web3-sdks/how-to-integrate-ipfs-with-ethereum) or use this very easy-to-use tool [NFT Storage](https://nft.storage/).

We will be using the second option, NFT Storage. Sign in to NFT Storage and upload your image files for rock, paper, and scissors. You should see something like this once they've been uploaded successfully:

Click on "Actions" and copy the IPFS URL of each image; we will need it for the metadata of each collection.

We will create three JSON metadata files to store information about our NFT collections.

* 1.json: Rock collection
* 2.json: Paper collection
* 3.json: Scissors collection

Our 1.json file will look something like this:

```
{ //1.

    "name": "Rocks",

    "description": "This is a collection of Rock NFTs.",

    "image": "https://ipfs.io/ipfs/bafkreifvhjdf6ve4jfv6qytqtux5nd4nwnelioeiqx5x2ez5yrgrzk7ypi",

}
```

* **name**: Has the name of the NFT.
* **description**: Has the description of the NFT.
* **image**: Has the link to the image we got earlier (IPFS URL).&#x20;
* If a collection has multiple images, which is usually the case, an extra parameter **id** is added to differentiate tokens amongst the collection.

Create the remaining JSON files, 2.json and 3.json, for paper and scissors collections, respectively.

To efficiently upload all the JSON files to IPFS, we will archive them in content-addressed format. [https://car.ipfs.io/](https://car.ipfs.io/) helps archive files in IPFS compatible content-addressed archive (.car) format.

Head over to [IPFS CAR](https://car.ipfs.io/) and upload all three JSON files. Once uploaded, download the .car file and upload it to [NFT Storage](https://nft.storage/). All our JSON files are now stored on IPFS in an archived manner. Copy the IPFS URL of the uploaded .car file, and you should be able to access JSON files by just entering the file name at the end of the URL, for example:

[https://ipfs.io/ipfs/bafybeihjjkwdrxxjnuwevlqtqmh3iegcadc32sio4wmo7bv2gbf34qs34a/1.json\
](https://ipfs.io/ipfs/bafybeihjjkwdrxxjnuwevlqtqmh3iegcadc32sio4wmo7bv2gbf34qs34a/1.json)

### Creating & Deploying the ERC1155 Contract[​](broken-reference) <a href="#creating--deploying-the-erc1155-contract" id="creating--deploying-the-erc1155-contract"></a>

We will use the [OpenZeppelin](https://openzeppelin.com/contracts/) contracts library to create our ERC1155 contract and deploy it using [Ethereum REMIX IDE](https://remix.ethereum.org/) on the Ropsten testnet. Make sure you have some Ropsten test ETH which you can also get from [edeXa faucet](https://faucet.edexa.com/).

Create a new file, token.sol, in REMIX and paste the following code into it.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";

contract rockPaperScissors is ERC1155 {
    uint256 public constant Rock = 1;
    uint256 public constant Paper = 2;
    uint256 public constant Scissors = 3;

    constructor() ERC1155("https://ipfs.io/ipfs/bafybeihjjkwdrxxjnuwevlqtqmh3iegcadc32sio4wmo7bv2gbf34qs34a/{id}.json") {
        _mint(msg.sender, Rock, 1, "");
        _mint(msg.sender, Paper, 1, "");
        _mint(msg.sender, Scissors, 1, "");
    }
}
```

Explanation of the code above:

Line 1: Specifying [SPDX license](https://spdx.org/licenses/) type, which is added after Solidity ^0.6.8. Whenever the source code of a smart contract is made available to the public, these licenses can help resolve/avoid copyright issues. If you do not wish to specify any license type, you can use a special value UNLICENSED or simply skip the whole comment (it will not result in an error, just a warning).

Line 2: Declaring the Solidity version.

Line 4: Importing the OpenZeppelin ERC1155 contract.

Lines 6-9: Creating our contract named **rockPaperScissors** and creating three variables **Rock**, **Paper**, and **Scissors**; then assigning proper id to each one of them.

Lines 11-15: Initializing constructor with the link to our car file as a parameter, minting different NFT collections with parameters:

* Address on which tokens will be minted to, **msg.sender** here means the deployer of the contract.
* Token id, we have already assigned names to token id, so using names here.
* Quantity of each token.
* The last is the data field which is kept empty here.

Compile the contract, go to the third tab on the left menu, select **Injected Web3** as environment and deploy it by choosing the proper contract name:

Approve the transaction from MetaMask. Once the transaction is complete, your contract will be deployed.

Now you can perform functions like getting the balance of the token by entering the address and token id. We can also retrieve the URI of the token by entering the token id.

OpenSea does not support the returned URI format. So we will need to overwrite the URI function to return the file name as a string:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";
import "@openzeppelin/contracts/utils/Strings.sol";

contract rockPaperScissors is ERC1155 {
    uint256 public constant Rock = 1;
    uint256 public constant Paper = 2;
    uint256 public constant Scissors = 3;

    constructor() ERC1155("https://ipfs.io/ipfs/bafybeihjjkwdrxxjnuwevlqtqmh3iegcadc32sio4wmo7bv2gbf34qs34a/{id}.json") {
        _mint(msg.sender, Rock, 1, "");
        _mint(msg.sender, Paper, 1, "");
        _mint(msg.sender, Scissors, 1, "");
    }

    function uri(uint256 _tokenid) override public pure returns (string memory) {
        return string(
            abi.encodePacked(
                "https://ipfs.io/ipfs/bafybeihjjkwdrxxjnuwevlqtqmh3iegcadc32sio4wmo7bv2gbf34qs34a/",
                Strings.toString(_tokenid),".json"
            )
        );
    }
}
```

Additions:

Line 5: Importing an OpenZeppelin contract to convert Integer to String.

Lines 18-25: Overriding the URI function by creating a custom URI function and converting token if from integer to string, then returning the complete URI.

Recompile the contract and deploy it. When you query the contract for URI now, it will return a format supported by OpenSea.

### Conclusion[​](broken-reference) <a href="#conclusion" id="conclusion"></a>

Congratulations on deploying your EDX1155 tokens. If you made it here now you know about the EDX1155 multi-token standard and how to create and deploy EDX1155 NFTs.

&#x20;If you have any feedback, You can always chat with us on our [Discord](https://discord.gg/ahckhyA) community server, featuring some of the coolest developers you'll ever meet :)
