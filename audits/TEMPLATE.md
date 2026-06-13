# Security review — <reviewer> — <YYYY-MM-DD>

- **Reviewer:** <name / handle / model>
- **Tool / method:** <e.g. grok build, manual review, automated agents>
- **Commit reviewed:** `<full SHA>` (`git rev-parse HEAD`)
- **Scope:** <what you examined>

## Verdict

<one line: e.g. safe-with-caveats / safe / issues-found — plus a sentence>

## What was checked

- [ ] **Crypto correctness** — keys/signatures/deposit_data match the standards (EIP-2333/2334/2335, deposit domain) and the official deposit-cli.
- [ ] **Confidentiality** — no secret (mnemonic / private key / keystore / password) is uploaded, logged, or persisted; only the UI language is stored.
- [ ] **Offline guarantee** — Generate / Top-up / Split run with the network off; WASM is inlined, not fetched.
- [ ] **Network surface** — every network call is 🌐-marked and sends only public data (pubkeys / operator IDs / addresses).
- [ ] **Exfiltration paths** — no hidden requests; CSP `connect-src` allowlist holds even if the bundle were tampered with.
- [ ] **Dependency integrity** — bundle reproduces byte-for-byte from source on the pinned toolchain.
- [ ] **UX footguns** — wrong network / wrong index / index reuse / address typos are guarded or clearly fenced by the official launchpad.

## Findings

| # | Severity | Area | Finding | Status |
|---|---|---|---|---|
| 1 | | | | |

## How I verified (reproduction)

<commands run, what you observed — so others can reproduce your review>
