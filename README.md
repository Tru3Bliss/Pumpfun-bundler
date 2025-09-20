# Pump.fun Bundler

A sophisticated Solana token bundler designed to create and launch tokens on pump.fun with advanced features including multi-wallet bundling, vanity address generation, and Jito MEV protection.

## 🚀 Features

- **Multi-Wallet Bundling**: Distributes SOL across multiple wallets and executes coordinated token purchases
- **Vanity Address Generation**: Generate custom token addresses with specific suffixes (e.g., ending with "pump")
- **Jito MEV Protection**: Uses Jito bundles for MEV protection and transaction ordering
- **Address Lookup Tables (LUT)**: Optimizes transaction size and reduces fees
- **Flexible Configuration**: Support for both multi-wallet and single-wallet bundling modes
- **Automatic Retry Logic**: Built-in retry mechanisms for RPC and Jito failures
- **Token Metadata**: Full support for token metadata including images, descriptions, and social links

## 📋 Prerequisites

- Node.js (v16 or higher)
- npm or yarn
- Solana CLI (optional, for advanced users)
- Sufficient SOL balance for:
  - Token creation fees
  - Distribution to bundler wallets
  - Jito tips
  - Transaction fees

## 🛠️ Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd pump.fun-bundler
```

2. Install dependencies:
```bash
npm install
# or
yarn install
```

3. Create a `.env` file in the root directory with the following variables:

```env
# Required Configuration
PRIVATE_KEY=your_main_wallet_private_key_in_base58
RPC_ENDPOINT=https://your-solana-rpc-endpoint
RPC_WEBSOCKET_ENDPOINT=wss://your-solana-websocket-endpoint

# Token Configuration
TOKEN_NAME=Your Token Name
TOKEN_SYMBOL=SYMBOL
TOKEN_SHOW_NAME=Display Name
DESCRIPTION=Your token description
TOKEN_CREATE_ON=Launch Date
TWITTER=https://twitter.com/yourhandle
TELEGRAM=https://t.me/yourchannel
WEBSITE=https://yourwebsite.com
FILE=./image/your_token_image.jpg

# Bundling Configuration
SWAP_AMOUNT=0.1
DISTRIBUTION_WALLETNUM=10
JITO_FEE=0.001
VANITY_MODE=false

