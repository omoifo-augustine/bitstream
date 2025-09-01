# BitStream Protocol v1.0

[![Clarity](https://img.shields.io/badge/clarity-v3-blue.svg)](https://clarity-lang.org/)
[![Stacks](https://img.shields.io/badge/stacks-bitcoin--layer--2-orange.svg)](https://stacks.co/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Tests](https://img.shields.io/badge/tests-vitest-brightgreen.svg)](https://vitest.dev/)

> Next-Generation Bitcoin Payment Channel Framework

BitStream Protocol revolutionizes cross-chain payment infrastructure by creating seamless bidirectional channels between Bitcoin's robust monetary network and Stacks' sophisticated smart contract capabilities. This groundbreaking system enables instant, low-cost transactions while maintaining the security guarantees and decentralization principles that make Bitcoin the world's premier digital asset.

## 🚀 Core Features

### 🔒 Security-First Architecture

- **Bitcoin-Synchronized Timelock Enforcement**: Dispute resolution anchored to Bitcoin block confirmations for maximum security and predictability
- **ECDSA secp256k1 Cryptographic Compatibility**: Native Bitcoin signature verification ensuring seamless cross-protocol interoperability
- **Multi-Party Atomic Settlement**: Coordinated channel closures that prevent double-spending and ensure economic finality across network boundaries
- **Incentive-Aligned Security Framework**: Economic penalties that make malicious behavior prohibitively expensive while rewarding honest participation

### ⚡ Lightning Network Compatibility

- **Bidirectional Payment Channels**: Instant, low-cost transactions between participants
- **Cooperative & Unilateral Closures**: Flexible settlement mechanisms for optimal user experience
- **State Channel Management**: UTXO-inspired transparency with Bitcoin-grade security guarantees
- **Cross-Chain Interoperability**: Seamless integration with existing Bitcoin payment infrastructure

### 💰 Economic Safeguards

- **Overflow Protection**: Comprehensive integer overflow detection and prevention
- **Economic Viability Checks**: Automatic rejection of dust transactions
- **Balance Conservation Laws**: Mathematical guarantees for fund integrity
- **Emergency Recovery Protocols**: Administrative safeguards for critical security situations

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v18 or higher)
- [Clarinet](https://github.com/hirosystems/clarinet) (v2.0 or higher)
- [Git](https://git-scm.com/)

## 🛠️ Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/omoifo-augustine/bitstream.git
   cd bitstream
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Verify installation**

   ```bash
   clarinet check
   ```

## 🏃‍♂️ Quick Start

### Development Environment Setup

1. **Start Clarinet console**

   ```bash
   clarinet console
   ```

2. **Deploy contract locally**

   ```clarity
   ::deploy_contract contracts/bitstream.clar
   ```

3. **Run tests**

   ```bash
   npm test
   ```

### Basic Channel Operations

#### Creating a Payment Channel

```clarity
;; Create a new payment channel with 100,000 microSTX
(contract-call? .bitstream create-payment-channel 
  0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
  'SP1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
  u100000)
```

#### Adding Liquidity

```clarity
;; Add additional funds to existing channel
(contract-call? .bitstream add-channel-liquidity 
  0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
  'SP1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
  u50000)
```

#### Cooperative Settlement

```clarity
;; Settle channel with mutual agreement
(contract-call? .bitstream settle-channel-cooperatively
  0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef
  'SP1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
  u75000  ;; Creator final balance
  u75000  ;; Participant final balance
  0x...   ;; Creator signature
  0x...)  ;; Participant signature
```

## 🧪 Testing

### Running the Test Suite

```bash
# Run all tests
npm test

# Run tests with coverage report
npm run test:report

# Watch mode for development
npm run test:watch
```

### Contract Validation

```bash
# Check contract syntax and semantics
clarinet check

# Run contract analysis
clarinet analyze
```

## 📚 Contract API Reference

### Public Functions

#### `create-payment-channel`

Creates a new bidirectional payment channel with atomic fund locking.

**Parameters:**

- `channel-id` (buff 32): Unique 256-bit channel identifier
- `recipient` (principal): Channel receiving party address
- `funding-amount` (uint): Initial channel capacity in microSTX

**Returns:** `(response bool uint)`

#### `add-channel-liquidity`

Increases existing channel capacity through additional fund commitment.

**Parameters:**

- `channel-id` (buff 32): Target channel identifier
- `recipient` (principal): Channel participant address
- `additional-funds` (uint): Amount to add in microSTX

**Returns:** `(response bool uint)`

#### `settle-channel-cooperatively`

Executes instant channel settlement when both parties provide valid signatures.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `recipient` (principal): Participant address
- `creator-final-balance` (uint): Creator's final balance
- `recipient-final-balance` (uint): Recipient's final balance
- `creator-authorization` (buff 65): Creator's signature
- `recipient-authorization` (buff 65): Recipient's signature

**Returns:** `(response bool uint)`

#### `initiate-unilateral-closure`

Initiates unilateral channel closure with Bitcoin-anchored dispute period.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `recipient` (principal): Participant address
- `claimed-creator-balance` (uint): Claimed creator balance
- `claimed-recipient-balance` (uint): Claimed recipient balance
- `state-authorization` (buff 65): State signature

**Returns:** `(response bool uint)`

#### `complete-disputed-settlement`

Completes disputed settlement after timelock expiration.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `recipient` (principal): Participant address

**Returns:** `(response bool uint)`

### Read-Only Functions

#### `get-channel-information`

Retrieves comprehensive channel state for monitoring and routing.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `creator` (principal): Channel creator address
- `participant` (principal): Channel participant address

**Returns:** `(optional {total-liquidity: uint, creator-funds: uint, participant-funds: uint, operational-status: bool, dispute-expiry: uint, version-counter: uint})`

### Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `MAX_CHANNEL_CAPACITY` | `u1000000000000` | Maximum channel capacity (1M STX) |
| `MIN_TRANSACTION_THRESHOLD` | `u0` | Minimum transaction amount |

### Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| u100 | `ERR_ACCESS_DENIED` | Unauthorized operation |
| u101 | `ERR_CHANNEL_EXISTS` | Duplicate channel creation |
| u102 | `ERR_CHANNEL_MISSING` | Channel lookup failed |
| u103 | `ERR_INSUFFICIENT_FUNDS` | Balance constraint violation |
| u104 | `ERR_SIGNATURE_INVALID` | Cryptographic verification failed |
| u105 | `ERR_CHANNEL_CLOSED` | Operations on inactive channel |
| u106 | `ERR_TIMELOCK_ACTIVE` | Settlement period not expired |
| u107 | `ERR_INVALID_INPUT` | Malformed request parameters |
| u108 | `ERR_BALANCE_OVERFLOW` | Economic limits exceeded |
| u109 | `ERR_INVALID_ADDRESS` | Principal validation failure |

## 🏗️ Architecture

### State Management

The protocol implements a comprehensive channel registry using Clarity's `define-map` functionality:

```clarity
(define-map bitstream-channels
  {
    channel-id: (buff 32),
    creator: principal,
    participant: principal,
  }
  {
    total-liquidity: uint,
    creator-funds: uint,
    participant-funds: uint,
    operational-status: bool,
    dispute-expiry: uint,
    version-counter: uint,
  }
)
```

### Security Validations

Multi-layer validation system ensuring protocol integrity:

- **Channel ID Validation**: 256-bit identifier verification
- **Economic Viability**: Dust transaction prevention
- **Signature Format**: Bitcoin-standard ECDSA validation
- **Safe Amount**: Integer overflow protection
- **Participant Verification**: Address validation and self-interaction prevention
- **Balance Equation**: Comprehensive balance integrity verification

### Dispute Resolution

Bitcoin-synchronized timelock enforcement with 144-block dispute windows (approximately 24 hours), providing security against counterparty unresponsiveness or malicious behavior.

## 🔐 Security Considerations

### Best Practices

1. **Always validate signatures** before executing state transitions
2. **Use proper nonce management** to prevent replay attacks
3. **Implement timeout mechanisms** for dispute resolution
4. **Monitor channel states** off-chain for optimal user experience
5. **Regular security audits** of contract interactions

### Known Limitations

- Maximum channel capacity: 1,000,000 STX
- Dispute resolution period: 144 blocks (non-configurable)
- Single-hop channels only (no routing)

## 🤝 Contributing

We welcome contributions from the community! Please read our [Contributing Guidelines](CONTRIBUTING.md) before submitting pull requests.

### Development Workflow

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes and add tests
4. Ensure all tests pass: `npm test`
5. Check contract validity: `clarinet check`
6. Commit your changes: `git commit -m 'Add amazing feature'`
7. Push to the branch: `git push origin feature/amazing-feature`
8. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Stacks Foundation](https://stacks.org/) for the robust Bitcoin Layer 2 infrastructure
- [Lightning Network](https://lightning.network/) for payment channel inspiration
- [Clarity Language](https://clarity-lang.org/) for secure smart contract development
- The Bitcoin community for establishing the foundational security principles
