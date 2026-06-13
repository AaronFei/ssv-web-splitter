# Security review — Claude (Anthropic) — 2026-06-13

- **Reviewer:** Claude (Anthropic), Opus 4.x
- **Tool / method:** Claude Code — multi-agent adversarial workflow (~48 agents): finders fanned out across attack surfaces, each candidate finding then independently verified/refuted by separate skeptic agents before being kept.
- **Commit reviewed:** core cryptographic + flow logic as of `9e2433d` (the security-hardening commit that implemented this review's recommendations). The crypto core (`src/generate.ts`, `src/keystore.ts`, the SSV split path) is unchanged in later commits, which only add UI / i18n / links / license.
- **Scope:** mnemonic generation & BLS derivation, EIP-2335 keystore encrypt/decrypt, deposit_data construction & signing, the SSV keyshare split, all three tabs' data flow, the network surface, and the deployed bundle.

## Verdict

**safe-with-caveats** — the core confidentiality claim holds: secrets never leave the tab; all key handling is offline-capable; the crypto is byte-identical to the official deposit-cli; the bundle is reproducible. Residual risk is operational (deploy-trust on online page load, UX fund-footguns), not a leak in the code.

## What was checked

- [x] **Crypto correctness** — pubkey / signature / deposit_message_root / deposit_data_root are **byte-identical to ethstaker-deposit-cli** across 0x01, 0x02 (compounding), non-integer amounts, and top-up. EIP-2335 keystore interoperates with deposit-cli in both directions. CSPRNG (`crypto.getRandomValues`) on every secret path; no `Math.random`.
- [x] **Confidentiality** — mnemonic / private key / keystore / password are processed only in-tab; never uploaded, logged, or written to storage. The only persisted value is the UI language.
- [x] **Offline guarantee** — Generate / Top-up / Split issue zero network requests; the BLS WASM is inlined as base64, so they run with the network physically off (verified via `file://`).
- [x] **Network surface** — exactly three optional, 🌐-marked calls, each sending only public data: detect-index (derived pubkeys), fetch operator keys (operator ID), connect wallet (public address, read-only).
- [x] **Exfiltration paths** — no hidden requests found. Post-review hardening added a CSP `connect-src` allowlist so even a tampered bundle cannot POST secrets to an attacker origin.
- [x] **Dependency integrity** — bundle reproduces byte-for-byte from source on the pinned toolchain (`.nvmrc` + `packageManager`); CI fails if `docs/` drifts from a fresh build.
- [x] **UX footguns** — flagged: wrong top-up index, index reuse, wrong network. Mitigated by EIP-55 address checksumming, a network echo in success messages, and the discipline of validating via the official launchpad before any ETH moves.

## Findings

| # | Severity | Area | Finding | Status |
|---|---|---|---|---|
| 1 | Medium | Deployment | On **online** page load, the served bundle cannot be self-verified against source at load time (deploy-trust root). | Mitigated: CSP blocks exfiltration; recommend offline (`file://`) use for real keys + 2FA on the repo. |
| 2 | Medium | UX | Wrong top-up index / index reuse / wrong network can misdirect or penalize funds. | Mitigated: validation + official-launchpad backstop; documented in README/UI. |
| 3 | Low | Privacy | The mnemonic is shown on screen for backup. | Documented: generate offline, don't screen-share, close the tab after use. |

## How I verified (reproduction)

- `pnpm test` — offline self-test, all fixed vectors pass.
- `pnpm verify:cli` — diff vs ethstaker-deposit-cli (byte-identical).
- Headless runs of Generate (0x01 & 0x02 incl. decimals), Top-up, Split, Detect — DevTools/Network confirmed zero requests on the core paths; only the 🌐 buttons reach out, with public-only payloads.
- `pnpm build` reproduces the deployed `docs/assets/index-*.js`.
