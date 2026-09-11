# Release Audit — `party_cta`

**Repository:** `https://github.com/misofm/partyos-extensions`
**Audit target:** pending working-tree source on `main` (matches the repository's current `HEAD`)
**Date:** 2026-09-02
**Toolchain:** `sui 1.78.1-722ac4fcf484`

## Verdict

Release-ready. No security or correctness findings remain.

## Implementation reviewed

`party_cta` is a state-attaching extension. It stores one ordered,
replace-whole `vector<Cta>` dynamic field on a Party. Labels are 1–60 bytes,
URLs are 1–2000 bytes, and the list is capped at 20. `set_ctas` and
`clear_ctas` require the matching `PartyAdminCap`; views are permissionless.
Set and clear events include primitive party/capability addresses plus complete
ordered prior/resulting CTA bytes, and an absent clear is silent. The module
contains no funds or automation entrypoints.

## Exact manifest pins

| Dependency | Repository | Revision |
|---|---|---|
| `partyos` | `https://github.com/misofm/partyos.git` | `841a875a4989082a0ebeb1beb464b71f9ea2bd73` |

The manifest has no local-path or floating dependencies.

## Verification

- Package tests: **16/16**, including expected-failure paths covering all four
  CTA validators, list capacity ordering, and wrong-cap set/replace/clear
  authorization.
- Production instruction coverage: **100.00%**.
- End-to-end scenario: Party creation and share, cap transfer, later cap-gated
  write to the shared Party, and permissionless read in another transaction.
- Repository aggregate: **77/77 tests on Testnet and 77/77 on Mainnet**, strict
  lint with warnings as errors; all 11 production modules are at **100.00%**.
- Fresh unpublished copies build strictly for both networks.

## Published metadata

The retained `Published.toml` is the sole record of the prior immutable Testnet package id;
it is not repeated here.
It predates pending source wherever inputs differ. Fresh immutable publication
uses the admin CLI with `--allow-republish`; only a confirmed successful
transaction may replace the target network block.
