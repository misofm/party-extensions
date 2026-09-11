# Release Audit — `party_media`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `codex/rich-events`
**Date:** 2026-09-11
**Toolchain:** `sui 1.79.0-46f18562f1f5`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_media` is a state-attaching extension. It stores one nonzero Walrus
quilt ID (`u256`) as a Party dynamic field. Set/replace and clear require the
matching `PartyAdminCap`; reads are permissionless. Rich set events carry the
party/cap addresses, existence bit, and prior/resulting quilt ids (129-byte
payload), while clear events carry the party/cap addresses and removed quilt
id (96-byte payload). Patch roles are an off-chain convention, and no media
bytes or funds are stored.

## Exact manifest pins

| Dependency | Repository | Revision |
|---|---|---|
| `partyos` | `https://github.com/misofm/partyos.git` | `841a875a4989082a0ebeb1beb464b71f9ea2bd73` |

The manifest has no local-path or floating dependencies.

## Verification

- Package tests: **9/9**, including **6** expected-failure paths covering zero
  validation precedence, wrong-cap set on absent/present state, and
  wrong-cap clear on absent/present state.
- Strict Testnet and Mainnet lint builds passed with warnings as errors.
- Production instruction coverage: **100.00%** on both Testnet and Mainnet.
- End-to-end scenario covers Party share, cap transfer, later cap-gated media
  write, and permissionless read from the shared Party, with views confirmed
  event-silent.
## Published metadata

The retained `Published.toml` is the sole record of the prior immutable Testnet package id;
it is not repeated here.
It is prior-generation metadata wherever pending inputs differ. Fresh immutable
publication uses the admin CLI with `--allow-republish`; only after confirmed
success may it replace the target network block.
