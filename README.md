# `Co-authored-by` vs `Assisted-by`: wording AI attribution in commits

How should a project credit AI in commit metadata — and what wording is right for **open-source** vs **commercial** projects? This repo is a primary-source-cited research report on that question.

## The short version

`Co-authored-by:` asserts **authorship**, and that is exactly the problem for AI. Under U.S. law an author must be human, AI-only output isn't copyrightable, and in DCO/`Signed-off-by` projects co-authorship implies a rights certification an AI legally cannot make. The open-source ecosystem has converged on a **non-authorial** pair of trailers instead — `Assisted-by:` for light help, escalating to `Generated-by:` for substantial AI output — which record tool use honestly while keeping the human as the sole author and signer.

**Bottom line:**

| Context | Use | Avoid |
|---|---|---|
| **OSS — DCO / `Signed-off-by` project** | `Assisted-by: <tool>:<model>` (→ `Generated-by:` for big chunks) **+ a human `Signed-off-by:`** | `Co-authored-by:`/`Co-developed-by:` for AI; any AI `Signed-off-by:` |
| **OSS — casual GitHub project** | `Assisted-by:` or a plain body-note disclosure | reflexive `Co-authored-by:` tool default |
| **Commercial / proprietary** | `Assisted-by:` **or** an internal provenance field; keep the public author line human | `Co-authored-by: <AI>` (muddies CLA / indemnity accountability) |

Two myths the research retires: AI trailers do **not** pollute anyone's contribution graph (they use synthetic emails matching no account), and the popular alternative phrasings (`Partly-generated-with`, `Co-developed-with`, `Written-with`) are valid but have **zero adoption** and don't beat the standard pair. The friction driver is that coding tools (Claude Code, Copilot, Cursor, aider) still *default* to `Co-authored-by:` for AI — all toggleable off.

## Read the full report

- **[REPORT.md](REPORT.md)** — executive summary, full wording-options catalog, OSS-vs-commercial recommendation matrix, and copy-paste commit examples.
- **[sources/](sources/)** — six numbered, independently-cited primary-source notes (git/GitHub trailer mechanics, the DCO & sign-off, AI-tool defaults, real project policies, copyright/legal, and the wording catalog).

Every claim in the report traces to a primary source with a URL.

## Provenance

This report is **AI-generated content**, produced by **Claude Opus 4.8** (`claude-opus-4-8`) via Claude Code under human direction and review. In keeping with its own recommendation, its commits are labelled with a `Generated-by:` trailer rather than `Co-authored-by:`. See the provenance note at the top of [REPORT.md](REPORT.md).
