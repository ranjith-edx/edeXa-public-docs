# Generate a New Edexa Address in JavaScript

### How to Generate a New edeXa Address in JavaScript

_4 min read_

### Overview[​](broken-reference) <a href="#overview" id="overview"></a>

When it comes to programming, there’s hardly anyone who has not used or heard about JavaScript. [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) was initially created for client-side scripting but has become a full-featured Object-Oriented and procedural language widely used for client and server applications today. JavaScript’s syntax borrows from Java and C++, and it is based on the [ECMAScript](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/) specification. With over 1,444,231 libraries and counting, JavaScript today is practically used everywhere, including making dApps (Decentralized Applications) on Ethereum. In this guide, we will cover creating an Edexa address in JavaScript using [ethers.js](https://docs.ethers.io/v5/).

**Prerequisites**

* NodeJS installed on your system.
* A text editor
* Terminal aka Command Line

### What is an edeXa address?[​](broken-reference) <a href="#what-is-an-ethereum-address" id="what-is-an-ethereum-address"></a>

While signing in to any platform on the internet, you need to authenticate using a combination of credentials. Consider an edeXa address as your username and a corresponding private key as the password. While your edeXa address is public and can be shared, the private key must always be kept secret. Using this combination lets you interact with the edeXa blockchain. An edeXa address is your identity on the blockchain, and it looks like this “0x6E0d01A76C3Cf4288372a29124A26D4353EE51BE”. Having a valid edeXa address is required for:

* Receiving/Sending edeXa currency
* Signing/Sending transactions
* Connecting to decentralized applications

### How an edeXa address is generated:[​](broken-reference) <a href="#how-an-ethereum-address-is-generated" id="how-an-ethereum-address-is-generated"></a>

* A random private key of 64 (hex) characters (256 bits / 32 bytes) is generated first. \
  For example:&#x20;

```
0xf4a2b939592564feb35ab10a8e04f6f2fe0943579fb3c9c33505298978b74893
```

* &#x20;A 128 (hex) character (64 bytes) public key is then derived from the generated private key using Elliptic Curve Digital Signature Algorithm ([ECDSA](https://hackernoon.com/a-closer-look-at-ethereum-signatures-5784c14abecc)). \
  For example:&#x20;

```
0x04345f1a86ebf24a6dbeff80f6a2a574d46efaa3ad3988de94aa68b695f09db9ddca37439f99548da0a1fe4acf4721a945a599a5d789c18a06b20349e803fdbbe3
```

* The [Keccak-256](https://keccak.team/keccak.html) hash function is then applied to (128 characters / 64 bytes) the public key to obtain a 64 character (32 bytes) hash string. The last 40 characters / 20 bytes of this string prefixed with 0x become the final edeXa address. \
  For example:

```
0xd5e099c71b797516c10ed0f0d895f429c2781142
```

> **Note**: 0x in coding indicates that the number/string is written in hex.

### What is ethers.js?[​](broken-reference) <a href="#what-is-ethersjs" id="what-is-ethersjs"></a>

ethers.js is a lightweight alternative to [Web3.js](https://web3js.readthedocs.io/en/v1.3.0/), which is the most commonly used Ethereum library today. Ethers.js is considered by some to be more stable and less buggy than other libraries and has extensive documentation. This library is also very friendly to beginners. Ethers.js is very well maintained and is preferred over Web3.js by many new developers.

You can learn more about ethers.js[ here.](https://www.quicknode.com/guides/web3-sdks/how-to-connect-to-ethereum-network-with-ethers-js)

### Generating an edeXa address in JavaScript[​](broken-reference) <a href="#generating-an-ethereum-address-in-javascript" id="generating-an-ethereum-address-in-javascript"></a>

Our first step here will be to check if node.js is installed on the system. To do so, copy-paste the following in your terminal/cmd:

If not installed, you can download the LTS version of NodeJS from the [official website](https://nodejs.org/en/).

If node.js is properly installed, let’s add the ethers.js library using npm (Node Package Manager, a part of node.js).

The most common issue at this step is an internal failure with \`node-gyp.\` You can follow [node-gyp installation instructions here](https://github.com/nodejs/node-gyp#installation).

> **Note**: You will need to have your python version match one of the compatible versions listed in the instructions above if you encounter the node-gyp issue.&#x20;

Another common issue is a stale cache; clear your npm cache by entering the following command:

If ethers.js is successfully installed, let’s proceed with creating an edeXa address.

Create a file named address.js, which will be a short script to create a random private key and an edeXa address from that key, copy-paste the following in your address.js file:

```
var ethers = require('ethers');  
var crypto = require('crypto');

var id = crypto.randomBytes(32).toString('hex');
var privateKey = "0x"+id;
console.log("SAVE BUT DO NOT SHARE THIS:", privateKey);

var wallet = new ethers.Wallet(privateKey);
console.log("Address: " + wallet.address);
```

Explanation of the code above

Line 1-2: Importing ethers library and crypto module that comes with node.js

Line 4: Generating a random 32 bytes hexadecimal string using the crypto object and storing it in the _id_ variable.

Line 4: Adding ‘0x’ prefix to the string in _id_ and storing the new string in a variable called _privateKey_.

Line 6: Printing our private key with a warning.

Line 8: Creating a new wallet using the _privateKey_ and storing it in the _wallet_ variable.

Line 9: Printing the address of the newly created _wallet_ with a message “Address:”

Save and run your script to generate a new edeXa address.

If your code executes successfully, the output will look similar to the screenshot below. The first line consists of the private key, and the second line consists of your new edeXa address.

### Conclusion[​](broken-reference) <a href="#conclusion" id="conclusion"></a>

Your edeXa address is your identity on the edeXa network. It is required to interact with the network and perform transactions. To continue learning ethers.js, Get more information on ethers.js from their [official documentation](https://docs.ethers.io/v5/). As you saw, generating a new edeXa address is quickly done with JavaScript and the latest libraries. The development of dApps on the edeXa blockchain is supported by a variety of tools that are continuously updated and improved by the fast-growing edeXa community. Look out for more easy-to-follow guides from edeXa labs - your provider of affordable and lightning-fast edeXa nodes.

If you have any feedback, You can always chat with us on our [Discord](https://discord.gg/Pa523yUk) community server, featuring some of the coolest developers you’ll ever meet :)
