# Polymarket CLI Skill

An AI agent skill for the [Polymarket CLI](https://github.com/Polymarket/polymarket-cli) — enabling AI coding agents to browse prediction markets, place trades, manage wallets, and interact with on-chain contracts from the terminal.

## What is this?

This is a **skill package** for AI coding agents (Claude Code, Cursor, Codex, Gemini CLI, etc.). It teaches agents how to use the Polymarket CLI effectively — including installation, command syntax, flags, workflows, and troubleshooting.

When installed, your AI agent will automatically know how to:

- **Browse markets** — Search, list, and view prediction market details
- **Check prices** — View order books, spreads, midpoints, and price history
- **Place trades** — Create limit orders, market orders, and manage open orders
- **Manage wallets** — Create, import, and configure wallets
- **On-chain operations** — Split/merge/redeem conditional tokens, approve contracts, bridge funds
- **Query data** — Positions, trade history, leaderboards, open interest

## Install

### Via [skills.sh](https://skills.sh) (Recommended)

```bash
npx skills add behfar-dev/Polymarket-cli-skill
```

This installs the skill for all supported AI agents in your project.

### Manual (Claude Code only)

Copy the `SKILL.md` and `references/` folder into your project's `.claude/skills/` directory.

## Skill Structure

```
SKILL.md                          # Main skill guide (overview, install, config, workflows)
references/
  commands-browsing.md            # Read-only market browsing commands
  commands-trading.md             # CLOB trading & order management
  commands-onchain.md             # Smart contract & CTF operations
  commands-wallet.md              # Wallet management & setup
```

## Prerequisites

The Polymarket CLI must be installed on your machine. Three options:

**Homebrew (macOS/Linux):**
```bash
brew tap Polymarket/polymarket-cli https://github.com/Polymarket/polymarket-cli
brew install polymarket
```

**Shell script:**
```bash
curl -sSL https://raw.githubusercontent.com/Polymarket/polymarket-cli/main/install.sh | sh
```

**Build from source (Rust 1.88+):**
```bash
git clone https://github.com/Polymarket/polymarket-cli
cd polymarket-cli
cargo install --path .
```

## Example Usage

Once the skill is installed, just ask your AI agent naturally:

- *"Search for Bitcoin prediction markets on Polymarket"*
- *"Show me the order book for this market"*
- *"What are the most active markets right now?"*
- *"Place a limit order on this market"*

The agent will use the Polymarket CLI with the correct commands and flags.

## Quick Command Reference

| Command | Description |
|---------|-------------|
| `polymarket status` | Check API health |
| `polymarket markets list --active true` | List active markets |
| `polymarket markets search "query"` | Search markets by keyword |
| `polymarket markets get <id>` | Get market details |
| `polymarket events get <slug>` | Get event with all sub-markets |
| `polymarket clob book <token_id>` | View order book |
| `polymarket clob price <token_id>` | Check current price |
| `polymarket -o json markets list` | JSON output for scripting |

> **Note:** Browsing commands work without a wallet. Trading commands require wallet setup via `polymarket setup`.

## Links

- [Polymarket CLI GitHub](https://github.com/Polymarket/polymarket-cli)
- [Polymarket](https://polymarket.com)
- [skills.sh](https://skills.sh)

## License

MIT
