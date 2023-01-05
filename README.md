This repository contains the Rust smart contracts for NearFT - an innovative automated market making DEX for NFTs on the Near blockchain.

NearFT is granted by Mintbase for development NFT infrastructure on Near

## Overview

Most NFT marketplaces nowadays operate in an order book manner and in an auction manner. To determine a final price on a marketplace such as Opensea, Mintbase or Paras, the NFT usually goes through a bidding process where the seller sets a floor price, and the cost of the NFT changes as users bid on the NFTs. In addition, the owner must manually change the price of the NFT based on the demand of the market to have their NFT sold. 

On such order book marketplaces, users need to adjust their NFTs’ prices manually following the market trend to sell the NFTs at higher or lower current prices. This model is, however, inefficient and borderline intractable to attract liquidity for NFTs. All this can take time and be frustrating for traders and content creators. Users might sell an NFT at a price lower than the market price if their manual adjustment to the NFT price is slower than the change in the NFT market price, which has been showing that it can quickly increase or decrease in a short time. 

Furthermore, most current NFT marketplaces have a very high trading fees percentage, i.e. around 4-7% for both sellers and buyers. This trading fee is very high compared to what it is in fungible trading markets. We believe that this is also one of the reasons why NFT trading volume is very low compared to fungible asset trading volume.

## Project Details

The NearFT AMM is designed to facilitate a new way to trade NFTs. The goal is to ensure that anyone can add liquidity to an NFT collection and earn trading fees. The AMM will use the supported linear curves to solve the NFT liquidity problem. There will be 3 types of pools in NearFT: 

* NFT pool: Pool creators deposit only their NFT and specify a token P that they are willing to receive as payment token (can be native token like Near or Stable coin, or even any other token on the chain) when users buy their NFTs from the pool. The creators will receive the specified token P as others swap their tokens for the NFTs in the pools. The creators can set an initial floor price in the token P. The price of NFTs in the pool will be automatically increased when users trade P for NFTs. This is a great way for content creators to monetize their artworks by creating NFT pools.
* Token pool: Pool creators deposit only their specified payment token P and denote the NFT they are willing to buy. The pool creator can set an initial price in P for NFTs that the pool owner is willing to buy. Any user that owns an NFT specified by the pool can sell the NFT for an amount of token P.
* TRADING pool: Pool creators can create a pool by:
Specifying a payment token P and an NFT collection contract
Deposit a list of NFTs from the collection to the pool
Deposit an initial liquidity in P for the pool
It is similar to a UniSwap liquidity pool. The creator can set the initial price for NFTs in the pool. 
Anyone can buy/sell NFTs using NEAR. Creators of these pools will mostly earn trading fees from the pools.

### Unique features in TRADING pools:
* Pool creators can set a time period that specifies a locked period for their initial liquidity. A pool creator then can only withdraw their provided initial liquidity after the locked period. The locked period will be then fully on-chain and transparent to all users
* Liquidity provision: Any one can provide liquidity to a trading by providing a list of NFT tokens from the same collection specified by the pool and a corresponding amount of the payment token specified by the pool. The amount of the payment token to provide is equal to the number of provided NFTs multiplied by the NFT price in the pool at the time the user provides liquidity. Once provided, the user will be given Liquidity Pool (LP) token for that pool that is proportional to the pool share they have in the pool

### Bonding curves
The NearFT AMM is designed to facilitate a new way to trade NFTs. The goal is to ensure that anyone can add liquidity to an NFT collection and earn trading fees. The AMM will use the following supported price bonding curves to adjust NFT price in the pools when there are swaps:

* Linear Curve: The linear curve performs an additive operation to update the price. If a user buys an NFT from a pool, the price of an NFT will be the current price plus a delta. On the other hand, if a user sells an NFT to the pool, the price of an NFT will be the current price minus a delta.

* Exponential Curve: The exponential curve makes a multiplicative operation on NFT token price. The delta is evaluated as a multiplier or percentage with 10^8 as 1. For example, if delta is 1e8 + 1e7, this represents a 10% change in price each time. If a user buys an NFT from a pool, the next price will be multiplicatively delta more. Conversely, if a user sells an NFT to a pool, the next price it will quote to purchase NFTs will be multiplicatively delta less.

More curves will be researched and developed to NearFT over time.

### Liquidity Pool (LP) token and liquidity handling

This works somewhat similar to how users provide and remove liquidity from Uniswap pools. However there are a few unique features that come from the uniqueness nature of NFTs
* A pool creator provides NFTs and an initial liquidity (i.e. in P token that can be a native token or any token on the supported chain) for trading, and receiving an initial amount of LP token for that pool, which basically represents 100% of the liquidity of the pool at the pool creation time
* The pool creator specifies whether the initial liquidity in the pool is locked till a certain specified time or not, until which the pool creator cannot burn the LP token to withdraw the pool liquidity
* Any liquidity addition after the pool creation time happens as follows:
  * Any one can provide liquidity including a list of NFT tokens and the corresponding liquidity in P. The amount of P needed to provide is equal to the number of provided NFTs multiplied by the current NFT price specified by the bonding curve in the pool
  * The liquidity provider will receive an amount of LP token of the pool that is corresponding to the pool share the added liquidity represents in the pool
  * The LP token can be transferred to anyone or can be used in any farming applications that have been widely used in all DeFi applications to incentivize users for providing liquidity. This practice has been shown to be very efficient in building up the liquidity pool for fungible assets in Uniswap-like AMMs. NearFT thus aims to greatly increase the liquidity and trading volume for NFT markets which is low compared to the trading volume of fungible assets.
  * Users with the LP token can withdraw/remove liquidity to retrieve NFTs and liquidity in P any time. The only exception is that the pool creator cannot transfer their LP token or withdraw liquidity before the locked period ends. 

### NFT swap types

There are 4 types of swaps 
* Token-to-NFTs: buy a random list of NFTs or a specified list of NFTs from a pool with the payment token specified in the pool
* NFTs-to-Token: sell a list of NFTs to Token.
* NFTs-to-NFTs: This is an arbitration or FlashSwap for NFTs on NearFT. Any user can make this swap when there is a difference/spread in the prices of NFTs of the same collection in different liquidity pools. Users can swap from a list of NFTs for the specified payment token on a pool, then from the payment token to NFTs on another pool, which can be followed by swaps on any number of pools to get the desired NFTs. The owners of pools in this swap type earn fees, just like liquidity providers on Uniswap-like AMMs.
* Token-to-Token: this is another arbitration or FlashSwap where traders can also earn Token when there is a spread of the same NFT collection in different pools. 

## Smart contracts
* NearFT is written using Rust
* NearFT was `audited` by an automatic smart contract auditing tool [Rustle](https://github.com/blocksecteam/rustle)

### Build
* run: ./build_local.sh

## Testnet
* NearFT is currently deployed on Near Testnet at https://nearft.nearfi.finance/ 

## Roadmap
* NearFT is expected to launch its Mainnet Q1 2023
* NearFT is integrating with Mintbase API to let users mint and trade NFTs directly on NearFT