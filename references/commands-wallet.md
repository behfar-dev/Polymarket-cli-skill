# Wallet & Utility Commands Reference

## Table of Contents

1. [Wallet Management](#wallet-management)
2. [Setup Wizard](#setup-wizard)
3. [Interactive Shell](#interactive-shell)
4. [Upgrade](#upgrade)
5. [Global Flags](#global-flags)

---

## Wallet Management

### `wallet create`

Generate a new random wallet and save to config.

```bash
polymarket wallet create [--force] [--signature-type <TYPE>]
```

| Flag | Default | Description |
|------|---------|-------------|
| `--force` | false | Overwrite existing wallet config |
| `--signature-type` | proxy | Signature type: `eoa`, `proxy`, or `gnosis-safe` |

Creates a random private key, derives the wallet address and proxy wallet address, and saves to `~/.config/polymarket/config.json`. The config directory is created with 0700 permissions and the file with 0600 permissions on Unix.

If a wallet already exists and `--force` is not passed, the command will error with a message pointing to the existing config file.

### `wallet import`

Import an existing private key.

```bash
polymarket wallet import <KEY> [--force] [--signature-type <TYPE>]
```

The key can be with or without the `0x` prefix — the CLI normalizes it.

### `wallet address`

Print just the wallet address.

```bash
polymarket wallet address
```

Respects the `--private-key` flag if provided, otherwise uses the configured key.

### `wallet show`

Show full wallet info: address, proxy address, signature type, config path, and key source.

```bash
polymarket wallet show
```

Output includes where the key is coming from (flag, env var, or config file).

### `wallet reset`

Delete all configuration and keys.

```bash
polymarket wallet reset [--force]
```

Without `--force`, prompts for confirmation. Deletes the entire `~/.config/polymarket/` directory.

---

## Setup Wizard

### `setup`

Guided first-time setup experience. Walks through 4 steps:

1. **Wallet** — Create a new wallet or import an existing key
2. **Proxy Wallet** — Derives the proxy wallet address
3. **Fund Wallet** — Shows instructions for depositing USDC
4. **Approve Contracts** — Reminds to run `polymarket approve set`

```bash
polymarket setup
```

If a wallet already exists, offers to reconfigure or skip ahead. This command runs interactively and is not suitable for scripting — use the individual `wallet` commands instead.

---

## Interactive Shell

### `shell`

Launch a REPL where you can run commands without the `polymarket` prefix.

```bash
polymarket shell
```

Inside the shell:
```
polymarket> markets list --limit 3
polymarket> clob book 48331043336612883...
polymarket> exit
```

Features:
- Command history (arrow keys)
- All commands work the same as the CLI
- `exit` or `quit` to leave, or Ctrl+D
- `setup` is not available inside the shell (must run outside)
- Can't nest shells (`shell` inside the shell prints a message)

---

## Upgrade

### `upgrade`

Update the CLI to the latest version.

```bash
polymarket upgrade
```

---

## Global Flags

These flags apply to every command and must appear before the subcommand:

```bash
polymarket [GLOBAL FLAGS] <COMMAND> [COMMAND FLAGS]
```

| Flag | Short | Type | Default | Description |
|------|-------|------|---------|-------------|
| `--output` | `-o` | table/json | table | Output format |
| `--private-key` | — | string | — | Override private key for this command |
| `--signature-type` | — | string | — | Override signature type for this command |
| `--version` | — | flag | — | Print version and exit |
| `--help` | `-h` | flag | — | Print help |

### Private Key Resolution Order

1. `--private-key` flag
2. `POLYMARKET_PRIVATE_KEY` environment variable
3. Config file at `~/.config/polymarket/config.json`

### Signature Type Resolution Order

1. `--signature-type` flag
2. `POLYMARKET_SIGNATURE_TYPE` environment variable
3. Config file signature_type field
4. Default: `proxy`
