# Contributors / 貢獻者

This project was built and independently reviewed with the help of AI agents from
different vendors, alongside its human maintainer.

本專案由人類維護者,搭配不同廠商的 AI 代理共同開發並獨立審查。

## Maintainer / 維護者
- **AaronFei** — author / maintainer · https://github.com/AaronFei

## AI contributors / AI 貢獻者
- **Claude (Anthropic)** — implementation + adversarial multi-agent security review.
  → [audits/2026-06-13-claude-adversarial.md](audits/2026-06-13-claude-adversarial.md)
- **Grok (xAI)** — independent security review via `grok build` (core crypto, SSV
  split, browser shims, CSP, build reproducibility, dependency audit, offline
  guarantees). → [audits/2026-06-13-grok-review.md](audits/2026-06-13-grok-review.md)

---

> **Why aren't the AI agents in GitHub's "Contributors" sidebar?**
> That sidebar/graph only lists **registered GitHub accounts** mapped to a commit
> author email. Claude and Grok are AI agents with no GitHub account, so they are
> credited here, in the [README](README.md#independent-reviews--contributors--獨立審查與貢獻者),
> in the [`audits/`](audits/) reviews, and in commit `Co-Authored-By:` trailers —
> rather than in that account-based sidebar.
>
> GitHub「Contributors」側欄只列出對應到 commit email 的**已註冊 GitHub 帳號**。
> Claude 與 Grok 是沒有 GitHub 帳號的 AI 代理,因此改以本檔、README、audits/ 審查紀錄
> 與 commit 的 `Co-Authored-By` 標記來標示貢獻。
