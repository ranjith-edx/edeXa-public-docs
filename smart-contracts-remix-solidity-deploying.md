# smart contracts remix solidity deploying

smart contracts remix solidity deploying

I guess you are as excited as us to deploy and interact with your first smart contract on the edeXa blockchain.

Don’t worry, as it’s our first smart contract, we’ll deploy it on a local test network so it does not cost anything for you to deploy and play as much as you’d like with it.

### Writing our contract

First step is to [visit Remix(opens in a new tab)](https://remix.ethereum.org/) and create a new file. On the upper left part of the Remix interface add a new file and enter the file name you want.

![Adding a new file in the Remix interface](https://d33wubrfki0l68.cloudfront.net/173b107cf54b49ac080ab43cf6d3139afb8a8fe5/484b7/static/0a30411b60d6dda25dfb8c133ee3df94/c1b63/remix.png)&#x20;

In the new file, we’ll paste the following code.

```
1// SPDX-License-Identifier: MIT
2 pragma solidity >=0.5.17;
3
4 contract Counter { 
5
6    // Public variable of type unsigned int to keep the number of counts
7    uint256 public count = 0;
8
9    // Function that increments our counter
10    function increment() public {
11        count += 1;
12    }
13
14    // Not necessary getter to get the count value
15    function getCount() public view returns (uint256) {
16        return count;
17    }
18
19 }
20

```

If you’re used to programming you can easily guess what this program does. Here is an explainer line by line:

* **Line 4**: We define a contract with the name `Counter`.
* **Line 7**: Our contract stores one unsigned integer named `count` starting at 0.
* **Line 10**: The first function will modify the state of the contract and `increment()` our variable `count`.
* **Line 15**: The second function is just a getter to be able to read the value of the `count` variable outside of the smart contract. Note that, as we defined our `count` variable as public this is not necessary but is shown as an example.

This is all for our first simple smart contract. As you may know, it looks like a class from OOP (Object-Oriented Programming) languages like Java or C++. It’s now time to play with our contract.

### Deploying our contract

As we wrote our very first smart contract, we’ll now deploy it to the blockchain to be able to play with it.

Deploying the smart contract on the blockchain is actually just sending a transaction containing the code of the compiled smart contract without specifying any recipients.

We’ll first compile the contract by clicking on the compile icon on the left hand side:

![The compile icon in the Remix toolbar](https://d33wubrfki0l68.cloudfront.net/b7f40db50426517470ba4c596580dc3de0ce3c82/56f4d/static/6fd0898b122d5126282251a22247319c/39d76/remix-compile-button.png)&#x20;

Then click on the compile button:

![The compile button in the Remix solidity compiler](https://d33wubrfki0l68.cloudfront.net/31ce452c41021bffd2dcb5e064b0373416c5cbc5/69454/static/b880ab53901e8bcd64a7e0af42ec5dd4/a2d48/remix-compile.png)&#x20;

You can choose to select the “Auto compile” option so the contract will always be compiled when you save the content on the text editor.

Then navigate to the deploy and run transactions screen:

![The deploy icon in the Remix toolbar](https://d33wubrfki0l68.cloudfront.net/c161b3f606bbb566ba49ad9ccefd735438da02e7/815ec/static/4c224fed8caab2d19cd6d021e674f598/9fc4b/remix-deploy.png)&#x20;

Once you are on the "deploy and run" transactions screen, double check that your contract name appears and click on Deploy. As you can see on the top of the page, the current environment is “JavaScript VM” that means that we’ll deploy and interact with our smart contract on a local test blockchain to be able to test faster and without any fees.

![The deploy button in the Remix solidity compiler](https://d33wubrfki0l68.cloudfront.net/7bdbfd976eaa8604ed14831a82a232fcb626328b/dfec4/static/f42cf3639030e0c2b1f7c64144ae1403/99f37/remix-deploy-button.png)&#x20;

Once you've clicked the “Deploy” button, you’ll see your contract appear on the bottom. Click the arrow on the left to expand it so we’ll see the content of our contract. This is our variable `counter`, our `increment()` function and the getter `getCounter()`.

If you click on the `count` or `getCount` button, it will actually retrieve the content of the contract’s `count` variable and display it. As we did not called the `increment` function yet, it should display 0.

![The function button in the Remix solidity compiler](https://d33wubrfki0l68.cloudfront.net/dc7e9db53a213edef47520435ef53eef20c42dc4/82615/static/a2c0da7c675f4351fc47581b96d0ddea/e0cdb/remix-function-button.png)&#x20;

Let’s now call the `increment` function by clicking on the button. You’ll see logs of the transactions that are made appearing on the bottom of the window. You’ll see that the logs are different when you’re pressing the button to retrieve the data instead of the `increment` button. It’s because reading data on the blockchain does not need any transactions (writing) or fees. Because only modifying the state of the blockchain requires to make a transaction:

![A log of transactions](https://d33wubrfki0l68.cloudfront.net/56abe3af1e09a3126fa1d43f396297a858afccf0/e536a/static/89547ad6de5bb325ac1de7fc091ef6ef/dc616/transaction-log.png)&#x20;

After pressing the increment button that will generate a transaction to call our `increment()` function if we click back on the count or getCount buttons we’ll read the newly updated state of our smart contract with the count variable being bigger than 0.

![Newly updated state of the smart contract](https://d33wubrfki0l68.cloudfront.net/eca9a06937c682a1abd27e0e49007e726b0e4285/07597/static/9dc35d6a094909894e2b0b59ff47ea8a/46684/updated-state.png)&#x20;

In the next tutorial, we’ll cover how you can add events to your smart contracts. Logging events is a convenient way to debug your smart contract and understand what is happening while calling a function.

#### Was this page helpful?