# Single Wallet Mode (for oneWalletBundle.ts)
BUYER_WALLET=buyer_wallet_private_key_in_base58
BUYER_AMOUNT=0.5
```

## 🎯 Usage

### Multi-Wallet Bundling (Recommended)

Run the main bundler script:
```bash
npm start
```

This will:
1. Generate or use a vanity address (if enabled)
2. Create the token with metadata
3. Distribute SOL to multiple wallets
4. Create and populate an Address Lookup Table
5. Execute coordinated buy transactions via Jito bundles

### Single Wallet Mode

For simpler operations with a single buyer wallet:
```bash
npm run single
```

### Additional Scripts

- **Close LUT**: `npm run close` - Closes the Address Lookup Table
- **Gather Funds**: `npm run gather` - Collects funds from bundler wallets
- **Check Status**: `npm run status` - Checks the status of transactions

## ⚙️ Configuration Options

### Token Settings
- `TOKEN_NAME`: The official name of your token
- `TOKEN_SYMBOL`: Token symbol (usually 3-5 characters)
- `TOKEN_SHOW_NAME`: Display name shown in wallets
- `DESCRIPTION`: Token description
- `FILE`: Path to token image (supports JPG, PNG)
- Social links: Twitter, Telegram, Website

### Bundling Settings
- `SWAP_AMOUNT`: SOL amount per wallet for purchasing (in SOL)
- `DISTRIBUTION_WALLETNUM`: Number of wallets to create and use
- `JITO_FEE`: Jito tip amount (in SOL)
- `VANITY_MODE`: Enable/disable vanity address generation

### RPC Configuration
- Use high-performance RPC endpoints for better success rates
- Recommended providers: Helius, QuickNode, Alchemy
- Ensure WebSocket support for real-time updates

## 🔧 Technical Details

### Architecture
- **Token Creation**: Uses pump.fun SDK for token deployment
- **Wallet Distribution**: Creates multiple keypairs and distributes SOL
- **LUT Management**: Optimizes transaction size using Address Lookup Tables
- **Jito Integration**: Sends bundles to multiple Jito endpoints for redundancy
- **Retry Logic**: Automatic retries for failed operations

### Transaction Flow
1. Token creation transaction
2. SOL distribution to bundler wallets
3. LUT creation and population
4. Coordinated buy transactions in bundles
5. Jito bundle submission

## 🚨 Troubleshooting

### Common Issues

#### RPC Errors
```
Error: RPC endpoint failed
```
**Solutions:**
- Use a premium RPC provider with higher rate limits
- Implement RPC endpoint rotation
- Check network connectivity
- Reduce concurrent requests

#### Jito Failures
```
Error: Jito bundle submission failed
```
**Solutions:**
- Increase Jito tip amount
- Try different Jito endpoints
- Reduce bundle size
- Check transaction simulation results

#### Insufficient Balance
```
Error: Main wallet balance is not enough
```
**Solutions:**
- Calculate required SOL: `(SWAP_AMOUNT + 0.01) * DISTRIBUTION_WALLETNUM + 0.04`
- Add more SOL to your main wallet
- Reduce `DISTRIBUTION_WALLETNUM` or `SWAP_AMOUNT`

#### Transaction Simulation Failures
**Solutions:**
- Reduce compute unit limits
- Optimize transaction size
- Check token metadata validity
- Verify wallet permissions

### Performance Optimization

1. **RPC Optimization**:
   - Use dedicated RPC endpoints
   - Implement connection pooling
   - Monitor rate limits

2. **Transaction Optimization**:
   - Use Address Lookup Tables effectively
   - Optimize compute unit allocation
   - Batch operations when possible

3. **Jito Optimization**:
   - Use appropriate tip amounts
   - Submit to multiple endpoints
   - Monitor bundle success rates

## 📁 Project Structure

```
├── constants/          # Configuration constants
├── executor/          # Transaction execution logic
│   ├── jito.ts       # Jito bundle execution
│   └── legacy.ts     # Legacy transaction execution
├── keys/             # Generated wallet keys and data
├── src/              # Core functionality
│   ├── main.ts       # Main bundling logic
│   ├── metadata.ts   # Token metadata handling
│   ├── uploadToIpfs.ts # IPFS upload functionality
│   └── vanity.ts     # Vanity address generation
├── utils/            # Utility functions
├── image/            # Token images
├── index.ts          # Main entry point
├── oneWalletBundle.ts # Single wallet mode
└── package.json      # Dependencies and scripts
```

## 🔐 Security Considerations

- **Private Keys**: Store private keys securely and never commit them to version control
- **RPC Endpoints**: Use trusted RPC providers to prevent data interception
- **Environment Variables**: Use `.env` files and ensure they're in `.gitignore`
- **Wallet Management**: Consider using hardware wallets for main operations

## 📊 Monitoring and Analytics

The bundler provides detailed logging for:
- Transaction signatures and confirmations
- Wallet creation and distribution
- LUT creation and population
- Jito bundle submissions
- Error tracking and retry attempts

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## ⚠️ Disclaimer

This software is for educational and research purposes. Users are responsible for:
- Compliance with local regulations
- Proper tax reporting
- Understanding the risks of cryptocurrency trading
- Securing their private keys and funds

## 📄 License

ISC License - see LICENSE file for details

## 🆘 Support

For issues and questions:
1. Check the troubleshooting section
2. Review the logs for specific error messages
3. Ensure all environment variables are properly set
4. Verify sufficient SOL balance and RPC connectivity

---

**Note**: This bundler is designed for pump.fun token launches. Always test with small amounts first and understand the risks involved in cryptocurrency operations.
