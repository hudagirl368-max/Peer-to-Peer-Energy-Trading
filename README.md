# Peer-to-Peer Energy Trading ⚡

A simple Clarity smart contract enabling producers to list energy offers and buyers to place orders, confirm delivery, raise disputes, and settle balances of energy credits tracked on-chain.

## Features

- Register producers
- Mint energy credits to a producer balance
- Create, update, deactivate/reactivate, and cancel offers with expiry based on stacks-block-height
- Place orders with min-quantity checks and automatic reservation from the offer
- Two-sided confirmation flow and dispute handling with admin resolution
- Read-only views for balances, offers, orders, and simple stats

## Files

- `Clarinet.toml`
- `contracts/Peer-to-Peer-Energy-Trading.clar`

## Quickstart

1) Create a new Clarinet project (or use an empty folder):

```bash
clarinet new peer-to-peer-energy-trading
```

2) Add the contract file and Clarinet.toml entries as in this repo. Alternatively, generate via Clarinet then replace contents:

```bash
clarinet contract new Peer-to-Peer-Energy-Trading
```

3) Fix Windows line endings to LF to avoid parser issues:

```powershell
(Get-Content "contracts/Peer-to-Peer-Energy-Trading.clar" -Raw).Replace("`r`n", "`n") | Set-Content "contracts/Peer-to-Peer-Energy-Trading.clar" -NoNewline
(Get-Content "Clarinet.toml" -Raw).Replace("`r`n", "`n") | Set-Content "Clarinet.toml" -NoNewline
```

4) Check compilation:

```bash
clarinet check
```

## Usage

- Register a producer:

```clarity
(contract-call? .peer-to-peer-energy-trading register-producer)
```

- Set admin (first caller sets it):

```clarity
(contract-call? .peer-to-peer-energy-trading set-admin)
```

- Mint energy credits for a registered producer:

```clarity
(contract-call? .peer-to-peer-energy-trading mint-energy u1000)
```

- Create an offer of energy with price, min-qty, and expiry-in-blocks:

```clarity
(contract-call? .peer-to-peer-energy-trading create-offer u500 u2 u50 u144)
```

- Place an order against an offer:

```clarity
(contract-call? .peer-to-peer-energy-trading place-order u1 u100)
```

- Seller confirms delivery, buyer confirms receipt; settlement credits buyer’s balance:

```clarity
(contract-call? .peer-to-peer-energy-trading seller-confirm-delivery u1)
(contract-call? .peer-to-peer-energy-trading buyer-confirm-receipt u1)
```

- Raise a dispute, admin resolves either by refunding to offer availability or releasing energy to buyer:

```clarity
(contract-call? .peer-to-peer-energy-trading raise-dispute u1)
(contract-call? .peer-to-peer-energy-trading admin-resolve-refund-buyer u1)
(contract-call? .peer-to-peer-energy-trading admin-resolve-release-to-buyer u1)
```

## Notes

- Uses `stacks-block-height` for timing.
- No token transfers of STX are performed; balances represent internal energy credits.

## Git

- Commit message:

Add peer-to-peer energy trading contract, config, and docs

- PR title:

Add P2P Energy Trading Clarity contract

- PR description:

This PR adds a peer-to-peer energy trading contract with producer registration, energy minting, offer lifecycle, order placement, confirmation flow, dispute handling, and read-only views. Includes Clarinet config and README with setup and usage instructions. ⚡
