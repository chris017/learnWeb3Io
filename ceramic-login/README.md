
# User-Owned Data Profiles Using Ceramic Network

In this project, I built a decentralized application (dApp) that allows users to create and manage their data profiles using the Ceramic Network.

## 📚 Overview

Ceramic is a decentralized data network that enables developers to create composable Web3 applications. By decentralizing application databases, developers can reuse data across applications and ensure interoperability. This project focuses on building sovereign user profiles that allow users to control their own data.

### Key Features
- **User Authentication**: Users can log in using their preferred wallet.
- **Profile Management**: Users can create and update their decentralized profiles.
- **Data Storage**: Profile information is stored securely on the Ceramic Network.

## 🚀 Getting Started

To run this dApp locally, follow these instructions:

### Prerequisites
Make sure you have the following installed:
- [Node.js](https://nodejs.org/) (v14 or higher)
- npm (comes with Node.js)

### Installation Steps
1. **Install Required Packages**
   ```bash
   npm install "@self.id/react" "@self.id/web" key-did-provider-ed25519
   npm install ethers@5 web3modal
   ```

### Running the dApp
1. **Start the Development Server**
   ```bash
   npm run dev
   ```

2. **Open Your Browser**
   Visit [http://localhost:3000](http://localhost:3000) to see your application in action.

## 💻 Code Structure

- **pages/_app.js**: Wraps the application with the Self.ID Provider for Ceramic integration.
- **pages/index.js**: Main application logic for connecting wallets and managing user profiles.
- **components/RecordSetter.js**: Component for setting and updating user profile information.

## 🌈 Conclusion

This project highlights the importance of user-owned data in Web3 and demonstrates how Ceramic enables decentralized applications with composable data. By allowing users to control their profiles, we can create a more user-centric internet.

Feel free to explore the repository, and don't hesitate to reach out if you have any questions or want to collaborate! 🙌

---

### References
- [LearnWeb3 - User-Owned Data Profiles using Ceramic Network](https://learnweb3.io/courses/junior/user-owned-data-profiles-using-ceramic-network)
- [Ceramic Network Documentation](https://developers.ceramic.network/learn/welcome/)
