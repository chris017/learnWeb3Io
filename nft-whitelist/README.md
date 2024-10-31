
# Build an NFT Collection with a Whitelist Using Foundry and Solidity

This project demonstrates how to build an NFT collection with a whitelist using Foundry and Solidity. You will learn how to create a whitelist dApp, allowing early supporters to mint NFTs for free while others must pay.

## 📚 Overview

In this tutorial, you will learn how to:
- Create a whitelist for your NFT collection.
- Allow users to mint NFTs from the collection based on their whitelist status.
- Deploy the contract to the Sepolia test network.

## 🚀 Getting Started

To build and deploy your NFT collection with a whitelist, follow these steps:

### Prerequisites
- Have [MetaMask](https://metamask.io/) installed and set up.
- Request some Sepolia Testnet ETH from a faucet. You can use the [Ethereum Sepolia Faucet](https://learnweb3.io/faucets/sepolia).
- Ensure that Foundry is installed on your machine.

### Setting Up the Project

1. **Create a New Folder for the Project**  
   Open your terminal and run:
   ```bash
   mkdir whitelist-dapp
   cd whitelist-dapp
   forge init foundry-app
   ```

### Writing the Whitelist Contract

1. **Create the Whitelist Contract**  
   Open the `whitelist-dapp` folder in your code editor, and create a new file named `Whitelist.sol` inside `foundry-app/src`. Write the following code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.24;

   contract Whitelist {
       uint8 public maxWhitelistedAddresses;
       mapping(address => bool) public whitelistedAddresses;
       uint8 public numAddressesWhitelisted;

       constructor(uint8 _maxWhitelistedAddresses) {
           maxWhitelistedAddresses = _maxWhitelistedAddresses;
       }

       function addAddressToWhitelist() public {
           require(!whitelistedAddresses[msg.sender], "Sender has already been whitelisted");
           require(numAddressesWhitelisted < maxWhitelistedAddresses, "More addresses can't be added, limit reached");

           whitelistedAddresses[msg.sender] = true;
           numAddressesWhitelisted += 1;
       }
   }
   ```

### Environment Variables

1. **Create a `.env` File**  
   Inside the `foundry-app` folder, create a `.env` file and add the following placeholder lines:
   ```bash
   PRIVATE_KEY="..."
   QUICKNODE_RPC_URL="..."
   ETHERSCAN_API_KEY="..."
   ```

2. **Set Up QuickNode**  
   - Sign up for an account at [QuickNode](https://www.quicknode.com/).
   - Create an endpoint for Ethereum Sepolia and copy the HTTP Provider link.

3. **Get Your Private Key**  
   Export your private key from MetaMask for the account that will deploy the contract.

4. **Load Environment Variables**  
   Load your environment variables in the terminal:
   ```bash
   source .env
   ```

### Deploying the Whitelist Contract

1. **Deploy the Contract**  
   To deploy your Whitelist contract, run the following command:
   ```bash
   forge create --rpc-url $QUICKNODE_RPC_URL --private-key $PRIVATE_KEY --constructor-args 10 --etherscan-api-key $ETHERSCAN_API_KEY src/Whitelist.sol:Whitelist
   ```

### Adding Users to the Whitelist

1. **Interact with the Contract**  
   Go to Sepolia Etherscan, search for your contract address, and go to the Contract tab. Connect your wallet, and call the `addAddressToWhitelist` function to add addresses to the whitelist.

### Writing the NFT Contract

1. **Create the NFT Contract**  
   Create a new file named `CryptoDevs.sol` inside the `foundry-app/src` directory and add the following code:

   ```solidity
   // SPDX-License-Identifier: MIT
   pragma solidity ^0.8.24;

   import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Enumerable.sol";
   import "@openzeppelin/contracts/access/Ownable.sol";
   import "./Whitelist.sol";

   contract CryptoDevs is ERC721Enumerable, Ownable {
       uint256 constant public _price = 0.01 ether;
       uint256 constant public maxTokenIds = 20;
       Whitelist whitelist;
       uint256 public reservedTokens;
       uint256 public reservedTokensClaimed = 0;

       constructor (address whitelistContract) ERC721("Crypto Devs", "CD") Ownable(msg.sender) {
           whitelist = Whitelist(whitelistContract);
           reservedTokens = whitelist.maxWhitelistedAddresses();
       }

       function mint() public payable {
           require(totalSupply() + reservedTokens - reservedTokensClaimed < maxTokenIds, "EXCEEDED_MAX_SUPPLY");

           if (whitelist.whitelistedAddresses(msg.sender)) {
               require(balanceOf(msg.sender) == 0, "ALREADY_OWNED");
               reservedTokensClaimed += 1;
           } else {
               require(msg.value >= _price, "NOT_ENOUGH_ETHER");
           }
           uint256 tokenId = totalSupply();
           _safeMint(msg.sender, tokenId);
       }

       function withdraw() public onlyOwner {
           address _owner = owner();
           uint256 amount = address(this).balance;
           (bool sent, ) = _owner.call{value: amount}("");
           require(sent, "Failed to send Ether");
       }
   }
   ```

### Deploying the NFT Contract

1. **Deploy the Contract**  
   Replace `<your Whitelist Contract's address>` in the following command with your Whitelist contract address and run:
   ```bash
   forge create --rpc-url $QUICKNODE_RPC_URL --private-key $PRIVATE_KEY --constructor-args <your Whitelist Contract's address> --etherscan-api-key $ETHERSCAN_API_KEY --verify src/CryptoDevs.sol:CryptoDevs
   ```

### Testing Whitelisted Mint

1. **Minting the NFT**  
   Open your NFT Contract on Sepolia Etherscan, go to the Contract tab, and connect your wallet. Call the `mint` function with a payable amount of 0 if you are on the whitelist. For non-whitelisted users, set the payable amount to 0.01 ETH.

## 🌈 Conclusion

Congratulations! You have built an NFT collection with a whitelist using Foundry and Solidity. This project demonstrates the integration of smart contracts and blockchain technology in a real-world application.

Feel free to explore the repository, and don't hesitate to reach out if you have any questions or want to collaborate! 🙌

---

### References
- [LearnWeb3 - Build an NFT Collection with a Whitelist Using Foundry and Solidity](https://learnweb3.io/courses/sophomore/build-an-nft-collection-with-a-whitelist-using-foundry-and-solidity)
