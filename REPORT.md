# Wording AI Attribution in Commits: `Co-authored-by` vs `Assisted-by` and the Alternatives

Ordered by the repository owner ([@heios](https://github.com/heios)); researched and written by an automated Claude research workflow. Published 2026-07-08 12:05 UTC+00:00.

> **How to read this.** The recommendations come first. For how far to trust each claim, see [Source confidence](#source-confidence); for the cited primary-source notes behind every claim, see [Supporting documents](#supporting-documents); for who made this and how, see [Provenance](#provenance) — all three are at the end. Uncommon terms (DCO, CLA, SLSA, …) are defined in the [glossary](GLOSSARY.md).

---

## 1. Executive summary

[`Co-authored-by:`](GLOSSARY.md#co-authored-by "GitHub convention crediting extra commit authors; it asserts authorship. See glossary.") asserts **authorship**, and that is exactly the problem for AI. Under U.S. law an author must be human — AI-only output is not copyrightable and there is no legal "author" to co-attribute ([USCO](GLOSSARY.md#usco "U.S. Copyright Office. See glossary.") 2023 guidance; [*Thaler v. Perlmutter*](GLOSSARY.md#thaler-v-perlmutter "Court ruling that a copyrightable work must be authored by a human. See glossary."), [cert. denied](GLOSSARY.md#certiorari "The Supreme Court's discretionary review; 'cert. denied' leaves the lower ruling standing. See glossary.") Mar 2026) — so `Co-authored-by: <AI>` names a status that does not legally exist [[05]](sources/05-copyright-and-legal.md). In [DCO](GLOSSARY.md#dco "Developer Certificate of Origin — a per-commit sign-off certifying you have the right to submit the code. See glossary.")/[`Signed-off-by`](GLOSSARY.md#signed-off-by "Commit trailer that, in DCO projects, certifies you may submit the code — a human legal attestation. See glossary.") projects it is worse: co-authorship there carries a *certification of rights* that only an accountable human can make, which is why the Linux kernel's merged policy tags AI with **[`Assisted-by:`](GLOSSARY.md#assisted-by "Non-authorial trailer crediting an AI as a helper, while the human stays sole author. See glossary.")** and rules that "AI agents MUST NOT add Signed-off-by tags" [[02]](sources/02-dco-signoff-and-authorship.md)[[04]](sources/04-project-policies-and-controversy.md). Neutral framings — `Assisted-by:` (light help) and [`Generated-by:`](GLOSSARY.md#generated-by "Provenance trailer noting AI produced substantial or whole parts of the change. See glossary.") (substantial output) — record tool use honestly while keeping the human as sole author/signer, and they are the convergent open-source standard (kernel, Apache, OpenInfra, Mesa, QGIS) [[06]](sources/06-wording-options-catalog.md). Crucially, none of this is about literal contribution-graph pollution: every AI trailer uses a synthetic email that matches no account, so no phrasing inflates any human's green squares — the objection is authorship implication and log/accountability noise, not graph credit [[01]](sources/01-coauthored-by-trailer-mechanics.md).

**Bottom line: drop `Co-authored-by:` for AI. Use `Assisted-by: <tool>:<model>` for open-source (escalate to `Generated-by:` when the AI wrote substantial chunks), keep a human `Signed-off-by:` where the project uses the DCO, and for commercial/proprietary code prefer `Assisted-by:` or an internal [provenance](GLOSSARY.md#software-provenance "Where and how software was produced, recorded in build attestations and SBOMs. See glossary.") field so the public author line stays human.** Always follow the specific project's policy if it has one — a few projects (Kubernetes) ban trailers entirely and want prose disclosure instead.

---

## 2. Why "co-author" is contested

Three independent lines converge on the same conclusion — the AI is a *tool*, not an *author* — and each is anchored in primary text:

- **Copyright law (US).** The USCO states the term "author" "excludes non-humans" and that applicants "should not list an AI technology … as an author or co-author"; *Thaler* (D.C. Cir. 2025, cert. denied 2 Mar 2026) holds work must be "authored in the first instance by a human being." AI-only output is not copyrightable. So `Co-authored-by: <AI>` is *inaccurate provenance* — not unlawful (it's just metadata), but it labels a legal non-entity as a co-author [[05]](sources/05-copyright-and-legal.md).
- **The DCO / sign-off.** GitHub's `Co-authored-by:` carries **no** legal assertion — it's bare credit. But the kernel/git `Signed-off-by:` **is** the act of certifying the Developer Certificate of Origin: "I certify that … I have the right to submit it." Kernel co-authorship ([`Co-developed-by:`](GLOSSARY.md#co-developed-by "Kernel trailer for shared authorship; must be paired with that person's Signed-off-by. See glossary.")) must be "immediately followed by a `Signed-off-by:` of the associated co-author." An AI holds no rights, is not a contactable/accountable identity, and cannot certify — so it cannot sign off and cannot be a certifying co-author [[02]](sources/02-dco-signoff-and-authorship.md). The [DCO GitHub App](GLOSSARY.md#dco-app "GitHub App that fails a pull request unless every commit has a valid Signed-off-by. See glossary.") mechanically enforces a valid `Signed-off-by:` on every commit, tied to the commit author's email [[02]](sources/02-dco-signoff-and-authorship.md).
- **Project policy.** The ecosystem has split. **Ban** projects (QEMU, Gentoo, NetBSD) refuse AI content outright — QEMU's rationale is explicitly the DCO: "how contributors could comply with DCO terms (b) or (c) … is unclear" [[04]](sources/04-project-policies-and-controversy.md). **Disclose** projects (kernel, Apache, OpenInfra, curl) allow AI with a *non-authorial* trailer precisely because `Co-authored-by:`/`Co-developed-by:` imply an author who can sign off. Several projects ban `Co-authored-by:` for AI by name (Kubernetes, Mesa, attrs, pip, Requests) [[06]](sources/06-wording-options-catalog.md). A cross-sectional study of 1,000 repos found 78% allow GenAI contributions, 51% require disclosure, 74% mandate human oversight — allow-with-disclosure dominates [[04]](sources/04-project-policies-and-controversy.md).

The tension in one line: the *coding tools* (Claude Code, Copilot, Cursor, aider) still **default** to `Co-authored-by:`, which is exactly the wording the copyright office warns against and DCO projects increasingly ban — a tooling default, not a considered standard [[06]](sources/06-wording-options-catalog.md).

---

## 3. Wording-options catalog

Two facts govern every row: **(a)** any hyphenated `Token: value` line is an equally valid, greppable [git trailer](GLOSSARY.md#git-trailer "Git's generic 'Token: value' metadata lines at the end of a commit message. See glossary.") (tokens may contain hyphens, not spaces) — git assigns none of them special meaning; **(b)** the GitHub contribution-graph effect is identical for all of them and driven only by email, so no phrasing inflates a human's graph, and `Co-authored-by:` is the *only* token GitHub renders as a linked co-author chip [[01]](sources/01-coauthored-by-trailer-mechanics.md)[[06]](sources/06-wording-options-catalog.md).

| Phrasing | Parseable trailer? | GitHub co-author chip? | Connotation | DCO/legal safety | Best-fit context |
|---|---|---|---|---|---|
| **`Co-authored-by:`** (tool default) | Yes | **Yes** (only token that does) | Strongest — asserts *equal authorship*; the thing USCO warns against | **Weakest** — reads as an authorship claim; banned for AI by several projects. No forged sign-off, but the implication is the problem | Human↔human collaboration; solo dev who wants zero-config and doesn't care. **Avoid for AI in DCO/[CLA](GLOSSARY.md#cla "Contributor License Agreement — a signed grant of rights over your contribution. See glossary.") projects** |
| **`Assisted-by:`** | Yes | No | "Mere tool / helper"; mirrors [`Acked-by`](GLOSSARY.md#kernel-credit-trailers "Kernel credit/review trailers: Acked-by, Reviewed-by, Tested-by, Reported-by, Suggested-by. See glossary.")/`Reviewed-by` | **Strong** — designed to sit beside a human `Signed-off-by`; USCO-safe. Kernel/Apache/OpenInfra/Mesa/QGIS standard | **Default for AI-permitting OSS** (light help). Machine-auditable, foundation-blessed |
| **`Generated-by:`** | Yes | No | "Machine-produced / provenance"; stronger AI signal than `Assisted-by` but not an author claim | **Strong** — provenance label; Apache "recommends" it; OpenInfra uses it for *substantial* AI output; arguably closest to USCO disclosure duty | OSS where AI produced substantial/whole chunks; [SBOM](GLOSSARY.md#sbom "Software Bill of Materials — an inventory of a build's components. See glossary.")/provenance tooling |
| **`Co-developed-by:`** | Yes | No | Near-`Co-authored-by` — equal development credit; kernel *rejected* this for AI | **Weak** — kernel semantics *require* a matching `Signed-off-by` the AI can't give; Kubernetes bans it by name | Human co-developers only. **Not recommended for AI** |
| **`AI-Assisted:` / `AI-assistant:`** | Yes | No | "Tool involved"; neutral | Safe (human signs off) | Proposal-only; loses interop vs `Assisted-by` |
| **`Aided-by:`** | Yes | No | "Helper" — synonym of `Assisted-by` | Safe | Proposal-only; strictly worse for interop |
| **`Generated-with:` / `Generated-using:`** | Yes | No | "Produced via this tool"; provenance-flavored | Safe | Proposal/ad-hoc; prefer `Generated-by` |
| **`Partly-generated-with:`** *(user suggestion)* | Yes | No | "Partially machine-produced" — precise but verbose | Safe | Proposal-only; verbosity buys little over `Generated-by`/`Assisted-by` |
| **`Co-developed-with:`** *(user suggestion)* | Yes | No | "Co-development" — same over-crediting as `Co-developed-by` | Medium-weak (authorship-adjacent) | Avoid |
| **`Written-with:`** *(user suggestion)* | Yes | No | Authored-jointly-with-a-tool; softer than `Co-authored-by` but still authorship-adjacent | Medium — leans toward authorship framing | Avoid for DCO/CLA projects |
| **`Prompted-by:`** | Yes | No | "Prompt-driven"; ambiguous subject (human prompter vs AI) | Safe (no authorship claim) | Niche; ambiguity hurts it |
| **`Tool:` / `Model:`** | Yes | No | Pure operational metadata; no credit at all | Safe | Machine/provenance pipelines; pair with `Assisted-by` for human-readable credit |
| **Plain body-text note** (e.g. "This PR was written in part with generative AI"; Claude Code's `🤖 Generated with Claude Code`) | **No** (not a `token:` line) | No | Fully flexible; zero authorship implication | **Safe** but **not machine-auditable** (can't reliably `git log --grep` a key) | Projects that reject trailers (Kubernetes) or want human-readable disclosure only |

*Adoption snapshot [[06]](sources/06-wording-options-catalog.md): prescribe `Assisted-by`/`Generated-by` — kernel, Apache, OpenInfra, Mesa, QGIS, IREE, STAC. Ban `Co-authored-by:` for AI — Kubernetes (bans all trailers; PR-body prose only), Mesa, attrs, pip, Requests, LinkML. Recommend `Co-authored-by:` — Pytest, IREE (either allowed). QEMU/Gentoo/NetBSD ban AI content outright, so no attribution applies.*

---

## 4. Recommendation matrix

Opinionated; one primary recommendation per row. This section is **analysis** synthesized from the sourced facts above.

| Context | Recommended wording | Rationale | What to avoid |
|---|---|---|---|
| **Open-source — DCO / `Signed-off-by` project** (kernel-style; DCO App enforced) | **`Assisted-by: <tool>:<model>`** for light help, **`Generated-by: <tool>`** when AI wrote substantial chunks — *always* with your own human `Signed-off-by:` | Matches the kernel's merged, foundation-blessed standard; keeps the human as sole certifier of the DCO; USCO-safe; greppable/auditable. AI must never add a `Signed-off-by:` [[02]](sources/02-dco-signoff-and-authorship.md)[[04]](sources/04-project-policies-and-controversy.md)[[06]](sources/06-wording-options-catalog.md) | `Co-authored-by:`/`Co-developed-by:` for AI (implies an author who must sign off but can't); any AI `Signed-off-by:` line; blank line missing before the trailer block |
| **Open-source — casual GitHub project** (no DCO/CLA, informal norms) | **`Assisted-by: <tool>:<model>`** (or a plain body-text disclosure if you prefer prose) | Honest provenance, no false authorship claim, interoperable with the emerging OSS standard, doesn't imply the AI earned a co-author chip. Follow the repo's own policy first if it has one | `Co-authored-by:` for AI as a reflex tool default; it renders a co-author chip and reads as equal authorship. (Harmless to the graph, but misleading) |
| **Commercial / proprietary** | **`Assisted-by:`** in the commit **or** an internal provenance/metadata field; keep the *public* author line human | Keeps a single accountable human author for CLA/[indemnity](GLOSSARY.md#ip-indemnity "A supplier's promise to cover intellectual-property claims over the code. See glossary.")/review posture; AI-only output isn't copyrightable so don't imply co-ownership; feeds internal AI-usage audit/SBOM tooling. Use `Generated-by:` narrowly when a block is essentially all-AI [[05]](sources/05-copyright-and-legal.md) | `Co-authored-by: <AI>` (muddies who is accountable/who signed the CLA; can misattribute via placeholder email); asserting AI co-authorship in anything customer-facing |

---

## 5. What I'd actually put in a commit

**Good default trailer (OSS, kernel-style — light AI help, human certifies):**

```
Fix off-by-one in ring-buffer wraparound

The index wrapped one element early under load; clamp to capacity.

Assisted-by: Claude:claude-opus-4 coccinelle
Signed-off-by: Alex Kh <alex@example.org>
```

**Body-note alternative (no structured trailer — for a project that bans trailers, e.g. Kubernetes):**

```
Add retry/backoff to the upload client

Retries transient 5xx responses with exponential backoff and jitter.

This change was written in part with the assistance of generative AI
(Claude); all code was reviewed by me and I take responsibility for it.
```

**What NOT to do (asserts authorship an AI can't hold; avoid for AI):**

```
Co-authored-by: Claude <noreply@anthropic.com>   # implies co-authorship
Signed-off-by: Claude <noreply@anthropic.com>    # an AI cannot certify the DCO
```

**Toggling tool defaults** (so your tool stops emitting `Co-authored-by:` for you): Claude Code — set `attribution.commit`/`attribution.pr` to `""` in `settings.json` (legacy `includeCoAuthoredBy: false`, deprecated); aider — `--no-attribute-co-authored-by` (plus `--no-attribute-author`/`--no-attribute-committer`, or `AIDER_ATTRIBUTE_*=false`); VS Code Copilot — `git.addAICoAuthor: "off"`; Cursor — Settings → Agent → Attribution (v3.11+: Git & PRs → Attribution) [[03]](sources/03-ai-tool-default-attribution.md). (Note: Claude Code's setting has been reported as intermittently not respected — verify the actual commit output.)

---

## Supporting documents

Six numbered, independently-cited primary-source notes sit behind this synthesis. Every claim above traces to one of them, and each note ends with its own source-quality caveats.

| Note | Description |
|---|---|
| [sources/01-coauthored-by-trailer-mechanics.md](sources/01-coauthored-by-trailer-mechanics.md) | What the `Co-authored-by:` trailer actually is — git trailer machinery, GitHub's email-matching, contribution-graph behavior, and cross-forge rendering (GitLab/Bitbucket). |
| [sources/02-dco-signoff-and-authorship.md](sources/02-dco-signoff-and-authorship.md) | The DCO text, what `Signed-off-by:` legally certifies, the kernel's `Co-developed-by:`+sign-off rule, and why an AI cannot certify. |
| [sources/03-ai-tool-default-attribution.md](sources/03-ai-tool-default-attribution.md) | What each major AI coding tool emits by default and how to turn it off (Claude Code, Copilot, aider, Cursor, Jules, Codex, etc.). |
| [sources/04-project-policies-and-controversy.md](sources/04-project-policies-and-controversy.md) | Real project policies — kernel `Assisted-by:`, QEMU/Gentoo/NetBSD bans, curl's honest-disclosure line — and the ban-vs-disclose split. |
| [sources/05-copyright-and-legal.md](sources/05-copyright-and-legal.md) | Copyright/authorship law (USCO, *Thaler*), CLAs/DCOs, provenance frameworks ([SLSA](GLOSSARY.md#slsa "Supply-chain Levels for Software Artifacts — a build-provenance framework. See glossary.")/SBOM), and the commercial angle. |
| [sources/06-wording-options-catalog.md](sources/06-wording-options-catalog.md) | The full catalog of phrasings — real-world and proposal-only — scored on parseability, connotation, legal safety, GitHub effect, and adoption. |

## Source confidence

The load-bearing facts rest on *primary, public* sources: git's reference manual (`git-scm.com`), GitHub's own docs, the canonical DCO (`developercertificate.org`), Linux-kernel process docs, the U.S. Copyright Office guidance/reports, *Thaler v. Perlmutter*, and each AI tool's own settings docs/source. Where a statement is *reasoned inference* rather than a direct quote — chiefly "an AI cannot sign off / cannot be a legal author" (a conclusion built from the DCO + copyright text) and the recommendation matrix itself — it is called out as analysis. Two areas are explicitly softer: (a) exact live default strings emitted by some tools (Jules, the Copilot cloud agent, aider's email field) are medium-confidence because vendors don't publish the literal string; (b) some project-policy rows (Mesa, attrs, pip, Requests, Pytest) are relayed via a community catalog, not read first-hand. All sampled claims were cross-checked against their cited primary sources; none were found unsupported.

## Provenance

- **Ordered by:** the repository owner ([@heios](https://github.com/heios)) — who set the question, scope, and structure, and reviewed the result.
- **Executed by:** an automated research workflow running in Claude Code (an orchestrating model plus research subagents).
- **Model(s):** Claude Opus 4.8 (`claude-opus-4-8`) throughout — orchestrator and subagents alike.
- **Tooling:** web search and retrieval of primary sources; each claim was cross-checked against its cited original.
- **Human role:** direction, scope, and review. This is **AI-generated content with human oversight** — not a human-authored document.
- **Published:** 2026-07-08 12:05 UTC+00:00.

This report is published deliberately as a working example of the practice it recommends: its own commits carry a `Generated-by: Claude Opus 4.8 (claude-opus-4-8)` trailer rather than a `Co-authored-by:` line — because the report is substantially machine-generated, and no AI can be a legal author or a sign-off party. See [§5, What I'd actually put in a commit](#5-what-id-actually-put-in-a-commit) and the [glossary](GLOSSARY.md).
