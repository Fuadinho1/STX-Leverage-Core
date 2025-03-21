

# STX-Leverage-Core Platform Core Contracts

An on-chain derivatives trading system built on the Stacks blockchain, offering core functionality for margin trading using STX tokens. This smart contract manages collateral deposits, leveraged positions (long/short), liquidation price calculations, and profit & loss (PnL) determination.

---

## 📄 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Contract Architecture](#contract-architecture)
- [Error Codes](#error-codes)
- [Setup and Deployment](#setup-and-deployment)
- [Contract Functions](#contract-functions)
  - [Read-Only Functions](#read-only-functions)
  - [Public Functions](#public-functions)
- [PnL and Liquidation Price Calculations](#pnl-and-liquidation-price-calculations)
- [Security Considerations](#security-considerations)
- [Potential Improvements](#potential-improvements)
- [License](#license)

---

## 🚀 Overview

The **STX-Derivatives Platform Core Contracts** are implemented in Clarity (Stacks smart contract language) and designed to provide the fundamental infrastructure for trading derivatives on-chain. It allows users to:

- Deposit and withdraw STX collateral.
- Open leveraged long and short positions.
- Calculate and enforce liquidation prices.
- Close positions, realizing profits or losses.
- Operate within a permissioned environment controlled by an admin (e.g., price updates).

---

## 🔧 Features

- **Leverage Trading**: Support for long and short positions with user-defined leverage.
- **Collateral Management**: Enforce a minimum collateral ratio (150%) to maintain platform solvency.
- **PnL Calculation**: Real-time profit & loss calculations based on a simplified price oracle.
- **Liquidation Price Calculation**: Automatic calculation of the price at which a position would be liquidated.
- **Admin Controls**: Contract owner can update prices (simulating an oracle) and manage contract ownership.
- **Error Handling**: Predefined error codes provide clarity on failed transactions.

---

## 🏗️ Contract Architecture

### Constants and Traits

- **Error Codes**: Standardized error handling.
- **Collateralization Ratios and Types**: Constants for position types and collateral thresholds.

### State and Storage
- **balances**: Tracks user STX balances.
- **positions**: Records user positions with their parameters.
- **position-counter**: Auto-incrementing ID for new positions.
- **contract-owner**: Controls administrative functions.
- **current-price**: Mock price oracle used for testing and settlement.

---

## ❗ Error Codes

| Code | Description               |
|------|---------------------------|
| 100  | Unauthorized access       |
| 101  | Invalid amount specified  |
| 102  | Insufficient balance      |
| 103  | Invalid position          |
| 104  | Insufficient collateral   |

---

## ⚙️ Setup and Deployment

### Prerequisites
- Stacks blockchain environment
- Clarity CLI (`clarity-cli`)
- Deployer wallet configured with STX tokens

### Steps

1. Clone the repository and navigate to your contracts directory:
   ```bash
   git clone https://github.com/yourusername/stx-derivatives-core.git
   cd stx-derivatives-core
   ```

2. Deploy the contract locally (example):
   ```bash
   clarity-cli launch
   ```

3. Deploy on the Stacks testnet or mainnet via [Stacks CLI](https://docs.stacks.co/docs/cli/overview/).

---

## 🛠️ Contract Functions

### Read-Only Functions

| Function Name                     | Description                                     |
|----------------------------------|-------------------------------------------------|
| `get-balance (user)`             | Returns the STX balance of a user.              |
| `get-position (position-id)`     | Retrieves data for a specific position.         |
| `get-current-price`              | Gets the current market price.                  |
| `calculate-liquidation-price`    | Computes the liquidation price for a position.  |

---

### Public Functions

#### 1. `deposit-collateral (amount)`
- Deposit STX tokens to be used as collateral.
- Updates the user's balance in the contract.

#### 2. `withdraw-collateral (amount)`
- Withdraws STX collateral if sufficient balance exists.
- Transfers STX back to the user.

#### 3. `open-position (position-type size leverage)`
- Opens a long/short leveraged position.
- Locks up the required collateral.
- Records liquidation price based on leverage.

#### 4. `close-position (position-id)`
- Closes an active position.
- Returns collateral plus PnL.
- Deletes the position from storage.

#### 5. `update-price (new-price)`
- Admin-only function to update the current market price.
- Would be replaced by an external oracle in production.

#### 6. `set-contract-owner (new-owner)`
- Admin-only function to transfer contract ownership.

---

## 📈 PnL and Liquidation Price Calculations

### Liquidation Price
- Long: `entry-price * (100 - (100 / leverage)) / 100`
- Short: `entry-price * (100 + (100 / leverage)) / 100`

### PnL Calculation (Simplified)
- Long: `(current-price - entry-price) * size`
- Short: `(entry-price - current-price) * size`

---

## 🔒 Security Considerations

- **Oracle Trust Assumption**: The `current-price` variable is set manually by the admin. In production, this must be replaced by a decentralized oracle.
- **Collateral Ratio Enforcement**: Minimum collateral ratio (`u150`) is set, but the code assumes the user is managing their collateral correctly beyond position opening.
- **Reentrancy**: Clarity contracts are not vulnerable to traditional reentrancy attacks but best practices are followed.
- **Access Control**: Critical functions (`update-price`, `set-contract-owner`) are admin-restricted.

---

## 🔮 Potential Improvements

- ✅ **Implement Oracle Integration** (Chainlink, Provable, or other decentralized sources).
- ✅ **Advanced PnL Models** with fees, funding rates, and liquidation penalties.
- ✅ **Partial Liquidations** for risky positions rather than full closures.
- ✅ **Margin Calls** and auto-top-up options.
- ✅ **Multi-Asset Support** for non-STX pairs.
- ✅ **Governance Mechanisms** for decentralized contract owner changes.
- ✅ **Event Emissions** for improved off-chain integration and UI feedback.

---
