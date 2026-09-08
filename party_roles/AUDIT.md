# Release Audit — `party_roles`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `main` (matches the repository's current `HEAD`)
**Date:** 2026-09-02
**Toolchain:** `sui 1.78.1-722ac4fcf484`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_roles` is a state-attaching extension. It stores up to 12
`ArtistRole` values through `typed_set`: eight closed canonical variants plus a
validated `Custom` name of 1–60 bytes. All mutations require the matching
`PartyAdminCap`; final removal reclaims the field, clear is idempotent, and
views are permissionless. Exact enum equality deliberately distinguishes a
canonical role from a same-spelled custom role.

## Exact manifest pins

| Dependency | Repository | Revision |
|---|---|---|
| `partyos` | `https://github.com/misofm/partyos.git` | `819fde6f34c0bc7eeb57ec7340cdf13dc56b3fca` |
| `typed_set` | `https://github.com/unconfirmedlabs/typed_set.git` | `b37474cbde166b7ddf8a3b615cd89f90182ace6f` |

The manifest has no local-path or floating dependencies.

## Verification

- Package tests: **9/9**, including **7** expected-failure paths covering both
  custom-name validators, duplicate, missing item, capacity, wrong cap, and
  authorization-before-capacity.
- Production instruction coverage: **100.00%**.
- End-to-end scenario covers Party share, cap transfer, later role write, and
  permissionless read from the shared Party.
- Repository aggregate: **77/77 tests on Testnet and 77/77 on Mainnet**, strict
  lint with warnings as errors; all 11 production modules are at **100.00%**.

## Published metadata

The retained `Published.toml` is the sole record of the prior immutable Testnet package id;
it is not repeated here.
It is prior-generation metadata wherever pending inputs differ. Fresh immutable
publication uses the admin CLI with `--allow-republish`; only after confirmed
success may it replace the target network block.
