
# Build Your Own Decentralized Exchange (DEX) Like Uniswap v1

In this project, I built a decentralized exchange (DEX) that allows users to swap ETH for a custom token and vice versa.

## 📚 Overview

This DEX allows for:
- Swapping ETH and a specified ERC-20 token.
- Charging a 1% fee on swaps.
- Providing liquidity with LP tokens that represent the user’s share in the liquidity pool.
- Users can burn their LP tokens to receive back ETH and the specified token.

## 🚀 Getting Started

To run this DEX locally, follow these instructions:

### Prerequisites
Make sure you have the following installed:
- [Foundry](https://book.getfoundry.sh/) (for smart contract development)
- [Node.js](https://nodejs.org/) (for any additional setup)

### Installation Steps
1. **Create a New Folder for the Project**
   ```bash
   mkdir dex-app
   cd dex-app
   ```

2. **Initialize Foundry**
   ```bash
   forge init foundry-app
   cd foundry-app
   ```

3. **Install OpenZeppelin Contracts**
   ```bash
   forge install OpenZeppelin/openzeppelin-contracts
   ```

4. **Create a `.env` File**
   In the `foundry-app` folder, create a `.env` file with the following placeholders:
   ```bash
   PRIVATE_KEY="..."
   RPC_URL="..."
   ETHERSCAN_API_KEY="..."
   ```

5. **Set Up Environment Variables**
   - For `PRIVATE_KEY`, export it from MetaMask (ensure it's from a test account).
   - For `RPC_URL`, create an account at [QuickNode](https://www.quicknode.com/) and set up an endpoint for the Sepolia testnet.
   - For `ETHERSCAN_API_KEY`, create an account on [Etherscan](https://etherscan.io/) to get your API key.

### Writing Smart Contracts

1. **Create the ERC-20 Token Contract**
   Create a new file named `Token.sol` under `foundry-app/src` and implement a simple ERC-20 token:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.25;

   import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

   contract Token is ERC20 {
       constructor() ERC20("Token", "TKN") {
           _mint(msg.sender, 1000000 * 10 ** decimals());
       }
   }
   ```

2. **Create the Exchange Contract**
   Create another file named `Exchange.sol` under `foundry-app/src` and implement the exchange logic.

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.25;

   import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

   contract Exchange is ERC20 {
       address public tokenAddress;

       constructor(address token) ERC20("ETH TOKEN LP Token", "lpETHTOKEN") {
           require(token != address(0), "Token address cannot be zero");
           tokenAddress = token;
       }

       // Define additional functions here for addLiquidity, removeLiquidity, etc.
   }
   ```

### Running and Deploying the DEX

1. **Load Environment Variables**
   Load your environment variables in the terminal:
   ```bash
   source .env
   ```

2. **Deploy the Token Contract**
   ```bash
   forge create --rpc-url $RPC_URL --private-key $PRIVATE_KEY --etherscan-api-key $ETHERSCAN_API_KEY src/Token.sol:Token
   ```

3. **Deploy the Exchange Contract**
   Copy the address of the deployed Token contract and run:
   ```bash
   forge create --rpc-url $RPC_URL --private-key $PRIVATE_KEY --constructor-args <Token_Contract_Address> --etherscan-api-key $ETHERSCAN_API_KEY src/Exchange.sol:Exchange
   ```

### Testing the DEX
1. **Adding Liquidity**
   Use Etherscan to approve the Exchange contract to spend your tokens and add liquidity through the contract interface.

2. **Swapping Tokens**
   Use the contract functions on Etherscan to perform ETH-to-Token and Token-to-ETH swaps.

3. **Removing Liquidity**
   Check your LP token balance and remove liquidity through the contract interface.

## 🌈 Conclusion

This project successfully demonstrates how to build a decentralized exchange similar to Uniswap v1, incorporating important concepts of automated market makers (AMMs) and ERC-20 token standards. 

Feel free to explore the repository, and don't hesitate to reach out if you have any questions or want to collaborate! 🙌

---

### References
- [LearnWeb3 - Build Your Own Decentralized Exchange](https://learnweb3.io/courses/sophomore/build-your-own-decentralized-exchange-like-uniswap-v1)
