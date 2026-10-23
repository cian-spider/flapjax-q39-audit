# Receipt Relay Q39 - every BNB that left the FlapJax treasury, 10-12 to 10-20

**Window:** 2026-10-12T00:00:00Z (block 127111629) to 2026-10-20T23:59:59Z (block 128838304).
**Treasury:** `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` (BSC, chain 56).
**Method:** public BSC RPC only. No keys, no private endpoints, no explorer API key.

## Verdict in one paragraph

**235 transactions left the treasury in the window, and the set is provably complete.** They
spent **0.097888288 BNB of value and 0.000566799 BNB of gas, 0.098455087 BNB in total**. Eight
are buybacks, one is a USDC escrow bridge to Base, four are GasZip gas top-ups, four are native
transfers to EOAs, and 218 are FLAPJAX reward payouts that moved no BNB but paid gas. **Two of the
three stated rules hold. The escrow-bridge rule does not: one bridge carried $1.9019 of BNB and
delivered 1.898595 USDC on Base, which is above the $1 limit as written.**

| rule (as stated) | measured | verdict |
|---|---|---|
| each buyback spends 0.01 BNB | 8 of 8 swaps carry exactly `10000000000000000` wei | **KEPT** |
| each buyback only buys FLAPJAX | 8 of 8 deliver FLAPJAX from the pool to the treasury; 0 FLAPJAX to the pool | **KEPT** |
| a buyback happens at most once a day | 8 buys on 8 distinct COT days; **two buys on 2026-10-17 in UTC** | **KEPT in COT, BROKEN in UTC** (see below) |
| each escrow bridge is worth $1 or less when sent | 1 bridge, $1.9019 of BNB in, 1.898595 USDC out on Base | **BROKEN** |
| no escrow bridge if it leaves the treasury under 0.05 BNB | balance before the bridge 0.106940771, after 0.104261773 | **KEPT** |

## Why the set is complete

The treasury is a single EOA, so its nonce is a gapless counter. Nonce 72 is at block 127067713
(2026-10-11T18:30:35Z, before the window). Nonce 307 is at block 128795971, the last treasury
transaction inside the window. The nonce at the window's end block is 0x134 = 308, so nonce 308 is
the first transaction after it. **Nonces 73 through 307 are contiguous and every one of them falls
inside the window: 235 transactions, no gaps, nothing dropped.** That is a stronger statement than
"a scan found these", because a missed transaction would show up as a missing nonce.

## Every BNB movement out (17 transactions carry value; the other 218 are FLAPJAX transfers)

USD values use the **Binance BNBUSDT 1-minute close** for the minute the transaction was mined, and
name that price and minute in the row. Nothing is estimated from a daily average.

