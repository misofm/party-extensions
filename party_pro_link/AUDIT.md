# Release Audit — `party_pro_link`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `main` (matches the repository's current `HEAD`)
**Date:** 2026-09-02
**Toolchain:** `sui 1.78.1-722ac4fcf484`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_pro_link` is a pure payload package, not a state-attaching extension. It
defines six URL payloads and three handle/subdomain payloads. Shared private
validators enforce nonempty values, URL length at most 2000 bytes, and handle
length at most 256 bytes. The package has no Party, cap, storage, event, or
automation logic; `party_platform_link` attaches its returned values.

## Manifest dependencies

| Dependency | Kind | Location |
|---|---|---|
| `platform_link` | local-path | `../lib/platform_link` |

The manifest's only dependency is the local-path `platform_link` sibling
above (`platform_link = { local = "../lib/platform_link" }`); it carries no
Git pin and no floating dependency.

## Verification

- Package tests: **6/6**, including **4** expected-failure paths covering
  empty URL, empty handle, overlength URL, and overlength handle.
- All nine constructors and their nine accessors execute successfully, including
  `management_page` and `publisher_page`.
- Production instruction coverage: **100.00%**.
- Shared-Party workflow: not applicable; this package is pure payload code.
- Repository aggregate: **77/77 tests on Testnet and 77/77 on Mainnet**, strict
  lint with warnings as errors; all 11 production modules are at **100.00%**.

## Published metadata

The retained `Published.toml` is the sole record of the prior immutable Testnet package id;
it is not repeated here.
Its bytecode predates the pending validator refactor even though public behavior
and ABI are preserved. Fresh immutable publication uses the admin CLI with
`--allow-republish`; only after confirmed success may it replace the target
network block.
