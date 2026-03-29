# Smart Contract Development Kit

A professional toolkit for building, testing, and deploying Ethereum smart contracts. Includes five production-quality Solidity contracts, comprehensive test suites, deployment scripts, and a web-based frontend for interaction.

---

## Overview

This project provides a complete end-to-end workflow for smart contract development:

- **5 Smart Contracts** written in Solidity 0.8.x with thorough documentation
- **5 Test Suites** using Mocha/Chai with 60+ test cases
- **Deployment Scripts** for local and Sepolia testnet
- **Interaction Scripts** for programmatic contract calls
- **Web Frontend** with wallet connection and contract UI
- **Comprehensive Documentation** covering blockchain fundamentals through deployment

### Contracts Included

| Contract | Type | Description |
|:---------|:-----|:------------|
| **SimpleStorage** | Storage | Store and retrieve unsigned integers with event tracking |
| **BasicToken** | ERC-20 | Fungible token with mint, burn, transfer, approve, and pause |
| **Escrow** | DeFi | Two-party escrow with deposit, release, refund, and dispute resolution |
| **Voting** | Governance | Proposal creation, weighted voting, delegation, and result tallying |
| **NFTMarketplace** | ERC-721 + Market | NFT minting, listing, buying, royalties, and withdrawal |

---

## Prerequisites