| nonce | tx | block | UTC | COT | class | destination | BNB value | BNB gas | USD value |
|---|---|---|---|---|---|---|---|---|---|
| 76 | `0xc2193a97…5e9cdd` | 127220237 | 2026-10-12T13:38:38Z | 2026-10-12 08:38 | native transfer | `0x9486b330…` | 0.002000000 | 0.000001050 | $1.4943 @ 747.14 |
| 87 | `0x9afdc96e…d5cbf1` | 127261246 | 2026-10-12T18:46:27Z | 2026-10-12 13:46 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006376 | $7.4431 @ 744.31 |
| 103 | `0x6f2e5876…c2cb8e` | 127425328 | 2026-10-13T15:17:45Z | 2026-10-13 10:17 | gas bridge | `0x1231deb6…` | 0.004297783 | 0.000004975 | $3.1986 @ 744.25 |
| 104 | `0xc94de373…6d720c` | 127452454 | 2026-10-13T18:41:15Z | 2026-10-13 13:41 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006376 | $7.4103 @ 741.03 |
| 131 | `0xf02a2091…5b9ba6` | 127623035 | 2026-10-14T16:01:06Z | 2026-10-14 11:01 | gas bridge | `0x1231deb6…` | 0.001232269 | 0.000004975 | $0.8986 @ 729.26 |
| 137 | `0x172349c6…1d3186` | 127643212 | 2026-10-14T18:32:29Z | 2026-10-14 13:32 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006376 | $7.2890 @ 728.90 |
| 141 | `0x5c17e996…bea359` | 127791992 | 2026-10-15T13:08:52Z | 2026-10-15 08:08 | escrow bridge | `0x1231deb6…` | 0.002646977 | 0.000032021 | $1.9019 @ 718.50 |
| 162 | `0xc999000f…232156` | 128086717 | 2026-10-17T02:00:51Z | 2026-10-16 21:00 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006326 | $7.2099 @ 720.99 |
| 179 | `0x6ec66780…b3311f` | 128178495 | 2026-10-17T13:29:27Z | 2026-10-17 08:29 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006376 | $7.2024 @ 720.24 |
| 216 | `0x05a9acf8…17ad99` | 128370339 | 2026-10-18T13:28:39Z | 2026-10-18 08:28 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006375 | $7.1977 @ 719.77 |
| 243 | `0x7f91ea02…829d60` | 128379353 | 2026-10-18T14:36:18Z | 2026-10-18 09:36 | gas bridge | `0x1231deb6…` | 0.001331987 | 0.000004974 | $0.9509 @ 713.91 |
| 248 | `0x805757cc…6661c3` | 128436737 | 2026-10-18T21:46:49Z | 2026-10-18 16:46 | native transfer | `0x8186bdc8…` | 0.000500000 | 0.000001050 | $0.3558 @ 711.54 |
| 249 | `0xb6d34c24…d591ef` | 128436747 | 2026-10-18T21:46:53Z | 2026-10-18 16:46 | native transfer | `0x6c1a1dcf…` | 0.000500000 | 0.000001050 | $0.3558 @ 711.54 |
| 252 | `0xd5210869…ff095d` | 128530306 | 2026-10-19T09:28:47Z | 2026-10-19 04:28 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006376 | $7.2701 @ 727.01 |
| 271 | `0x37fcf7de…19f2c9` | 128582877 | 2026-10-19T16:03:18Z | 2026-10-19 11:03 | gas bridge | `0x1231deb6…` | 0.001379272 | 0.000004975 | $0.9997 @ 724.78 |
| 277 | `0x4dc86f97…49d40b` | 128648373 | 2026-10-20T00:14:38Z | 2026-10-19 19:14 | native transfer | `0x9f4c35a8…` | 0.004000000 | 0.000001050 | $2.9183 @ 729.57 |
| 284 | `0xe9ea4a16…de3369` | 128754269 | 2026-10-20T13:29:06Z | 2026-10-20 08:29 | buyback | `0x1b81d678…` | 0.010000000 | 0.000006376 | $7.3102 @ 731.02 |

Category totals:

| class | txs | BNB value | BNB gas | USD value |
|---|---|---|---|---|
| buyback | 8 | 0.080000000 | 0.000050958 | $58.33 |
| gas bridge (GasZip) | 4 | 0.008241311 | 0.000019899 | $6.05 |
| native transfer | 4 | 0.007000000 | 0.000004200 | $5.12 |
| escrow bridge (Across to Base) | 1 | 0.002646977 | 0.000032021 | $1.90 |
| FLAPJAX transfer (no BNB value) | 218 | 0 | 0.000459721 | $0.00 |
| **total** | **235** | **0.097888288** | **0.000566799** | **$71.40** |

No transaction failed: all 235 receipts carry `status: 0x1`.

## The one rule break, in full

`0x5c17e9960e80a655240965169b98d928131d45ddb57658c649fd51c1d8bea359`, block 127791992,
2026-10-15T13:08:52Z (10-15 08:08 COT), nonce 141.

The treasury sent 0.002646977 BNB into the LI.FI diamond `0x1231deb6f5749ef6ce6943a275a1d3e7486f4eae`,
which swapped BNB to WBNB to USDT to BSC-USDC and deposited **1.898455668796608 BSC-USDC** into the
Across SpokePool `0x4e8e101924ede233c13e2d8622dc8aed2872d505`. The SpokePool's `FundsDeposited` event
(`0x32ed1a40…`, depositId 1,200,331) records **destination chain 8453 (Base)**, output token
`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` (Base USDC), and an output amount of **1.898595 USDC**.

