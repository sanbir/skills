<picture>
  <img alt="Rocket Pool - Decentralised Ethereum Staking Protocol" src="https://raw.githubusercontent.com/rocket-pool/.github/main/assets/logo.svg" width="auto" height="120">
</picture>

# Rocket Pool Skills

AI agent skill bundles for interacting with Rocket Pool smart contracts via `cast` (Foundry) or Ethereum MCP tools.

> [!WARNING]
> _These skills are provided to be used at your own risk. Exposing a private key or mnemonic phrase to an AI agent is not exercising proper risk management & puts your funds at risk. This repository is provided for informational and educational purposes only and is used entirely at your own risk. The authors and contributors assume no liability for any damages, losses, or legal claims arising from its use._

## Usage

Load one or more skill directories into your AI agent's local skills/instructions mechanism.
Each skill directory includes a `SKILL.md` plus supporting references and ABI files.

Use `all-in-one/` when you want full protocol coverage in one bundle, or load one or more domain bundles from `split/` for narrower context.

If your agent does not support local skill loading, use the same folders as prompt context:

1. Provide the relevant `SKILL.md`
2. Keep `references/addresses.json` available
3. Keep `assets/abis/` available for function signatures and tuple layouts

Example agent config format (adjust to your tool):

```json
{
  "skills": ["/path/to/rocketpool-skills/all-in-one"]
}
```

Example using split bundles:

```json
{
  "skills": [
    "/path/to/rocketpool-skills/split/liquid-staking",
    "/path/to/rocketpool-skills/split/node-operations"
  ]
}
```

Each skill provides contract addresses, function signatures, ABI files, and `cast` examples for mainnet and Hoodi testnet.

## Examples

Set environment variables once:

```bash
export RPC_URL="https://ethereum-rpc.publicnode.com"
export WALLET="0xYourWallet"
export PK="0xYourPrivateKey"
```

### Example 1: Read rETH exchange rate (`all-in-one/`)

Prompt your agent:

```text
Using the Rocket Pool all-in-one skill, get the current rETH exchange rate on mainnet with cast.
```

Typical command:

```bash
cast call 0xae78736Cd615f374D3085123A210448E74Fc6393 "getExchangeRate()(uint256)" --rpc-url $RPC_URL
```

### Example 2: Mint rETH (`split/liquid-staking/`)

Prompt your agent:

```text
Using the liquid-staking skill, mint 1 ETH worth of rETH and show the exact cast command.
```

Typical command:

```bash
cast send 0xCE15294273CFb9D9b628F4D61636623decDF4fdC "deposit()" --value 1ether --rpc-url $RPC_URL --private-key $PK
```

### Example 3: Check node RPL stake (`split/node-operations/`)

Prompt your agent:

```text
Using the node-operations skill, check the RPL stake for this node address: 0xNodeAddress.
```

Typical command pattern:

```bash
cast call <rocketNodeStaking_address> "getNodeRPLStake(address)(uint256)" 0xNodeAddress --rpc-url $RPC_URL
```

### Example 4: Pull ABI-aware signature help

Prompt your agent:

```text
Using the relevant Rocket Pool skill, find the exact function signature for a call that takes a tuple argument and generate the cast command.
```

Expected behavior:

1. Agent reads `SKILL.md` for workflow guidance
2. Agent looks up contract address in `references/addresses.json`
3. Agent uses `assets/abis/*.json` to derive the exact tuple signature
4. Agent returns a concrete `cast call` or `cast send` command

## Structure

### `all-in-one/`

A single monolithic skill covering all ~54 Rocket Pool contracts across every domain. Use this when you want full protocol coverage in one skill.

- 53 ABI files
- All contract addresses (mainnet + Hoodi)
- Separate reference files per domain linked from the main `SKILL.md`

### `split/`

Six self-contained skills, one per domain. Each has its own `SKILL.md` with inlined function signatures, a filtered `addresses.json`, and only the relevant ABIs. Use these when you only need specific protocol functionality and want to keep context lean.

| Skill             | Contracts | ABIs | Covers                                                                       |
| ----------------- | --------- | ---- | ---------------------------------------------------------------------------- |
| `liquid-staking`  | 2         | 2    | rETH minting/burning, deposit pool, exchange rates                           |
| `node-operations` | 18        | 12   | Node registration, minipools, megapools, RPL staking, bond reduction         |
| `governance`      | 27        | 25   | pDAO proposals/voting, oDAO membership, security council, all DAO settings   |
| `network`         | 10        | 8    | RocketStorage registry, balances, prices, fees, snapshots, voting delegation |
| `rewards`         | 5         | 5    | Reward cycles, Merkle claims, smoothing pool, treasury                       |
| `tokens`          | 3         | 3    | RPL ERC-20, inflation, vault, L2 token addresses                             |

### When to use which

- **All-in-one**: you're exploring the protocol, don't know which domain you need, or regularly work across multiple domains
- **Split**: you know exactly what you're doing (for example only liquid staking integrations) and want faster, more focused responses
