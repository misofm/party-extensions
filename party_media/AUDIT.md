# Release Audit — `party_media`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `main` (matches the repository's current `HEAD`)
**Date:** 2026-09-02
**Toolchain:** `sui 1.78.1-722ac4fcf484`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_media` is a state-attaching extension. It stores one nonzero Walrus
quilt ID (`u256`) as a Party dynamic field. Set/replace and idempotent clear
require the matching `PartyAdminCap`; reads are permissionless. Patch roles are
an off-chain convention, and no media bytes or funds are stored.

## Exact manifest pins

| Dependency | Repository | Revision |
|---|---|---|
| `partyos` | `https://github.com/misofm/partyos.git` | `819fde6f34c0bc7eeb57ec7340cdf13dc56b3fca` |

The manifest has no local-path or floating dependencies.

## Verification

- Package tests: **5/5**, including **2** expected-failure paths for zero ID
  and wrong-cap authorization.
- Production instruction coverage: **100.00%**.
- End-to-end scenario covers Party share, cap transfer, later cap-gated media
  write, and permissionless read from the shared Party.
- Repository aggregate: **77/77 tests on Testnet and 77/77 on Mainnet**, strict
  lint with warnings as errors; all 11 production modules are at **100.00%**.

## Published metadata

The retained `Published.toml` is the sole record of the prior immutable Testnet package id;
it is not repeated here.
It is prior-generation metadata wherever pending inputs differ. Fresh immutable
publication uses the admin CLI with `--allow-republish`; only after confirmed
success may it replace the target network block.
