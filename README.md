# Carbon Track 🌱

A blockchain-based supply chain transparency platform that enables companies to track, verify, and display the complete carbon footprint of their products from raw materials to final delivery through Digital Product Passports (DPP).

## 🚀 Features

### Core Functionality
- **📊 Carbon Footprint Tracking**: Track emissions across entire supply chains
- **🔗 Blockchain Verification**: Immutable records using ERC-1155 tokens on Avalanche
- **🗺️ Interactive Supply Chain Maps**: Visualize product journeys with OpenLayers
- **📋 Digital Product Passports**: Public, verifiable product information
- **🏭 Multi-Plant Management**: Manage multiple facilities and production lines
- **🤝 Partner Networks**: Bidirectional supplier/customer relationships

### Advanced Visualizations
- **🌳 Hierarchical Product Trees**: D3.js-powered component breakdown
- **📈 Carbon Analytics**: Comprehensive environmental impact metrics
- **🚚 Transport Method Tracking**: Shipping routes and methods visualization
- **📱 QR Code Generation**: Easy token verification and access

## 🛠️ Technology Stack

### Frontend
- **Next.js 15.5.3** with App Router
- **React 19.1.0** with TypeScript
- **Tailwind CSS 4** for styling
- **Radix UI + Shadcn/ui** for components

### Blockchain
- **Ethers.js 6.15.0** for Web3 interactions
- **Wagmi 2.16.9** for wallet connections
- **ERC-1155** smart contracts
- **Avalanche Fuji Testnet**

### Data & Visualization
- **MongoDB 6.19.0** for persistence
- **OpenLayers** for interactive maps
- **react-d3-tree** for hierarchical views
- **Recharts** for analytics

### Development
- **Hardhat** for smart contract development
- **Biome** for linting and formatting
- **pnpm** package manager

## 📋 Prerequisites

Before you begin, ensure you have:

- **Node.js 18+** installed
- **pnpm** package manager
- **MongoDB** running locally or connection string
- **MetaMask** wallet extension
- **Git** for version control

## ⚡ Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/carbon-track.git
cd carbon-track
```

### 2. Install Dependencies
```bash
pnpm install
```

### 3. Environment Setup
Copy the example environment file and configure your variables:
```bash
cp .env.example .env.local
```

### 4. Configure Your Environment
Edit `.env.local` with your settings:
```env
MONGODB_URI=mongodb://localhost:27017/carbon-footprint
NEXT_PUBLIC_CONTRACT_ADDRESS=0x6f70264A4f3608FFa8Ff3ED5C6a1c4542D79fb88
NEXT_PUBLIC_CHAIN_ID=43113
NEXT_PUBLIC_RPC_URL=https://api.avax-test.network/ext/bc/C/rpc
```

### 5. Start Development Server
```bash
pnpm dev
```

Visit [http://localhost:3000](http://localhost:3000) to see the application.

## 🔧 Development Scripts

```bash
# Development with Turbopack
pnpm dev

# Production build
pnpm build

# Start production server
pnpm start

# Code linting
pnpm lint

# Code formatting
pnpm format
```

## 🏗️ Smart Contract Development

### Setup Hardhat
```bash
cd hardhat
npm install
```

### Deploy Contracts
```bash
# Deploy to local network
npx hardhat run scripts/deploy.ts

# Deploy to Fuji testnet
npx hardhat run scripts/deploy.ts --network fuji
```

### Run Tests
```bash
npx hardhat test
```

## 🌐 Wallet Configuration

### MetaMask Setup for Avalanche Fuji
1. Open MetaMask
2. Add Avalanche Fuji Testnet:
   - **Network Name**: Avalanche Fuji
   - **RPC URL**: https://api.avax-test.network/ext/bc/C/rpc
   - **Chain ID**: 43113
   - **Currency Symbol**: AVAX
   - **Block Explorer**: https://testnet.snowtrace.io/

### Get Test AVAX
Visit the [Avalanche Faucet](https://faucet.avax.network/) to get test AVAX for transactions.

## 📖 Usage Guide

### Getting Started
1. **Connect Wallet**: Click "Connect Wallet" and approve the connection
2. **Register Company**: Fill out company details and blockchain verification
3. **Add Plants**: Register your production facilities with GPS coordinates
4. **Create Products**: Define product templates with carbon footprint data
5. **Start Production**: Create batches that mint blockchain tokens
6. **Track Supply Chain**: Visualize complete product journeys

### Key Workflows
- **Product Creation**: Define templates → Create batches → Mint tokens
- **Component Tracking**: Use existing tokens as components in new products
- **Partner Management**: Add suppliers/customers for token transfers
- **Carbon Accounting**: Track emissions per unit and total production

## 🏢 Business Model

Carbon Track supports enterprise deployment with:
- **Company-based accounts**: Each wallet represents a company
- **Subscription tiers**: Feature access based on subscription level
- **Usage tracking**: Monitor token operations and API calls
- **White-label options**: Customize branding for enterprise clients

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details on:
- Code style and standards
- Pull request process
- Issue reporting
- Development workflow

## 📚 Documentation

- [API Documentation](docs/API.md)
- [Deployment Guide](docs/DEPLOYMENT.md)
- [Smart Contract Documentation](hardhat/README.md)

## 🛡️ Security

- All smart contracts use OpenZeppelin standards
- Gas optimization for Avalanche network
- Comprehensive error handling
- Input validation and sanitization

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙋‍♂️ Support

If you need help or have questions:
- Create an issue on GitHub
- Check existing documentation
- Review the API documentation

---

**Built with ❤️ for supply chain transparency and environmental accountability**