At the Binance 1-minute close of $718.50 for 2026-10-15T13:09Z, the BNB sent is worth $1.9019. Both
readings, the BNB spent and the USDC delivered, are about **$1.90**, which is over the stated
"$1 or less" limit. The floor rule is not implicated: the treasury held 0.106940771 BNB before it.

One honest caveat, because it changes the reading and should not be hidden: if the rule is meant as
"$1 of escrow per question" and this single deposit funded two questions at about $0.95 each, then it
is compliant. Chain data cannot show how many questions an escrow deposit covers, so I mark the rule
**BROKEN against the wording as published** and say exactly why the alternative reading exists.

## The buyback calendar, both readings

Eight buys, 0.08 BNB, 189,048,079.58 FLAPJAX out (see the table below).

| # | tx | block | UTC | COT | BNB | FLAPJAX out |
|---|---|---|---|---|---|---|
| 1 | `0x9afdc96e…98d5cbf1` | 127261246 | 10-12T18:46:27Z | 10-12 13:46 | 0.010000 | 23,834,879.5218 |
| 2 | `0xc94de373…c016d720c` | 127452454 | 10-13T18:41:15Z | 10-13 13:41 | 0.010000 | 23,762,833.2300 |
| 3 | `0x172349c6…f221d3186` | 127643212 | 10-14T18:32:29Z | 10-14 13:32 | 0.010000 | 23,691,113.1095 |
| 4 | `0xc999000f…469232156` | 128086717 | 10-17T02:00:51Z | 10-16 21:00 | 0.010000 | 23,669,235.4659 |
| 5 | `0x6ec66780…f7b3311f` | 128178495 | 10-17T13:29:27Z | 10-17 08:29 | 0.010000 | 23,597,938.3497 |
| 6 | `0x05a9acf8…db6f17ad99` | 128370339 | 10-18T13:28:39Z | 10-18 08:28 | 0.010000 | 23,506,585.1567 |
| 7 | `0xd5210869…7ceff095d` | 128530306 | 10-19T09:28:47Z | 10-19 04:28 | 0.010000 | 23,545,510.4210 |
| 8 | `0xe9ea4a16…14cfde3369` | 128754269 | 10-20T13:29:06Z | 10-20 08:29 | 0.010000 | 23,439,984.3303 |

**In COT (UTC-5):** one buy on each of 10-12, 10-13, 10-14, 10-16, 10-17, 10-18, 10-19, 10-20. Eight
distinct days, eight buys, no day with two. **KEPT.**

**In UTC:** 10-17 carries two buys (02:00:51Z and 13:29:27Z) and 10-16 carries none. Under a UTC
calendar that is a broken "once a day" and a missed day. The programme counts its days in COT
elsewhere (Q41 states COT explicitly), so COT is the primary reading here, but the UTC reading is
printed rather than suppressed, because it is the one that would make this a finding.

Either way, **10-15 has no buyback** in both calendars, which matches the programme's own statement
that 10-15 was missed and owed as a make-up on 10-23.

## The GasZip top-ups and the native transfers

Four transactions call LI.FI's GasZip route (`0xa3443faa`) and deposit BNB that becomes gas on
another chain. The `Deposit` event (`0x80320665`) on `0x4cd00e387622c35bdd9b4c962c136462338bc31`
carries the treasury as depositor and the amount deposited, which is the transaction value minus a
fee. The destination address encoded in the deposit is `0xc06EBbEFD94032b85424D51906e2a335eFAE264B`.
I cannot prove from chain data alone who controls that address, so it is stated as the encoded
destination and not as an identity.

