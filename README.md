# Fund Me

A minimal crowdfunding smart contract built with [Foundry](https://book.getfoundry.sh/). Users can send ETH to the contract; the owner can withdraw the balance. Funding requires a minimum of **$5 USD** (converted via a Chainlink ETH/USD price feed).

## Features

- Accept ETH through `fund()`, `receive()`, or `fallback()`
- Enforce a minimum funding amount using Chainlink price feeds
- Owner-only withdrawal (`withdraw` and gas-optimized `cheaperWithdraw`)
- Network-aware deployment with mock price feeds on local Anvil
- Deployment and interaction scripts for local, Sepolia, and zkSync networks

## Project Structure

```
src/
  FundMe.sol          # Main crowdfunding contract
  PriceConverter.sol  # Library for ETH/USD conversion
script/
  DeployFundMe.s.sol  # Deployment script
  HelperConfig.s.sol  # Network-specific Chainlink feed addresses
  Interactions.s.sol  # Fund and withdraw scripts
test/
  unit/               # Unit tests
  integration/        # Integration tests
  mocks/              # MockV3Aggregator for local testing
```

## Prerequisites

- [Foundry](https://book.getfoundry.sh/getting-started/installation)
- A `.env` file for Sepolia deployment (see below)

## Installation

```shell
make install
```

Or manually:

```shell
forge install cyfrin/foundry-devops@0.2.2
forge install smartcontractkit/chainlink-brownie-contracts@1.1.1
forge install foundry-rs/forge-std@v1.8.2
```

## Build

```shell
forge build
```

For zkSync:

```shell
make zkbuild
```

## Test

```shell
forge test
```

For zkSync-specific tests:

```shell
make zktest
```

## Local Development

Start a local Anvil node:

```shell
make anvil
```

In another terminal, deploy to Anvil:

```shell
make deploy
```

Fund the contract (set `SENDER_ADDRESS` in the Makefile first):

```shell
make fund
```

Withdraw as the owner:

```shell
make withdraw
```

## Deploy to Sepolia

Create a `.env` file with:

```shell
SEPOLIA_RPC_URL=<your_sepolia_rpc_url>
ACCOUNT=<your_keystore_account_name>
ETHERSCAN_API_KEY=<your_etherscan_api_key>
```

Then deploy:

```shell
make deploy ARGS="--network sepolia"
```

## Deploy to zkSync

Start the local zkSync node:

```shell
make zk-anvil
```

Deploy locally:

```shell
make deploy-zk
```

For zkSync Sepolia, add `ZKSYNC_SEPOLIA_RPC_URL` to your `.env` and run:

```shell
make deploy-zk-sepolia
```

## Manual Script Usage

Deploy:

```shell
forge script script/DeployFundMe.s.sol:DeployFundMe \
  --rpc-url <rpc_url> \
  --private-key <private_key> \
  --broadcast
```

Fund:

```shell
forge script script/Interactions.s.sol:FundFundMe \
  --rpc-url <rpc_url> \
  --private-key <private_key> \
  --broadcast
```

Withdraw:

```shell
forge script script/Interactions.s.sol:WithdrawFundMe \
  --rpc-url <rpc_url> \
  --private-key <private_key> \
  --broadcast
```

## Makefile Commands

| Command | Description |
|---------|-------------|
| `make build` | Compile contracts |
| `make test` | Run tests |
| `make anvil` | Start local Anvil node |
| `make deploy` | Deploy to local Anvil |
| `make deploy ARGS="--network sepolia"` | Deploy to Sepolia |
| `make fund` | Send ETH to deployed contract |
| `make withdraw` | Withdraw contract balance (owner only) |
| `make format` | Format Solidity code |
| `make snapshot` | Generate gas snapshots |

## Dependencies

- [forge-std](https://github.com/foundry-rs/forge-std) — Foundry testing utilities
- [chainlink-brownie-contracts](https://github.com/smartcontractkit/chainlink-brownie-contracts) — Chainlink price feed interfaces
- [foundry-devops](https://github.com/cyfrin/foundry-devops) — Deployment tracking and zkSync helpers

## License

MIT
