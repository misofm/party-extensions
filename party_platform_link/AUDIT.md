# Release Audit — `party_platform_link`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `codex/rich-events`
**Date:** 2026-09-11
**Toolchain:** `sui 1.79.0-46f18562f1f5`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_platform_link` is the state-attaching bridge for pure platform payloads.
It generically sets/replaces or clears one `PlatformLink<Data>` dynamic field on
a Party, gates writes with the matching `PartyAdminCap`, and provides
permissionless optional reads. The primitive owns the authoritative rich
`PlatformLinkSetEvent<Data>` and `PlatformLinkRemovedEvent<Data>` emissions;
the wrapper's retained `LinkSetEvent<Data>` and `LinkClearedEvent<Data>` types
are inert compatibility declarations. Clear authenticates before its
absent-state no-op. The package adds no payload validation, funds, Vault
borrowing, Action logic, or entry automation.

## Exact manifest pins

| Dependency | Kind | Location | Mode |
|---|---|---|---|
| `partyos` | Git pin | `https://github.com/misofm/partyos.git` @ `841a875a4989082a0ebeb1beb464b71f9ea2bd73` | production |
| `platform_link` | local-path | `../lib/platform_link` | production |
| `party_social` | local-path | `../party_social` | test-only |

`partyos` is the manifest's only Git pin. `platform_link` and `party_social`
are local-path dependencies, not floating; `party_social` is excluded from
the production graph by `modes = ["test"]`. `Move.lock` and
`Published.toml` are retained unchanged.

## Verification

- Package tests: **8/8** on both Testnet and Mainnet, including four
  expected-failure paths for wrong-cap absent/present set and clear.
- Strict Testnet and Mainnet lint builds passed with warnings as errors.
- Production instruction coverage: **100.00%** on both Testnet and Mainnet
  (`set_link`, `clear_link`, `has_link`, and `link`).
- Event tests cover exact primitive parent/type/hash/length fields, one-event
  deltas for insert/different/equal replacement/present clear, silence for
  absent/repeated clear and views, distinct X/Instagram streams and parties,
  inert legacy vectors, and the shared-party cap-holder/reader workflow.

## Published metadata

The retained `Published.toml` is the sole record of the prior immutable Testnet package id;
it is not repeated here.
Its dependency inputs predate the pending exact-pin reconciliation. Fresh
immutable publication uses the admin CLI with `--allow-republish`; only after
confirmed success may it replace the target network block.
