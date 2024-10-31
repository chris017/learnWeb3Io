
# Build an NFT-Powered Fully On-Chain DAO to Invest in NFT Collections as a Group

This project demonstrates how to build a decentralized autonomous organization (DAO) powered by NFTs, allowing NFT holders to propose and vote on investments in NFT collections.

## 📚 Overview

In this tutorial, you will learn how to:
- Create a DAO that allows NFT holders to vote on proposals.
- Implement an on-chain NFT marketplace.
- Deploy the contract to the Sepolia test network.

## 🚀 Getting Started

To build and deploy your DAO, follow these steps:

### Prerequisites
- Have [MetaMask](https://metamask.io/) installed and set up.
- Request some Sepolia Testnet ETH from a faucet. You can use the [Ethereum Sepolia Faucet](https://learnweb3.io/faucets/sepolia).
- Ensure that Foundry is installed on your machine.

### Setting Up the Project

1. **Create a New Folder for the Project**  
   Open your terminal and run:
   ```bash
   mkdir onchain-dao
   cd onchain-dao
   mkdir foundry
   cd foundry
   forge init
   forge install openzeppelin/openzeppelin-contracts
   forge remappings > remappings.txt
   ```

### Writing the NFT Contract

1. **Create the NFT Contract**  
   Create a new file named `CryptoDevsNFT.sol` under `foundry/src` and write the following code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.26;

   import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";

   contract CryptoDevsNFT is ERC721Enumerable {
       constructor() ERC721("CryptoDevs", "CD") {}

       function mint() public {
           _safeMint(msg.sender, totalSupply());
       }
   }
   ```

### Creating the Fake NFT Marketplace

1. **Create the Marketplace Contract**  
   Create a new file named `FakeNFTMarketplace.sol` under `foundry/src` and add the following code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.18;

   contract FakeNFTMarketplace {
       mapping(uint256 => address) public tokens;
       uint256 nftPrice = 0.1 ether;

       function purchase(uint256 _tokenId) external payable {
           require(msg.value == nftPrice, "This NFT costs 0.1 ether");
           tokens[_tokenId] = msg.sender;
       }

       function getPrice() external view returns (uint256) {
           return nftPrice;
       }

       function available(uint256 _tokenId) external view returns (bool) {
           return tokens[_tokenId] == address(0);
       }
   }
   ```

### Writing the DAO Contract

1. **Create the DAO Contract**  
   Create a new file named `CryptoDevsDAO.sol` under `foundry/src` and add the following initial code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.9;

   import "@openzeppelin/contracts/access/Ownable.sol";

   contract CryptoDevsDAO is Ownable {
       // Proposal structure and other functionalities will go here
   }

   interface IFakeNFTMarketplace {
       function getPrice() external view returns (uint256);
       function available(uint256 _tokenId) external view returns (bool);
       function purchase(uint256 _tokenId) external payable;
   }

   interface ICryptoDevsNFT {
       function balanceOf(address owner) external view returns (uint256);
       function tokenOfOwnerByIndex(address owner, uint256 index) external view returns (uint256);
   }
   ```

2. **Define the Proposal Structure**  
   Inside the `CryptoDevsDAO` contract, add the following struct for proposals:

   ```solidity
   struct Proposal {
       uint256 nftTokenId;
       uint256 deadline;
       uint256 yayVotes;
       uint256 nayVotes;
       bool executed;
       mapping(uint256 => bool) voters;
   }

   mapping(uint256 => Proposal) public proposals;
   uint256 public numProposals;
   ```

3. **Implement Proposal Creation, Voting, and Execution**  
   Add the functions to create proposals, vote, and execute proposals. See the tutorial for the complete implementation.

### Environment Variables

1. **Create a `.env` File**  
   Inside the `foundry` folder, create a `.env` file with the following placeholders:
   ```bash
   PRIVATE_KEY="..."
   RPC_URL="..."
   ETHERSCAN_API_KEY="..."
   ```

### Deploying the Contracts

1. **Deploy the NFT Contract**  
   Use the following command to deploy the NFT contract:
   ```bash
   forge create --rpc-url $RPC_URL --private-key $PRIVATE_KEY --etherscan-api-key $ETHERSCAN_API_KEY --verify src/CryptoDevsNFT.sol:CryptoDevsNFT
   ```

2. **Deploy the Fake NFT Marketplace**  
   Deploy the marketplace contract:
   ```bash
   forge create --rpc-url $RPC_URL --private-key $PRIVATE_KEY --etherscan-api-key $ETHERSCAN_API_KEY --verify src/FakeNFTMarketplace.sol:FakeNFTMarketplace
   ```

3. **Deploy the DAO Contract**  
   Finally, deploy the DAO contract:
   ```bash
   forge create --rpc-url $RPC_URL --private-key $PRIVATE_KEY --constructor-args <Marketplace Address> <NFT Address> --etherscan-api-key $ETHERSCAN_API_KEY --verify src/CryptoDevsDAO.sol:CryptoDevsDAO
   ```

### Frontend Development

1. **Create a Next.js App**  
   Go back to the `onchain-dao` directory and create a new Next.js project:
   ```bash
   npx create-next-app@latest frontend
   ```

2. **Set Up Wallet Connection**  
   Install necessary packages and set up wallet connections using RainbowKit, Wagmi, and Viem.

3. **Implementing the DAO Interface**  
   Create components to allow users to create and vote on proposals, as well as to view all proposals.

## 🌈 Conclusion

Congratulations! You have built an NFT-powered fully on-chain DAO to invest in NFT collections. This project showcases the integration of blockchain technology and decentralized governance.

Feel free to explore the repository, and don't hesitate to reach out if you have any questions or want to collaborate! 🙌

---

### References
- [LearnWeb3 - Build an NFT-Powered Fully On-Chain DAO](https://learnweb3.io/courses/sophomore/build-an-nft-powered-fully-on-chain-dao-to-invest-in-nft-collections-as-a-group)
