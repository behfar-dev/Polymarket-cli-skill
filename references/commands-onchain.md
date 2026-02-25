# On-Chain Commands Reference

Commands for interacting with smart contracts on Polygon, querying on-chain data, and bridging assets.

## Table of Contents

1. [Contract Approvals](#contract-approvals)
2. [CTF Operations](#ctf-operations)
3. [On-Chain Data Queries](#on-chain-data-queries)
4. [Bridge](#bridge)

---

## Contract Approvals

Before trading on Polymarket, ERC-20 (USDC) and ERC-1155 (CTF token) approvals must be granted to the exchange contracts.

### `approve check`

Check current approval status. Read-only — no gas needed.

```bash
# Check your own wallet's approvals
polymarket approve check

# Check another wallet's approvals
polymarket approve check <ADDRESS>
```

Shows USDC allowance and CTF approval status for each contract: CTF Exchange, Neg Risk Exchange, and Neg Risk Adapter.

### `approve set`

Approve all required contracts for trading. Sends 6 on-chain transactions (2 per contract: one ERC-20 approve and one ERC-1155 setApprovalForAll).

```bash
polymarket approve set
```

**Requires MATIC for gas on Polygon.** The contracts approved are:
- CTF Exchange — USDC + CTF tokens
- Neg Risk Exchange — USDC + CTF tokens
- Neg Risk Adapter — USDC + CTF tokens

USDC address on Polygon: `0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174`

---

## CTF Operations

The Conditional Token Framework (CTF) manages ERC-1155 tokens representing market outcomes. All write operations require a wallet and MATIC for gas. Read-only ID calculations don't need gas.

### `ctf split`

Split USDC collateral into outcome tokens (e.g., YES and NO).

```bash
polymarket ctf split \
  --condition <CONDITION_ID> \
  --amount <USDC_AMOUNT> \
  [--collateral <ADDRESS>] \
  [--partition <CSV>] \
  [--parent-collection <HEX>]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--condition` | hex | required | Condition ID (0x-prefixed, 32 bytes) |
| `--amount` | decimal | required | USDC amount (e.g., 10 = $10) |
| `--collateral` | address | USDC | Collateral token address |
| `--partition` | CSV | "1,2" | Index sets for outcome slots |
| `--parent-collection` | hex | zero | Parent collection ID for nested positions |

Example: Split $10 into YES/NO tokens:
```bash
polymarket ctf split --condition 0xCONDITION... --amount 10
```

For markets with 3 outcomes, use `--partition "1,2,4"`.

### `ctf merge`

Merge outcome tokens back into USDC. Same flags as `split`.

```bash
polymarket ctf merge --condition 0xCONDITION... --amount 10
```

### `ctf redeem`

Redeem winning tokens after a market resolves.

```bash
polymarket ctf redeem \
  --condition <CONDITION_ID> \
  [--collateral <ADDRESS>] \
  [--index-sets <CSV>] \
  [--parent-collection <HEX>]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--condition` | hex | required | Condition ID |
| `--collateral` | address | USDC | Collateral token |
| `--index-sets` | CSV | "1,2" | Which outcome positions to redeem |
| `--parent-collection` | hex | zero | Parent collection for nested positions |

### `ctf redeem-neg-risk`

Redeem neg-risk positions.

```bash
polymarket ctf redeem-neg-risk \
  --condition <CONDITION_ID> \
  --amounts "10,5"
```

The `--amounts` flag takes comma-separated USDC amounts, one per outcome.

### Read-Only ID Calculations

These don't need gas or a wallet — they just compute IDs locally via RPC calls.

```bash
# Calculate a condition ID
polymarket ctf condition-id \
  --oracle <ADDRESS> \
  --question <HEX_32> \
  --outcomes <N>

# Calculate a collection ID
polymarket ctf collection-id \
  --condition <HEX_32> \
  --index-set <N> \
  [--parent-collection <HEX_32>]

# Calculate a position ID (ERC-1155 token ID)
polymarket ctf position-id \
  --collateral <ADDRESS> \
  --collection <HEX_32>
```

---

## On-Chain Data Queries

Public data about wallets, markets, and leaderboards. No wallet needed.

### Portfolio Queries

```bash
# Open positions
polymarket data positions <ADDRESS> [--limit 25] [--offset N]

# Closed positions
polymarket data closed-positions <ADDRESS> [--limit 25] [--offset N]

# Total position value
polymarket data value <ADDRESS>

# Number of unique markets traded
polymarket data traded <ADDRESS>

# Trade history
polymarket data trades <ADDRESS> [--limit 25] [--offset N]

# On-chain activity
polymarket data activity <ADDRESS> [--limit 25] [--offset N]
```

All address arguments are 0x-prefixed Ethereum addresses.

### Market Data

```bash
# Top token holders for a market
polymarket data holders <CONDITION_ID> [--limit 10]

# Open interest
polymarket data open-interest <CONDITION_ID>

# Live volume for an event
polymarket data volume <EVENT_ID>
```

### Leaderboards

```bash
# Trader leaderboard
polymarket data leaderboard \
  [--period <day|week|month|all>] \
  [--order-by <pnl|vol>] \
  [--limit 25] \
  [--offset N]

# Builder leaderboard
polymarket data builder-leaderboard \
  [--period <day|week|month|all>] \
  [--limit 25] \
  [--offset N]

# Builder volume time-series
polymarket data builder-volume \
  [--period <day|week|month|all>]
```

---

## Bridge

Deposit assets from other chains into Polymarket.

### `bridge deposit`

Get deposit addresses for multiple chains.

```bash
polymarket bridge deposit <WALLET_ADDRESS>
```

Returns deposit addresses for EVM, Solana, and Bitcoin.

### `bridge supported-assets`

List chains and tokens that can be bridged.

```bash
polymarket bridge supported-assets
```

### `bridge status`

Check the status of a deposit.

```bash
polymarket bridge status <DEPOSIT_ADDRESS>
```

The deposit address can be EVM, Solana, or Bitcoin format.
