# Ethereum Virtual Machine (EVM)?

### What is the Ethereum Virtual Machine (EVM)?

_5 min read_

### Overview[​](broken-reference) <a href="#overview" id="overview"></a>

The Ethereum Virtual Machine (EVM) is a core piece of Ethereum that helps power the blockchain and smart contracts. It is vital in assisting Ethereum to achieve user adoption and decentralization. In this guide, we will take a detailed look at the EVM to learn what the EVM is and how it operates. Then, we will cover some key points to solidify our understanding.

**What You Will Need**[**​**](broken-reference)

* Basic knowledge of [Ethereum](https://www.quicknode.com/guides/ethereum-development/what-is-ethereum)
* Basic understanding of data structures and memory

**What You Will Learn**[**​**](broken-reference)

* What the Ethereum Virtual Machine (EVM) is
* How the Ethereum Virtual Machine (EVM) operates
* Key takeaways about the Ethereum Virtual Machine (EVM)

### What is the EVM?[​](broken-reference) <a href="#what-is-the-evm" id="what-is-the-evm"></a>

The Ethereum Virtual Machine (EVM) is the computation engine for Ethereum that manages the state of the blockchain and enables smart contract functionality. The EVM is contained within the client software (e.g., [Geth](https://geth.ethereum.org/), [Nethermind](https://nethermind.io/), and more) that you need in order to run a [node](https://www.quicknode.com/guides/infrastructure/ethereum-full-node-vs-archive-node) on Ethereum. Nodes on Ethereum keep copies of transaction data, which the EVM processes to update the distributed ledger. Generally speaking, nodes on Ethereum natively support the EVM as the client software implements this functionality.

Next, let us cover some of the tasks that the EVM performs.

The EVM participates in block creation and transaction execution. In block creation, the EVM sets standards for managing the state from block to block. These states are stored in a [Merkle Patricia Trie](https://ethereum.org/en/developers/docs/data-structures-and-encoding/patricia-merkle-trie/) and hold the ground truth state for Ethereum.

In transaction execution, the EVM executes tasks (e.g., function calls to a smart contract) by interpreting the instructions in [Opcodes](https://www.ethervm.io/) (machine instructions at a low level); however, the data is formatted in bytecode. To get the data into bytecode, you can use a programming language such as [Solidity](https://docs.soliditylang.org/en/latest/) (i.e., the native programming language for smart contracts) to compile and deploy the smart contract using bytecode.

Note that when the EVM executes tasks, it is limited to the amount of gas provided by the transaction and the overall limitations of the EVM. Gas is a unit of measure for computing power on Ethereum.

To learn more about transactions, check out our guide on [What are Ethereum Transactions?\
](https://www.quicknode.com/guides/ethereum-development/what-are-ethereum-transactions)

Next, let us take a look at the architecture of the EVM.

### How is the EVM designed?[​](broken-reference) <a href="#how-is-the-evm-designed" id="how-is-the-evm-designed"></a>

The EVM operates off of a [stack-based](https://en.wikipedia.org/wiki/Stack-based\_memory\_allocation) memory structure and contains memory components such as _Memory_, _Storage_, and _Stack_ (which are used to read/write to the blockchain and manage state).

The EVM is considered [_quasi-turing complete_](https://en.wikipedia.org/wiki/Turing\_completeness), which means it can solve problems given a set of instructions and inputs but is limited to the amount of gas provided along with the transaction.

Now, let us cover how the EVM operates internally when a transaction's bytecode is being executed or when a new block is being created.

### How does the EVM operate?[​](broken-reference) <a href="#how-does-the-evm-operate" id="how-does-the-evm-operate"></a>

In this section, we will cover how the EVM operates. Note that this is a high level explanation and doesn't cover every operation of the EVM.

As we mentioned earlier, the EVM executes tasks formatted in bytecode which the EVM interprets in Opcodes. Let us explain how smart contract code (bytecode) is broken down into Opcodes.

For this example, we will reference the Simple storage contract (1\_Storage.sol) on [Remix.IDE](https://remix.ethereum.org/). The simple storage contract's compiled bytecode looks like this:

```
608060405234801561001057600080fd5b50610150806100206000396000f3fe608060405234801561001057600080fd5b50600436106100365760003560e01c80632e64cec11461003b5780636057361d14610059575b600080fd5b610043610075565b60405161005091906100d9565b60405180910390f35b610073600480360381019061006e919061009d565b61007e565b005b60008054905090565b8060008190555050565b60008135905061009781610103565b92915050565b6000602082840312156100b3576100b26100fe565b5b60006100c184828501610088565b91505092915050565b6100d3816100f4565b82525050565b60006020820190506100ee60008301846100ca565b92915050565b6000819050919050565b600080fd5b61010c816100f4565b811461011757600080fd5b5056fea26469706673582212209a159a4f3847890f10bfb87871a61eba91c5dbf5ee3cf6398207e292eee22a1664736f6c63430008070033
```

> You can see the same bytecode by going to [Remix.IDE](https://remix.ethereum.org/), navigating to the example **1\_Storage.sol** contract, and then compiling and clicking the Bytecode copy button.

Note that the EVM only uses 140 unique opcodes; however, there are a total of 256 Opcodes which are 1 byte in length. Now let us look at the bytecode for one of the functions in the contract (e.g., **store()**).

```
60003560e01c80632e64cec11461003b5780636057361d1461005957
```

Each byte in the example above refers to a different Opcode. For example, the first byte (e.g., 60) refers to a PUSH1 opcode, the next byte (e.g., 00) refers to the data being pushed, the third byte (60) refers to the PUSH1 opcode again, and the next byte refers to its input (e.g., e0). To see all the Opcodes for the function above, you can go to [evm.codes/playground](https://www.evm.codes/playground) and input the bytecode into the IDE and click Run (make sure to select Bytecode in the dropdown.

Now that we know a bit more about how the EVM executes bytecode, let us move onto understanding the Ethereum state transition function.

**The Ethereum State Transition Function**\


The Ethereum state transition function is a formula that the EVM processes every time it executes a transaction. The purpose of the function is to make sure transactions adhere to the transaction standard and are technically valid (e.g., correct nonce and valid signature). The formula is as follows:

> In the formula above, **Y(S, T)** is the transition function that takes two arguments, **S**, which is the old state, and **T**, which is the new set of instructions. The output is signified as **S'**

You should now better understand how the EVM operates and is designed. Next, let us cover some key benefits of the EVM.

### Key Takeaways[​](broken-reference) <a href="#key-takeaways" id="key-takeaways"></a>

* **Turing complete**: Ethereum being Turing complete allows it to be able to run arbitrary logic, unlike other blockchains such as Bitcoin.
* **Composability**: Ethereum isn't the only EVM-compatible blockchain. Many Layer 1 (L1) and [Layer 2](https://ethereum.org/en/developers/docs/scaling/) (L2) chains are EVM-compatible, making it easy to migrate and deploy contracts or transfer tokens between chains. Some examples are [Polygon](https://polygon.technology/), [BNB Smart Chain](https://www.bnbchain.org/en), [Avalanche](https://www.avax.network/), and more.
* **Decentralization**: The EVM empowers developers to deploy dApps (decentralized applications) on Ethereum.

### Additional Resources[​](broken-reference) <a href="#additional-resources" id="additional-resources"></a>

If you want to dive deeper into the EVM, check out the following resources:

* [Ethereum Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf)
* [EVM Opcodes Reference](https://www.ethervm.io/)
* [Solidity and the EVM](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html#index-6)

Or if you're curious about creating your own smart contract, look at our [An overview of how smart contracts work on Ethereum](https://www.quicknode.com/guides/smart-contract-development/an-overview-of-how-smart-contracts-work-on-ethereum) guide.

### Final Thoughts[​](broken-reference) <a href="#final-thoughts" id="final-thoughts"></a>

Kudos! You now understand a bit more about the EVM. Got ideas or questions, or want to show off what you have learned? Tell us on [Discord](https://discord.gg/ahckhyA) .