| Requirement | Version | Installation |
|:------------|:--------|:-------------|
| **Node.js** | >= 18.0.0 | [nodejs.org](https://nodejs.org/) |
| **npm** | >= 9.0.0 | Included with Node.js |
| **MetaMask** | Latest | [metamask.io](https://metamask.io/) |
| **Git** | Any recent | [git-scm.com](https://git-scm.com/) |

For testnet deployment you will also need:
- An [Alchemy](https://www.alchemy.com/) account (free tier)
- Sepolia test ETH from a [faucet](https://sepoliafaucet.com/)

---

## Project Structure

```
smart-contract-devkit/
|
|-- contracts/                  # Solidity smart contracts
|   |-- SimpleStorage.sol       # Basic state variable storage
|   |-- BasicToken.sol          # ERC-20 fungible token
|   |-- Escrow.sol              # Two-party escrow with disputes
|   |-- Voting.sol              # Governance voting system
|   |-- NFTMarketplace.sol      # ERC-721 NFT marketplace
|
|-- test/                       # Automated test suites
|   |-- SimpleStorage.test.js
|   |-- BasicToken.test.js
|   |-- Escrow.test.js
|   |-- Voting.test.js
|   |-- NFTMarketplace.test.js
|
|-- scripts/                    # Deployment and interaction scripts
|   |-- deploy.js               # Deploy all contracts
|   |-- interact.js             # Interact with deployed contracts
|
|-- frontend/                   # Web-based user interface
|   |-- index.html              # Main HTML page
|   |-- app.js                  # ethers.js contract interaction logic
|   |-- style.css               # Dark-themed responsive UI
|
|-- docs/                       # Documentation
|   |-- BLOCKCHAIN_PRIMER.md    # Blockchain and Ethereum fundamentals
|   |-- SOLIDITY_GUIDE.md       # Solidity language reference
|   |-- DEPLOYMENT_GUIDE.md     # Step-by-step deployment walkthrough
|   |-- METAMASK_SETUP.md       # MetaMask installation and configuration
|   |-- CONTRACT_REFERENCE.md   # Function-by-function contract docs
|   |-- screenshots/            # Screenshot documentation
|
|-- hardhat.config.js           # Hardhat configuration
|-- package.json                # Node.js dependencies and scripts
|-- .env.example                # Environment variable template
|-- .gitignore                  # Git ignore rules
|-- LICENSE                     # MIT License
|-- README.md                   # This file
```

---

## Quick Start

### 1. Install Dependencies

```bash
cd smart-contract-devkit
npm install
```

### 2. Set Up Environment

```bash
cp .env.example .env
# Edit .env with your private key and API keys
```

### 3. Compile Contracts

```bash
npx hardhat compile
```

### 4. Run Tests

```bash
npx hardhat test
```

### 5. Start Local Blockchain

```bash
npx hardhat node
```

### 6. Deploy Contracts (in a new terminal)

```bash
npx hardhat run scripts/deploy.js --network localhost
```

### 7. Interact with Contracts

```bash
npx hardhat run scripts/interact.js --network localhost
```

### 8. Launch Frontend

```bash
# Using any static file server:
npx serve frontend
# Then open http://localhost:3000
```

---

## Available Scripts

| Command | Description |
|:--------|:------------|
| `npm run compile` | Compile all Solidity contracts |
| `npm test` | Run all test suites |
| `npm run test:gas` | Run tests with gas usage reporting |
| `npm run test:coverage` | Generate code coverage report |
| `npm run node` | Start local Hardhat blockchain node |
| `npm run deploy:local` | Deploy contracts to local node |
| `npm run deploy:sepolia` | Deploy contracts to Sepolia testnet |
| `npm run interact:local` | Run interaction script on local node |
| `npm run interact:sepolia` | Run interaction script on Sepolia |
| `npm run clean` | Remove compiled artifacts and cache |

---

## Testing

The test suite covers:

- Contract deployment and initialization
- State transitions and access control
- ERC-20 and ERC-721 standard compliance
- Escrow lifecycle (create, fund, release, refund, dispute)
- Voting mechanics (register, vote, delegate, finalize)
- Marketplace operations (mint, list, buy, cancel, withdraw)
- Edge cases, error conditions, and security scenarios

Run all tests:

```bash
npx hardhat test
```

Run a specific test file:

```bash
npx hardhat test test/Escrow.test.js
```

---

## Deployment

### Local Development

1. Start the Hardhat node: `npx hardhat node`
2. Deploy: `npx hardhat run scripts/deploy.js --network localhost`
3. Note the contract addresses from the output

### Sepolia Testnet

1. Configure `.env` with your `PRIVATE_KEY` and `ALCHEMY_API_KEY`
2. Ensure you have Sepolia test ETH
3. Deploy: `npx hardhat run scripts/deploy.js --network sepolia`
4. Verify on Etherscan (see `docs/DEPLOYMENT_GUIDE.md`)

---

## Security Features

All contracts implement industry-standard security patterns:

- **Reentrancy Guard**: Mutex lock on all ETH-transferring functions
- **Checks-Effects-Interactions**: State changes before external calls
- **Access Control**: Owner-only and role-based function restrictions
- **Custom Errors**: Gas-efficient error handling (Solidity 0.8.x)
- **Overflow Protection**: Built-in arithmetic overflow/underflow checks
- **Pull Payment**: Recipients withdraw funds instead of receiving pushes
- **Input Validation**: Zero-address checks, amount validation, state machine enforcement
- **Pausable**: Emergency stop mechanism on token transfers

---

## Documentation

| Document | Content |
|:---------|:--------|
| [Blockchain Primer](docs/BLOCKCHAIN_PRIMER.md) | Blockchain fundamentals, Ethereum, gas, accounts |
| [Solidity Guide](docs/SOLIDITY_GUIDE.md) | Language reference, types, patterns, optimization |
| [Deployment Guide](docs/DEPLOYMENT_GUIDE.md) | Step-by-step compile, test, deploy walkthrough |
| [MetaMask Setup](docs/METAMASK_SETUP.md) | Wallet installation, network config, faucets |
| [Contract Reference](docs/CONTRACT_REFERENCE.md) | Every function documented per contract |

---

## Technology Stack

| Layer | Technology | Version |
|:------|:-----------|:--------|
| Smart Contracts | Solidity | 0.8.19 |
| Development Framework | Hardhat | 2.19.x |
| Testing | Mocha + Chai | 4.x |
| Ethereum Library | ethers.js | 6.x |
| Frontend | Vanilla JavaScript + HTML/CSS | ES6+ |
| Wallet | MetaMask | Latest |
| Testnet | Sepolia | -- |
| RPC Provider | Alchemy | -- |

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
