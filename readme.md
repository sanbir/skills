# Rocket Pool Skills

Claude Code skills for interacting with Rocket Pool smart contracts via `cast` (Foundry) or Ethereum MCP tools.

## Usage

Install a skill by adding it to your project's `.claude/settings.json`:

```json
{
  "skills": [
    "/path/to/rocketpool-skills/all-in-one"
  ]
}
```

Or install individual domain skills from `split/`:

```json
{
  "skills": [
    "/path/to/rocketpool-skills/split/liquid-staking",
    "/path/to/rocketpool-skills/split/node-operations"
  ]
}
```

Each skill provides contract addresses, function signatures, ABI files, and cast examples for mainnet and Hoodi testnet.

## Structure

### `all-in-one/`

A single monolithic skill covering all ~54 Rocket Pool contracts across every domain. Use this when you want full protocol coverage in one skill.

- 53 ABI files
- All contract addresses (mainnet + Hoodi)
- Separate reference files per domain linked from the main SKILL.md

### `split/`

Six self-contained skills, one per domain. Each has its own SKILL.md with inlined function signatures, a filtered addresses.json, and only the relevant ABIs. Use these when you only need specific protocol functionality and want to keep context lean.

| Skill | Contracts | ABIs | Covers |
|-------|-----------|------|--------|
| `liquid-staking` | 2 | 2 | rETH minting/burning, deposit pool, exchange rates |
| `node-operations` | 18 | 12 | Node registration, minipools, megapools, RPL staking, bond reduction |
| `governance` | 27 | 25 | pDAO proposals/voting, oDAO membership, security council, all DAO settings |
| `network` | 10 | 8 | RocketStorage registry, balances, prices, fees, snapshots, voting delegation |
| `rewards` | 5 | 5 | Reward cycles, Merkle claims, smoothing pool, treasury |
| `tokens` | 3 | 3 | RPL ERC-20, inflation, vault, L2 token addresses |

### When to use which

- **All-in-one** — you're exploring the protocol, don't know which domain you need, or regularly work across multiple domains
- **Split** — you know exactly what you're doing (e.g., only liquid staking integrations) and want faster, more focused responses
