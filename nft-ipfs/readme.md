
# Build Your Own NFT Collection with Metadata Stored on IPFS

This project demonstrates how to build an NFT collection and store its metadata on IPFS. You will learn how to create a unique NFT collection, deploy it on the Ethereum Sepolia test network, and connect it with a front-end interface.

## 📚 Overview

In this tutorial, you will learn how to:
- Create an NFT collection with unique items.
- Store metadata for each NFT on IPFS.
- Deploy the contract to the Sepolia test network.

## 🚀 Getting Started

To build and deploy your NFT collection, follow these steps:

### Prerequisites
- Complete the [IPFS Theory tutorial](https://learnweb3.io/lessons/introduction-to-ipfs-the-inter-planetary-file-system).
- Have [MetaMask](https://metamask.io/) installed and set up.
- Request some Sepolia Testnet ETH from a faucet. You can use the [Ethereum Sepolia Faucet](https://learnweb3.io/faucets/sepolia).

### Uploading Images and Metadata to IPFS

1. **Upload to IPFS using Pinata**
   - Sign up for an account on [Pinata](https://www.pinata.cloud/).
   - Download the LW3Punks folder and upload it to Pinata.
   - Note the CID provided for the folder.

2. **Upload Metadata Files**
   - Create JSON metadata files for each NFT (as shown below) and upload them to Pinata:
   ```json
   {
     "name": "1",
     "description": "NFT Collection for LearnWeb3 Students",
     "image": "ipfs://CID-OF-THE-LW3Punks-Folder/NFT-1.png"
   }
   ```

### Setting Up the Contract

1. **Create a New Foundry Project**
   Open your terminal and run:
   ```bash
   mkdir nft-ipfs
   cd nft-ipfs
   forge init foundry-app
   ```

2. **Install OpenZeppelin Contracts**
   Install the OpenZeppelin Contracts library:
   ```bash
   cd foundry-app
   forge install OpenZeppelin/openzeppelin-contracts
   ```

3. **Create the NFT Contract**
   Create a new file named `LW3Punks.sol` inside the `src` directory and add the following code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.25;

   import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
   import "@openzeppelin/contracts/access/Ownable.sol";
   import "@openzeppelin/contracts/utils/Strings.sol";

   contract LW3Punks is ERC721Enumerable, Ownable {
       using Strings for uint256;
       string _baseTokenURI;
       uint256 public _price = 0.01 ether;
       bool public _paused;
       uint256 public maxTokenIds = 10;
       uint256 public tokenIds;

       modifier onlyWhenNotPaused {
           require(!_paused, "Contract currently paused");
           _;
       }

       constructor(string memory baseURI) ERC721("LW3Punks", "LW3P") {
           _baseTokenURI = baseURI;
       }

       function mint() public payable onlyWhenNotPaused {
           require(tokenIds < maxTokenIds, "Exceed maximum LW3Punks supply");
           require(msg.value >= _price, "Ether sent is not correct");
           tokenIds += 1;
           _safeMint(msg.sender, tokenIds);
       }

       function _baseURI() internal view virtual override returns (string memory) {
           return _baseTokenURI;
       }

       function setPaused(bool val) public onlyOwner {
           _paused = val;
       }

       function withdraw() public onlyOwner {
           address _owner = owner();
           uint256 amount = address(this).balance;
           (bool sent, ) = _owner.call{value: amount}("");
           require(sent, "Failed to send Ether");
       }

       receive() external payable {}
       fallback() external payable {}
   }
   ```

### Deploying the Contract

1. **Create a `.env` File**
   Inside the `foundry-app` folder, create a `.env` file and add the following lines:
   ```bash
   QUICKNODE_HTTP_URL="add-quicknode-http-provider-url-here"
   PRIVATE_KEY="add-the-private-key-here"
   ```

2. **Set Up QuickNode**
   - Sign up at [QuickNode](https://www.quicknode.com/) and create an endpoint for Ethereum Sepolia.
   - Copy the HTTP Provider link and update your `.env` file.

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
   forge create --rpc-url $QUICKNODE_HTTP_URL --constructor-args "YOUR-METADATA-CID" --private-key $PRIVATE_KEY src/LW3Punks.sol:LW3Punks
   ```

### Setting Up the Frontend

1. **Create a Next.js App**
   In the terminal pointing to your `nft-ipfs` folder, run:
   ```bash
   npx create-next-app@latest next-app
   cd next-app
   ```

2. **Install Required Packages**
   Install RainbowKit and other dependencies:
   ```bash
   npm install @rainbow-me/rainbowkit wagmi viem@2.x @tanstack/react-query
   ```

3. **Configure Your App**
   Update your app to include the necessary components and connect to your NFT contract.

4. **Run Your App**
   Start your application:
   ```bash
   npm run dev
   ```

5. **Visit Your App**
   Open [http://localhost:3000](http://localhost:3000) in your browser to see your NFT collection!

## 🌈 Conclusion

Congratulations! You have built and deployed your own NFT collection with metadata stored on IPFS. This project demonstrates the integration of blockchain technology, IPFS, and web development.

Feel free to explore the repository, and don't hesitate to reach out if you have any questions or want to collaborate! 🙌

---

### References
- [LearnWeb3 - Build Your Own NFT Collection with Metadata Stored on IPFS](https://learnweb3.io/courses/junior/build-your-own-nft-collection-with-metadata-stored-on-ipfs)
