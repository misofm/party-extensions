# Release Audit — `party_platform_link`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `main` (matches the repository's current `HEAD`)
**Date:** 2026-09-02
**Toolchain:** `sui 1.78.1-722ac4fcf484`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_platform_link` is the state-attaching bridge for pure platform payloads.
It generically sets/replaces or clears one `PlatformLink<Data>` dynamic field on
a Party, gates writes with the matching `PartyAdminCap`, provides permissionless
optional reads, and emits phantom-typed change events. Clear authenticates
before its absent-state no-op. It adds no payload validation, funds, Vault
borrowing, Action logic, or entry automation.

## Manifest dependencies

| Dependency | Kind | Location | Mode |
|---|---|---|---|
| `partyos` | Git pin | `https://github.com/misofm/partyos.git` @ `819fde6f34c0bc7eeb57ec7340cdf13dc56b3fca` | production |
| `platform_link` | local-path | `../lib/platform_link` | production |
| `party_social` | local-path | `../party_social` | test-only |

`partyos` is the manifest's only Git pin. `platform_link` and `party_social`
are local-path dependencies, not floating; `party_social` is excluded from
the production graph by `modes = ["test"]`.

## Verification

- Package tests: **5/5**, including **1** expected-failure path proving a
  foreign cap cannot exploit absent-state clear.
- Production instruction coverage: **100.00%**.
- End-to-end scenario covers Party share, cap transfer, generic link write,
  and later permissionless read from the shared Party.
- Repository aggregate: **77/77 tests on Testnet and 77/77 on Mainnet**, strict
  lint with warnings as errors; all 11 production modules are at **100.00%**.

## Published metadata

The retained `Published.toml` records prior immutable Testnet package
`0xb1b555e8f02ed32e49fcd13defe0e03755679fd665a5c348f02eb407ebb15b85`.
Its dependency inputs predate the pending exact-pin reconciliation. Fresh
immutable publication uses the admin CLI with `--allow-republish`; only after
confirmed success may it replace the target network block.