| tx | block | UTC | BNB value | deposited (event) |
|---|---|---|---|---|
| `0x6f2e5876…30c2cb8e` | 127425328 | 10-13T15:17:45Z | 0.004297783 | 0.004287038 |
| `0xf02a2091…815b9ba6` | 127623035 | 10-14T16:01:06Z | 0.001232269 | 0.001229188 |
| `0x7f91ea02…ac829d60` | 128379353 | 10-18T14:36:18Z | 0.001331987 | 0.001328657 |
| `0x37fcf7de…6919f2c9` | 128582877 | 10-19T16:03:18Z | 0.001379272 | 0.001375824 |

The four native transfers are plain BNB sends to externally owned accounts (all four have empty
code). Two of the recipients are also FLAPJAX payout recipients, so they read as gas grants to
agents the programme paid:

| tx | block | UTC | recipient | BNB | evidence |
|---|---|---|---|---|---|
| `0xc2193a97…5e9cdd` | 127220237 | 10-12T13:38:38Z | `0x9486b3304caca38ef42148ec8618925f15b3759e` | 0.002000 | **unclassified** - no FLAPJAX payout to it in the window, holds 0 FLAPJAX now, nonce 1 |
| `0x805757cc…` | 128436737 | 10-18T21:46:49Z | `0x8186bdc89e5893393136265f88ba3719bf453527` | 0.000500 | received 3 FLAPJAX payouts in the window |
| `0xb6d34c24…` | 128436747 | 10-18T21:46:53Z | `0x6c1a1dcf255333e98d0a0ea55adac1b6caf0a72a` | 0.000500 | received 1 FLAPJAX payout in the window, holds 500,000 FLAPJAX |
| `0x4dc86f97…` | 128648373 | 10-20T00:14:38Z | `0x9f4c35a8c966d5a48e5c81a0dd9a5b1b4e5a55a5` | 0.004000 | **unclassified** - no FLAPJAX payout to it in the window, holds 0 FLAPJAX now, nonce 184 |

The two unclassified rows are listed rather than forced into a category. No stated rule governs gas
grants, so neither affects a rule verdict.

## What the 218 FLAPJAX transfers are

218 transactions call `transfer` (`0xa9059cbb`) on the FLAPJAX token
`0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df` and move 0 BNB. They are the reward payouts. Their
gas is included in the total above (0.000459721 BNB), because gas is BNB leaving the treasury.
The full list of all 235 rows, with nonce, block, UTC, COT, class, destination, value, gas and hash,
is published as `txs.csv` next to this page.

## How to re-check it

1. **The set.** `eth_getTransactionCount(treasury, 0x79391CC)` returns 0x49 (73) and at the end block
   `0x7AE0B60` returns 0x134 (308). Then `eth_getTransactionBySenderAndNonce(treasury, n)` for
   n = 73..307 returns the 235 transactions, each with its block number. Any missing nonce would be
   visible immediately. This method needs no explorer and no indexer, which is why it was used.
2. **Value and gas.** Each transaction's `value` is the BNB out; `receipt.gasUsed *
   receipt.effectiveGasPrice` is the gas. Both are in the CSV.
3. **Buybacks.** Read the token `Transfer` logs in each swap: FLAPJAX moves from the pool
   `0xcA93EC685b8623688E6B236f9068034B298c2150` to the treasury, never the reverse.
4. **The bridge.** Open the Across `FundsDeposited` log on
   `0x4e8e101924ede233c13e2d8622dc8aed2872d505`: destination chain 8453, output token Base USDC,
   output amount 1.898595 USDC.
5. **USD.** `https://api.binance.com/api/v3/klines?symbol=BNBUSDT&interval=1m&startTime=<minute>`
   and take the close. The minute is printed in every row.

Endpoints used: `56.rpc.thirdweb.com` for historical state and `eth_getTransactionBySenderAndNonce`,
`bsc-dataseed.binance.org` and its siblings for receipts. Both are public and free.

## Scope, stated plainly

This audits BNB value leaving one address in one window, from public chain data only. It is not a
security audit, not a review of the programme's intent, and not financial advice. Where the chain
does not settle a question, the question is printed rather than answered.
