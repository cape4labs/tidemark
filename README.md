> 🚀 **Project for the [Portaldot Hacker House 2026](https://dorahacks.io/hackathon/portaldot-hacker-house-2026/detail#highlights) hackathon**

# Resilient Oracle MVP

A fault-tolerant price oracle for V3 that demonstrates three things: testnet readiness, safe behavior during failures, and practical value for other smart contracts.

## How it works

Several independent TypeScript reporters fetch prices from external sources, calculate a local median, and submit reports to `OracleAggregator`. The contract filters out stale data, checks quorum, and calculates the median of fresh reports on read. The `PriceGatedVault` consumer uses this price and rejects operations when the data is stale or quorum is lost.

```text
Price sources → Reporters → OracleAggregator → PriceGatedVault
                              ↘ Dashboard
```

## Project structure

- `contracts/` — smart contracts and tests.
- `reporter/` — price reporter service.
- `dashboard/` — monitoring interface.

## Oracle features

- per-feed parameters: `decimals`, `maxAge`, `minReporters`, and `maxDeviationBps`;
- `AggregatorV3Interface` compatibility;
- reporter allowlist and feed administration;
- outlier-resistant median while fewer than half of the reporters are malicious;
- automatic stale-report filtering without a separate transaction;
- `PriceSubmitted` and `OutlierFlagged` events for monitoring.

## Judge demo

1. Disable one of five reporters — the median continues to work.
2. Submit a price ten times higher — the median does not move and the reporter is flagged as an outlier.
3. Disable three of five reporters — quorum is lost and `PriceGatedVault` fails safely.

## MVP status and limitations

Reporters are permissioned and added by the owner; staking and slashing are not included yet. Price sources are centralized APIs. The source for `POT/USD` still needs to be agreed on: if there is no sufficiently liquid market, a test or synthetic source will be used and this limitation will be disclosed.

Smart-contract code will be created only from October 14, during the hackathon period. Before then, only the specification, architecture, and demo plan are prepared.

## License

This project is distributed under MIT Licence.
