# Trading Commands Reference (CLOB)

All commands in this file interact with the CLOB (Central Limit Order Book). Read-only commands don't require a wallet. Trading, balance, reward, and account commands require authentication.

## Table of Contents

1. [Read-Only CLOB Commands](#read-only-clob-commands)
2. [Order Placement](#order-placement)
3. [Order Management](#order-management)
4. [Balance and Trades](#balance-and-trades)
5. [Rewards](#rewards)
6. [API Key Management](#api-key-management)
7. [Account](#account)

---

## Read-Only CLOB Commands

No wallet needed for any of these.

### `clob ok`

Health check for the CLOB API.

```bash
polymarket clob ok
```

### Price Queries

```bash
# Single token price
polymarket clob price <TOKEN_ID> --side <buy|sell>

# Batch prices (comma-separated token IDs)
polymarket clob batch-prices "TOKEN1,TOKEN2" --side buy

# Midpoint (average of best bid and ask)
polymarket clob midpoint <TOKEN_ID>

# Batch midpoints
polymarket clob midpoints "TOKEN1,TOKEN2"

# Bid-ask spread
polymarket clob spread <TOKEN_ID> [--side <buy|sell>]

# Batch spreads
polymarket clob spreads "TOKEN1,TOKEN2"
```

### Order Book

```bash
# Single token order book
polymarket clob book <TOKEN_ID>

# Multiple order books
polymarket clob books "TOKEN1,TOKEN2"
```

### Last Trade

```bash
polymarket clob last-trade <TOKEN_ID>
polymarket clob last-trades "TOKEN1,TOKEN2"
```

### Market Info

```bash
# Get market by condition ID (0x-prefixed hex)
polymarket clob market <CONDITION_ID>

# List all CLOB markets (paginated)
polymarket clob markets [--cursor <CURSOR>]

# Sampling markets (reward-eligible)
polymarket clob sampling-markets [--cursor <CURSOR>]

# Simplified markets (reduced detail)
polymarket clob simplified-markets [--cursor <CURSOR>]

# Simplified sampling markets
polymarket clob sampling-simp-markets [--cursor <CURSOR>]
```

### Price History

```bash
polymarket clob price-history <TOKEN_ID> --interval <INTERVAL> [--fidelity <N>]
```

| Interval | Description |
|----------|-------------|
| `1m` | 1 minute |
| `1h` | 1 hour |
| `6h` | 6 hours |
| `1d` | 1 day |
| `1w` | 1 week |
| `max` | All available data |

The `--fidelity` flag controls the number of data points returned.

### Metadata

```bash
# Tick size (minimum price increment)
polymarket clob tick-size <TOKEN_ID>

# Fee rate in basis points
polymarket clob fee-rate <TOKEN_ID>

# Check if token is neg-risk
polymarket clob neg-risk <TOKEN_ID>

# Server time
polymarket clob time

# Geoblock check
polymarket clob geoblock
```

---

## Order Placement

All commands in this section require a configured wallet.

### `clob create-order` (Limit Order)

```bash
polymarket clob create-order \
  --token <TOKEN_ID> \
  --side <buy|sell> \
  --price <DECIMAL> \
  --size <DECIMAL> \
  [--order-type <GTC|FOK|GTD|FAK>] \
  [--post-only]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--token` | string | required | Token ID (numeric string) |
| `--side` | buy/sell | required | Order side |
| `--price` | decimal | required | Price per share (0-1, e.g., 0.50) |
| `--size` | decimal | required | Number of shares |
| `--order-type` | enum | GTC | Order type: GTC, FOK, GTD, FAK |
| `--post-only` | flag | false | Only add to book, don't match |

Example: Buy 10 shares at 50 cents:
```bash
polymarket clob create-order --token 48331043336612883... --side buy --price 0.50 --size 10
```

### `clob market-order`

```bash
polymarket clob market-order \
  --token <TOKEN_ID> \
  --side <buy|sell> \
  --amount <DECIMAL> \
  [--order-type <FOK|FAK>]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--token` | string | required | Token ID |
| `--side` | buy/sell | required | Order side |
| `--amount` | decimal | required | USDC for buys, shares for sells |
| `--order-type` | enum | FOK | FOK or FAK |

The `--amount` meaning depends on the side: for buys it's USDC to spend, for sells it's shares to sell.

### `clob post-orders` (Batch)

```bash
polymarket clob post-orders \
  --tokens "TOKEN1,TOKEN2" \
  --side <buy|sell> \
  --prices "0.40,0.60" \
  --sizes "10,10" \
  [--order-type <GTC|FOK|GTD|FAK>]
```

All comma-separated lists must have the same number of entries. Side and order type apply to all orders in the batch.

---

## Order Management

### Cancel Orders

```bash
# Cancel a single order
polymarket clob cancel <ORDER_ID>

# Cancel multiple orders
polymarket clob cancel-orders "ORDER1,ORDER2"

# Cancel all open orders
polymarket clob cancel-all

# Cancel all orders for a specific market
polymarket clob cancel-market [--market <CONDITION_ID>] [--asset <TOKEN_ID>]
```

### View Orders

```bash
# List open orders
polymarket clob orders [--market <CONDITION_ID>] [--asset <TOKEN_ID>] [--cursor <CURSOR>]

# Get a single order
polymarket clob order <ORDER_ID>
```

---

## Balance and Trades

### `clob balance`

```bash
polymarket clob balance --asset-type <collateral|conditional> [--token <TOKEN_ID>]
```

For `collateral` (USDC balance), no token is needed. For `conditional` (outcome token balance), provide the token ID.

### `clob update-balance`

Refresh on-chain balance allowance.

```bash
polymarket clob update-balance --asset-type <collateral|conditional> [--token <TOKEN_ID>]
```

### `clob trades`

List your trade history.

```bash
polymarket clob trades [--market <CONDITION_ID>] [--asset <TOKEN_ID>] [--cursor <CURSOR>]
```

---

## Rewards

All reward commands require authentication.

```bash
# Earnings for a specific date
polymarket clob rewards --date 2024-06-15 [--cursor <CURSOR>]

# Total earnings for a date
polymarket clob earnings --date 2024-06-15

# Earnings with market config
polymarket clob earnings-markets --date 2024-06-15 [--cursor <CURSOR>]

# Current reward percentages
polymarket clob reward-percentages

# Active reward programs
polymarket clob current-rewards [--cursor <CURSOR>]

# Market-specific reward details
polymarket clob market-reward <CONDITION_ID> [--cursor <CURSOR>]

# Check if orders are scoring rewards
polymarket clob order-scoring <ORDER_ID>
polymarket clob orders-scoring "ORDER1,ORDER2"
```

Date format is always `YYYY-MM-DD`.

---

## API Key Management

```bash
# List API keys
polymarket clob api-keys

# Create or derive a new API key
polymarket clob create-api-key

# Delete current API key
polymarket clob delete-api-key
```

---

## Account

### `clob account-status`

Check if your account is in closed-only mode.

```bash
polymarket clob account-status
```

### `clob notifications`

```bash
# List notifications
polymarket clob notifications

# Delete notifications
polymarket clob delete-notifications "NOTIF1,NOTIF2"
```
