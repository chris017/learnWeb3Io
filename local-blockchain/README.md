
# Testing Smart Contracts on a Local Blockchain Node Using Foundry

The tutorial covers setting up a local blockchain, deploying a sample smart contract, and interacting with it via MetaMask and Remix.

## 📚 Overview

Using a local blockchain for testing smart contracts offers several advantages:
- **Speed**: Testing is much faster since it runs on your machine without needing to wait for other nodes.
- **Efficiency**: Local consensus is quick, and you can utilize specialized tools designed for local testing, like the `console.log` feature from the Forge standard library.

## 🚀 Getting Started

To test smart contracts locally using Foundry, follow these steps:

### Prerequisites
Make sure you have the following installed:
- [Foundry](https://book.getfoundry.sh/) (for smart contract development)

### Setup Steps

1. **Initialize a Foundry Project**
   Open your terminal and execute:
   ```bash
   forge init foundry-app
   cd foundry-app
   ```

2. **Create a Sample Smart Contract**
   Create a new file named `Greeter.sol` in the `src` directory and add the following code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.25;

   import "forge-std/console.sol";

   contract Greeter {
       string private greeting;

       constructor(string memory _greeting) {
           console.log("Deploying a Greeter with greeting:", _greeting);
           greeting = _greeting;
       }

       function greet() public view returns (string memory) {
           return greeting;
       }

       function setGreeting(string memory _greeting) public {
           console.log("Changing greeting from '%s' to '%s'", greeting, _greeting);
           greeting = _greeting;
       }

       receive() external payable {}
       fallback() external payable {}
   }
   ```

### Running the Local Blockchain

1. **Start the Local Blockchain Node**
   In your terminal, run:
   ```bash
   anvil --hardfork cancun
   ```

2. **Deploy the Smart Contract**
   Deploy your smart contract using the following command:
   ```bash
   forge create --rpc-url http://127.0.0.1:8545 --private-key <YOUR_PRIVATE_KEY> src/Greeter.sol:Greeter
   ```

### Connecting MetaMask

1. **Set Up MetaMask**
   - Open MetaMask and click on your profile icon.
   - Select the top-left network dropdown, then click "Add Network".
   - Fill in the following details:
     - **Network Name**: Localhost 8545
     - **New RPC URL**: http://127.0.0.1:8545
     - **Chain ID**: 31337
     - **Currency Symbol**: ETH

2. **Import Account**
   - Copy one of the accounts displayed in your terminal running Anvil.
   - In MetaMask, go to "Import Account" and paste the private key.

### Interacting with the Smart Contract

1. **Using Remix**
   - Go to [Remix IDE](https://remix.ethereum.org).
   - Create a new file named `Greeter.sol` and paste the same code you used earlier.
   - Compile the contract.
   - Under the deployment tab, select "Injected Provider - MetaMask".
   - Deploy the contract by providing an initial greeting value.

2. **Set and Get Greeting**
   - Use the `setGreeting` function to change the greeting.
   - Call the `greet` function to retrieve the current greeting.

## 🌈 Conclusion

This project illustrates the process of testing smart contracts on a local blockchain using Foundry, providing a fast and efficient environment for development and debugging.

Feel free to explore the repository, and don't hesitate to reach out if you have any questions or want to collaborate! 🙌

---

### References
- [LearnWeb3 - Testing Smart Contracts on a Local Blockchain Node Using Foundry](https://learnweb3.io/courses/junior/testing-smart-contracts-on-a-local-blockchain-node-using-foundry)
