# `Co-authored-by` vs `Assisted-by`: wording AI attribution in commits

How should a project credit AI in commit metadata — and what wording is right for **open-source** vs **commercial** projects? This repo is a primary-source-cited research report on that question.

## Bottom line

**Drop [`Co-authored-by:`](GLOSSARY.md#co-authored-by "GitHub convention crediting extra commit authors; it asserts authorship.") for AI.** Use [`Assisted-by: <tool>:<model>`](GLOSSARY.md#assisted-by "Non-authorial trailer crediting an AI as a helper, while the human stays sole author.") for open-source (escalate to [`Generated-by:`](GLOSSARY.md#generated-by "Provenance trailer noting AI produced substantial or whole parts of the change.") when the AI wrote substantial chunks), keep a human [`Signed-off-by:`](GLOSSARY.md#signed-off-by "Commit trailer that, in DCO projects, certifies you may submit the code — a human legal attestation.") where the project uses the [DCO](GLOSSARY.md#dco "Developer Certificate of Origin — a per-commit sign-off certifying you have the right to submit the code."), and for commercial/proprietary code prefer `Assisted-by:` or an internal [provenance](GLOSSARY.md#software-provenance "Software provenance — where and how software was produced, recorded in build attestations and SBOMs.") field so the public author line stays human. Always follow the specific project's policy if it has one — a few projects (Kubernetes) ban trailers entirely and want prose disclosure instead.

| Context | Use | Avoid |
|---|---|---|
| **OSS — DCO / `Signed-off-by` project** | `Assisted-by: <tool>:<model>` (→ `Generated-by:` for big chunks) **+ a human `Signed-off-by:`** | `Co-authored-by:`/[`Co-developed-by:`](GLOSSARY.md#co-developed-by "Kernel trailer for shared authorship; must be paired with that person's Signed-off-by.") for AI; any AI `Signed-off-by:` |
| **OSS — casual GitHub project** | `Assisted-by:` or a plain body-note disclosure | reflexive `Co-authored-by:` tool default |
| **Commercial / proprietary** | `Assisted-by:` **or** an internal provenance field; keep the public author line human | `Co-authored-by: <AI>` (muddies [CLA](GLOSSARY.md#cla "Contributor License Agreement — a signed grant of rights over your contribution.") / [indemnity](GLOSSARY.md#ip-indemnity "IP indemnity — a supplier's promise to cover intellectual-property claims over the code.") accountability) |

## The gist

`Co-authored-by:` asserts **authorship**, and that is exactly the problem for AI. Under U.S. law an author must be human, AI-only output isn't copyrightable, and in DCO/`Signed-off-by` projects co-authorship implies a rights certification an AI legally cannot make. The open-source ecosystem has converged on a **non-authorial** pair of trailers instead — `Assisted-by:` for light help, escalating to `Generated-by:` for substantial AI output — which record tool use honestly while keeping the human as the sole author and signer.

Two myths the research retires: AI trailers do **not** pollute anyone's contribution graph (they use synthetic emails matching no account), and the popular alternative phrasings (`Partly-generated-with`, `Co-developed-with`, `Written-with`) are valid but have **zero adoption** and don't beat the standard pair. The friction driver is that coding tools (Claude Code, Copilot, Cursor, aider) still *default* to `Co-authored-by:` for AI — all toggleable off.

## Read the full report

- **[REPORT.md](REPORT.md)** — executive summary, full wording-options catalog, OSS-vs-commercial recommendation matrix, and copy-paste commit examples.
- **[GLOSSARY.md](GLOSSARY.md)** — plain-language definitions of the less-common terms (DCO, CLA, SLSA, kernel trailers, the case law).
- **[sources/](sources/)** — six numbered, independently-cited primary-source notes (git/GitHub trailer mechanics, the DCO & sign-off, AI-tool defaults, real project policies, copyright/legal, and the wording catalog).

Every claim in the report traces to a primary source with a URL.

## Provenance

This report is **AI-generated content**: ordered by the repository owner and executed by an automated Claude research workflow — **Claude Opus 4.8** (`claude-opus-4-8`) via Claude Code — under human direction and review. Published 2026-07-08 12:05 UTC+00:00. In keeping with its own recommendation, its commits are labeled with a `Generated-by:` trailer rather than `Co-authored-by:`. See the full [Provenance](REPORT.md#provenance) note in the report.
