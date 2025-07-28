# LEP3 - SuperNode Dual-Source Stake Validation

|  |  |
| --- | --- |
| **Proposal ID** | TBD (submit via `lumera-gov` CLI) |
| **Upgrade Name** | `v1.7.0 Aurora Vortex` |
| **Target Height** | Testnet-2: ~2025-07-30
Mainnet: + 2 weeks (TBC) |
| **Core Change** | Dual-Source Stake Validation for SuperNode registration |
| **Authors** | Lumera Core Team |

---

## 1. Summary

This proposal introduces **Dual-Source Stake Validation**, allowing the SuperNode minimum-stake requirement to be met by *self-delegation + delegation from the SuperNode account*.  The SuperNode account may be a **vesting** (delayed or permanently locked) account funded by the Lumera Foundation or other sponsors.

## 2. Motivation

- **Lower a barrier to entry** for competent operators with limited upfront capital.
- **Foundation incentive lever**: distribute locked tokens that secure the network without immediate sell-pressure.
- **Operational flexibility** during bull/bear cycles where self-stake might fluctuate.

## 3. Specification

### 3.1 Protocol Changes

- **Keeper update** `CheckValidatorSupernodeEligibility`.
- Hooks (`AfterDelegationModified`, etc.) now re-evaluate the **combined stake**.

### 3.2 Genesis / Migration

- No DB migration.
- Existing SuperNodes remain active **if combined stake ≥ requirement**.

### 3.3 Governance Parameters

| Parameter | Old | New | Governance-controllable |
| --- | --- | --- | --- |
| `MinimumStakeForSN` | 25 000 LUME mainnet
10 000 LUME testnet | *unchanged* | ✔ |

## 4. Backwards Compatibility

Pure add-only change; no existing API breaks.  Legacy txs without `supernode_account` continue to work.

## 5. Implementation Plan

1. **Code complete** & unit tests — July 24
2. **v1.7.0 tag** & Testnet-2 upgrade — Jul 30
3. **Audit review window** — 1 week
4. **Mainnet governance proposal** — ~Aug 11
5. **Mainnet upgrade** — ~Aug 15

## 6. Risks

- **Sybil via foundation vesting**: mitigated by mandatory self-delegation > 0.
- **Edge cases in hooks** when both delegations unbond simultaneously — covered in integration tests.

## 7. Reference Implementation

- PR ##40,41,42 (lumera/x/supernode)
- Unit tests `supernode_test.go`
