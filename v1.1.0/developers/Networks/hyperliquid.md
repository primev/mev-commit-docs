# Hyperliquid Network (Early Access)

Primev is piloting mev-commit support for the Hyperliquid network.

## Architecture

- **Bootnode requirement**: A separate bootnode IP must be configured for Hyperliquid.
- **Provider model**: Hyperliquid validators act as providers.
- **Bidder setup**: Bidders must restrict `allowedProviders` to specific Hyperliquid validators.

## How to Connect as a Provider

1. Run a Hyperliquid validator.
2. Deploy your mev-commit provider node behind the same machine.
3. Configure to use the bootnode IP: <tba>.

## How to Connect as a Bidder

1. Deploy your mev-commit bidder node.
2. Configure your bidder to send to Hyperliquid providers only.

We suggest running a separate bidder node for your Ethereum and Hyperliquid operations during early access.

_See `config.toml` for relevant sections._

## Status

- ✅ Custom network support (manual setup)
- 🔜 Native integration with network-aware booting
- 🔜 Oracle and provider registry upgrades

> ⚠️ Native Hyperliquid support is **coming soon**. Stay tuned for easier multi-network initialization.
