
# Build Your Own Basic NFT Contract on Ethereum

This project demonstrates how to build a simple NFT contract on the Ethereum network using Foundry and OpenZeppelin Contracts.

## 📚 Overview

In this tutorial, you will learn how to:
- Write a Solidity contract for an NFT.
- Deploy the contract to the Sepolia test network using Foundry.

## 🚀 Getting Started

To build and deploy your NFT contract, follow these steps:

### Prerequisites
- Make sure you have [MetaMask](https://metamask.io/) installed and set up.
- Request some Sepolia Testnet ETH from a faucet. You can use the [Ethereum Sepolia Faucet](https://learnweb3.io/faucets/sepolia).

### Installing Foundry

1. **Install Foundry**  
   For Mac or Linux users, run the following command in your terminal:
   ```bash
   curl -L https://foundry.paradigm.xyz | bash
   ```

2. **Set Up Foundry**  
   Once Foundry is installed, confirm the installation by checking the versions:
   ```bash
   rustc --version
   cargo --version
   ```

3. **Create a New Foundry Project**
   Create a new folder for your NFT tutorial and set it up:
   ```bash
   mkdir NFT-Tutorial
   cd NFT-Tutorial
   forge init foundry-app
   ```

4. **Installing OpenZeppelin Contracts**
   Install the OpenZeppelin Contracts library:
   ```bash
   cd foundry-app
   forge install OpenZeppelin/openzeppelin-contracts
   ```

### Writing the NFT Contract

1. **Create the NFT Contract**
   Create a new Solidity file named `NFTee.sol` inside the `src` folder and write the following code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.24;

   import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

   contract NFTee is ERC721 {
       constructor() ERC721("NFTee", "ITM") {
           // Mint an NFT to yourself
           _mint(msg.sender, 1);
       }
   }
   ```

2. **Configure Remappings**
   Configure remappings for your project by running:
   ```bash
   forge remappings > remappings.txt
   ```

### Deploying the Contract

1. **Create a `.env` File**
   Inside the `foundry-app` folder, create a `.env` file and add the following lines:
   ```bash
   QUICKNODE_RPC_URL="..."
   PRIVATE_KEY="..."
   ```

2. **Set Up QuickNode**
   - Sign up for an account at [QuickNode](https://www.quicknode.com/).
   - Create an endpoint for the Ethereum Sepolia network and copy the HTTP Provider link.

3. **Get Your Private Key**
   Export your private key from MetaMask for the account that will deploy the contract.

4. **Load Environment Variables**
   Load your environment variables in the terminal:
   ```bash
   source .env
   ```

5. **Deploy the Contract**
   Deploy your NFT contract using:
   ```bash
   forge create --rpc-url $QUICKNODE_RPC_URL --private-key $PRIVATE_KEY src/NFTee.sol:NFTee
   ```

   After the deployment, copy the contract address and check it on [Sepolia Etherscan](https://sepolia.etherscan.io/).

## 🌈 Conclusion

Congratulations! You have successfully built and deployed your first NFT contract on the Ethereum test network. This project demonstrates the process of creating a simple NFT using Foundry and OpenZeppelin.

Feel free to explore the repository, and don't hesitate to reach out if you have any questions or want to collaborate! 🙌

---

### References
- [LearnWeb3 - Build Your Own Basic NFT Contract](https://learnweb3.io/courses/freshman/build-your-own-basic-nft-contract-on-ethereum)
