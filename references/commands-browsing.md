# Browsing Commands Reference

All commands in this file are read-only and do not require a wallet.

## Table of Contents

1. [Markets](#markets)
2. [Events](#events)
3. [Tags](#tags)
4. [Series](#series)
5. [Comments](#comments)
6. [Profiles](#profiles)
7. [Sports](#sports)
8. [Status](#status)

---

## Markets

### `markets list`

List markets with optional filters.

```bash
polymarket markets list [OPTIONS]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--limit` | integer | 25 | Max results to return |
| `--offset` | integer | — | Pagination offset |
| `--order` | string | — | Sort field (e.g., `volume_num`, `liquidity_num`) |
| `--ascending` | flag | false | Sort ascending instead of descending |
| `--active` | bool | — | Filter by active status |
| `--closed` | bool | — | Filter by closed status |

Examples:
```bash
polymarket markets list --limit 10
polymarket markets list --active true --order volume_num
polymarket markets list --closed false --limit 50 --offset 25
```

### `markets get`

Get a single market by numeric ID or slug.

```bash
polymarket markets get <ID_OR_SLUG>
```

The CLI auto-detects whether the argument is a numeric ID or a slug. If all characters are digits, it queries by ID; otherwise by slug.

Examples:
```bash
polymarket markets get 12345
polymarket markets get will-trump-win-the-2024-election
```

### `markets search`

Search markets by query string.

```bash
polymarket markets search <QUERY> [OPTIONS]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--limit` | integer | 10 | Results per type |

Examples:
```bash
polymarket markets search "bitcoin" --limit 5
polymarket markets search "election"
```

### `markets tags`

Get tags associated with a specific market.

```bash
polymarket markets tags <MARKET_ID>
```

---

## Events

Events group related markets (e.g., "2024 Election" contains multiple yes/no markets).

### `events list`

```bash
polymarket events list [OPTIONS]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--limit` | integer | 25 | Max results |
| `--offset` | integer | — | Pagination offset |
| `--order` | string | — | Sort field |
| `--ascending` | flag | false | Sort ascending |
| `--active` | bool | — | Filter by active |
| `--closed` | bool | — | Filter by closed |
| `--tag` | string | — | Filter by tag name |

Examples:
```bash
polymarket events list --limit 10
polymarket events list --tag politics --active true
```

### `events get`

```bash
polymarket events get <EVENT_ID>
```

### `events tags`

Get tags for a specific event.

```bash
polymarket events tags <EVENT_ID>
```

---

## Tags

### `tags list`

List all available tags.

```bash
polymarket tags list
```

### `tags get`

Get details for a specific tag.

```bash
polymarket tags get <TAG_SLUG>
```

### `tags related`

Get related tags for a given tag.

```bash
polymarket tags related <TAG_SLUG>
```

### `tags related-tags`

Alternative endpoint for related tags.

```bash
polymarket tags related-tags <TAG_SLUG>
```

---

## Series

Series represent recurring events (e.g., weekly crypto price predictions).

### `series list`

```bash
polymarket series list [OPTIONS]
```

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--limit` | integer | 25 | Max results |
| `--offset` | integer | — | Pagination offset |

### `series get`

```bash
polymarket series get <SERIES_ID>
```

---

## Comments

### `comments list`

List comments on an entity.

```bash
polymarket comments list [OPTIONS]
```

| Flag | Type | Required | Description |
|------|------|----------|-------------|
| `--entity-type` | string | yes | Entity type (e.g., `event`) |
| `--entity-id` | string | yes | Entity ID |
| `--limit` | integer | no | Max results |
| `--offset` | integer | no | Pagination offset |

### `comments get`

Get a single comment by ID.

```bash
polymarket comments get <COMMENT_ID>
```

### `comments by-user`

Get comments by a specific user address.

```bash
polymarket comments by-user <ADDRESS>
```

---

## Profiles

### `profiles get`

Look up a public profile by wallet address.

```bash
polymarket profiles get <ADDRESS>
```

---

## Sports

### `sports list`

List available sports.

```bash
polymarket sports list
```

### `sports market-types`

List sports market types.

```bash
polymarket sports market-types
```

### `sports teams`

List teams with optional filters.

```bash
polymarket sports teams [OPTIONS]
```

| Flag | Type | Description |
|------|------|-------------|
| `--league` | string | Filter by league (e.g., NFL, NBA) |
| `--limit` | integer | Max results |

---

## Status

Check API health.

```bash
polymarket status
```

Returns the current API status string in table mode, or `{"status": "..."}` in JSON mode.
