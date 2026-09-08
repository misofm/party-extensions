# Release Audit — `party_social`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `main` (matches the repository's current `HEAD`)
**Date:** 2026-09-02
**Toolchain:** `sui 1.78.1-722ac4fcf484`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_social` is a pure payload package, not a state-attaching extension. It
defines ten social-platform payload types and constructors. A shared private
validator enforces nonempty handles no longer than 256 bytes. Platform-specific
format and URL reconstruction remain client policy. The package has no Party,
cap, storage, event, or automation logic; `party_platform_link` attaches its
returned values to Party state.

## Manifest dependencies

| Dependency | Kind | Location |
|---|---|---|
| `platform_link` | local-path | `../lib/platform_link` |

The manifest's only dependency is the local-path `platform_link` sibling
above (`platform_link = { local = "../lib/platform_link" }`); it carries no
Git pin and no floating dependency.

## Verification

- Package tests: **5/5**, including **3** expected-failure paths for empty X,
  empty Instagram, and the shared overlength validator.
- All ten constructors and accessors execute on success.
- Production instruction coverage: **100.00%**.
- Shared-Party workflow: not applicable; this package is pure payload code.
- Repository aggregate: **77/77 tests on Testnet and 77/77 on Mainnet**, strict
  lint with warnings as errors; all 11 production modules are at **100.00%**.

## Published metadata

The retained `Published.toml` records prior immutable Testnet package
`0x23e8eebf229d4284297964f3b9ceab1574da515260afad22da7a113d14fec952`.
Its bytecode predates the pending validator refactor even though public behavior
and ABI are preserved. Fresh immutable publication uses the admin CLI with
`--allow-republish`; only after confirmed success may it replace the target
network block.
