
# AWESOME MEV REWARDS STREAM

Real-time MEV reward data is like market data feed for anyone competing inside Ethereum’s block auctions. The stream surfaces who extracted what, when, and through which builders, so every participant can price risk, spot opportunities, and react before the next block lands.

## Biggest Winners: Searchers (MEV Bots)

Searchers live and die by timing. When they subscribe to the stream, they gain:

- Early signals that an arbitrage or liquidation is heating up.
- Insight into which builders are actively extracting specific opportunities.
- Live estimates of unclaimed MEV, so they stop chasing already-drained trades.
- The ability to trigger their own bundles exactly when profitability peaks.
- Feedback loops to tune bidding strategies against builder behavior.

## What’s in the Stream?

- Per-block MEV reward tallies, broken down by builder, relayer, and validator.
- Transaction- and bundle-level attribution so you know who profited and why.
- Latency-optimized delivery over WebSocket/HTTP streams for sub-second reactions.
- Historical replay endpoints for backtesting and labeling training data.

## Sample Stream Query

Subscribe to the `TransactionBalances` stream to capture rewards as soon as they land. Filter for one or more builder/searcher payout wallets using `in` filter.

[Run Stream](https://ide.bitquery.io/Watch-MEV-Rewards-to-Builders)

```graphql
subscription WatchMEVRewards {
  EVM(network: eth) {
    TransactionBalances(
      where: {
        TokenBalance: {
          BalanceChangeReasonCode: { eq: 6 }
          Address: {
            in: [
              "0x396343362be2a4da1ce0c1c210945346fb82aa49"
              "0x4838B106FCe9647Bdf1E7877BF73cE8B0BAD5f97"
            ]
          }
        }
        Block: { Number: { gt: "0" } }
      }
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        Hash
        ValueInUSD
        MEV_reward: Value
      }
      TokenBalance {
        Address
        PreBalance
        PostBalance
        PostBalanceInUSD
      }
    }
  }
}
```


