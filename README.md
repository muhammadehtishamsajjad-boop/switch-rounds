# switch-rounds

The Switch Protocol Switchboard's published round data, one file per round, written by the worker
and read by the DApp. Public so anyone can archive it, pin it, or recompute any round.

| Path | What it is |
|---|---|
| `rounds/000001.json` | A sealed round: every input (instrument prices, volumes, oracle readings, holder balances and transfers for the hour), the score table, the outcome, the allocations, and in live mode the acquire and commit transaction hashes. |
| `settlements/000001.json` | A settlement batch (six rounds): aggregated transfers per holder and instrument, the batch root, dust carried in and out, push status and transaction hashes. |
| `open.json` | The round currently accruing. |
| `holders.json` | Holder count at the last seal, from a replay of the token's Transfer events. |
| `genesis.json` | Unix seconds at which round 1 opened. |
| `worker.log` | One line per worker run. |

Every number is recomputable from public chain data, and the chain holds each round's score-table hash
and allocation root, so an edited file fails verification:

```bash
git clone https://github.com/muhammadehtishamsajjad-boop/switch-dapp && cd switch-dapp && npm ci
npx tsx scripts/verify.mts ../switch-rounds/rounds/000001.json
```

The worker runs from `.github/workflows/round.yml` every hour. Engine, worker and DApp: the `switch-dapp` repo.
