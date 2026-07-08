# 05 — Copyright, Authorship & the Commercial/Legal Angle

*Scope: why "author" is a legally loaded word for AI, and why that pushes commercial + OSS projects toward "assisted with" / metadata trailers rather than `Co-authored-by: <AI>`.*

---

## TL;DR

- Under U.S. law, **an "author" must be human**. AI-only output is **not copyrightable**; there is no legal "author" to co-attribute. This is settled at the Copyright Office (2023 guidance, 2025 report) and in court (*Thaler v. Perlmutter*, D.C. Cir. 2025; cert. denied 2026).
- `Co-authored-by: <AI>` therefore asserts a status that **does not legally exist**. It is not itself a legal instrument, so it is not "illegal," but it is **inaccurate provenance** and clashes with the human-authorship framing that CLAs/DCOs depend on.
- Neutral phrasings — **`Assisted-by:`**, **`Generated-by:`**, "generated with," or an internal metadata field — keep the **human as sole author/signer** while honestly recording tool use. The **Linux kernel has now codified exactly this**: AI is tagged with `Assisted-by:` but **may not sign the DCO** — only a human can. (An early July 2025 RFC floated `Co-developed-by:` for AI; that was **dropped in favor of `Assisted-by:`** precisely because `Co-developed-by:` requires a Signed-off-by an AI cannot give.)

---

## 1. The human-authorship rule (why "author" is loaded)

| Source | What it says (verbatim where quoted) | URL |
|---|---|---|
| USCO 2023 Registration Guidance (88 Fed. Reg. 16190) | "the term 'author,' which is used in both the Constitution and the Copyright Act, **excludes non-humans**." Also: "copyright can protect only material that is **the product of human creativity**." | https://www.copyright.gov/ai/ai_policy_guidance.pdf |
| USCO 2023 Guidance — the "assisting instrument" test | Registrability turns on "whether the 'work' is basically one of human authorship, with the computer merely being an assisting instrument, or whether the traditional elements of authorship … were actually **conceived and executed not by man but by a machine**." Case-by-case. | https://www.copyright.gov/ai/ai_policy_guidance.pdf |
| USCO 2023 Guidance — disclosure duty | Applicants have "a **duty to disclose** the inclusion of AI-generated content" and to describe "the human author's contributions." | https://www.copyright.gov/ai/ai_policy_guidance.pdf |
| USCO, *Copyright and AI, Part 2: Copyrightability* (Jan 2025) | "Copyright does not extend to **purely AI-generated material**, or material where there is **insufficient human control** over the expressive elements." "Prompts do not alone provide sufficient control." Using AI "as an assistive tool" does not by itself defeat protection for the human-authored whole. | https://www.copyright.gov/ai/Copyright-and-Artificial-Intelligence-Part-2-Copyrightability-Report.pdf |
| *Thaler v. Perlmutter*, No. 23-5233 (D.C. Cir., decided **Mar 18, 2025**) | Holding (Millett, J.): the Copyright Act "requires all eligible work to be **authored in the first instance by a human being**." A "non-human machine" cannot be an author. | https://media.cadc.uscourts.gov/opinions/docs/2025/03/23-5233.pdf |
| SCOTUS cert. denial (No. 25-449) | Supreme Court **denied certiorari on Mar 2, 2026**, leaving the D.C. Circuit's human-authorship holding standing. Corroborated by multiple firms (Baker Donelson, Holland & Knight, Reed Smith, Finnegan). | Docket: https://www.supremecourt.gov/search.aspx?filename=/docket/docketfiles/html/public/25-449.html ; https://www.mayerbrown.com/en/insights/publications/2026/03/supreme-court-denies-review-in-ai-authorship-case |

**Takeaway:** "author" / "co-author" is a term of art with legal weight (registration, ownership, works-made-for-hire, standing). Applying it to an AI names a legal non-entity. "Assisted," "generated with," and "tool" avoid that — and mirror the Office's own "**assisting instrument**" language.

> Jurisdiction note: the above is **U.S. law**. The UK (CDPA 1988 §9(3)) does recognize a limited "computer-generated works" author = "the person by whom the arrangements necessary for the creation of the work are undertaken" — i.e. still a *person*, never the machine. So even the most AI-friendly major regime does not make the AI the author. *(§9(3) text verified against legislation.gov.uk: "the author shall be taken to be the person by whom the arrangements necessary for the creation of the work are undertaken.")*

---

## 2. Why `Co-authored-by: <AI>` is awkward (not illegal — inaccurate)

- The `Co-authored-by:` git trailer is a **GitHub/git convention for crediting multiple *people***; it drives contribution graphs and requires a **real email tied to an account**. GitHub docs: "attribute a commit to more than one author"; "you must use the email associated with their account." (https://docs.github.com/articles/creating-a-commit-with-multiple-authors)
- It is **metadata, not a legal filing** — it does not itself grant/deny copyright. So "Co-authored-by: Claude" is not unlawful. The problem is **provenance accuracy**: it labels as a co-author something that cannot be an author.
- **Concrete failure mode:** default AI co-author lines use placeholder emails (e.g. `noreply@anthropic.com`); GitHub has **misattributed such commits to whichever human happened to register that address**, corrupting the contribution graph. Documented on the tool trackers: the Claude Code `noreply@anthropic.com` line was linked to an unrelated user (anthropics/claude-code#1653), and OpenAI Codex's default `codex@example.com` silently misattributed commits to a stranger who claimed that address (openai/codex#18095). GitHub's own docs confirm the mechanism ("Why are my commits linked to the wrong user?"). (https://github.com/anthropics/claude-code/issues/1653 ; https://github.com/openai/codex/issues/18095 ; https://docs.github.com/en/pull-requests/committing-changes-to-your-project/troubleshooting-commits/why-are-my-commits-linked-to-the-wrong-user ; commentary: https://fabiorehm.com/blog/2026/03/02/our-coding-agent-commits-deserve-better-than-co-authored-by/)
- The responsibility argument (widely made, e.g. fabiorehm; All Things Open): "The coding agent is a tool and **I am the one responsible** for what it produces." A co-author line muddies who is accountable and who signed the CLA/DCO. (https://fabiorehm.com/blog/2026/03/02/our-coding-agent-commits-deserve-better-than-co-authored-by/ ; https://allthingsopen.org/articles/open-source-ai-contributions-assisted-by-git-trailer-standard)

---

## 3. CLAs / DCOs: who may certify, and why AI can't

The whole contribution-agreement stack assumes a **human legal person** who has rights and can make representations.

| Instrument | Key certification / representation | Implication for AI |
|---|---|---|
| **DCO 1.1** (developercertificate.org) | "the contribution was **created in whole or in part by me** and I have the right to submit it …"; plus (b)/(c) chain-of-provenance; (d) record kept indefinitely with sign-off. | AI has **no rights to grant** and cannot "certify." A machine cannot make the DCO representation. |
| **Apache ICLA v2.2** §5 | "You **represent that each of Your Contributions is Your original creation**." §7 covers non-original / third-party material (mark "Submitted on behalf of a third-party"). "Contribution" = "any **original work of authorship**." | Anchored on *human* original authorship + legal capacity to represent. An AI is not "You." |
| GitHub DCO/CLA convention | `Signed-off-by:` / signature ties the commit to an accountable, emailed identity. | The **human** signs; AI does not. |

**Linux kernel — this is now explicit policy (primary source):**

- `docs.kernel.org/process/coding-assistants.html`: "**AI agents MUST NOT add `Signed-off-by` tags. Only humans can legally certify the Developer Certificate of Origin (DCO).**" The human submitter must review the code, ensure license compliance, **add their own `Signed-off-by`**, and take "full responsibility." AI is credited with `Assisted-by: AGENT_NAME:MODEL_VERSION [TOOL…]`. (The earlier RFC's `Co-developed-by:` for AI was **not adopted**; the merged page uses `Assisted-by:` only.) (https://docs.kernel.org/process/coding-assistants.html)
- `docs.kernel.org/process/submitting-patches.html`: "If you used **any sort of advanced coding tool** in the creation of your patch, you need to acknowledge that use by adding an **`Assisted-by`** tag." Every `Co-developed-by:` must be "immediately followed by a `Signed-off-by:` of the associated co-author" — a requirement an AI cannot satisfy, reinforcing that the AI is a *tool*, not a signing co-author. (https://docs.kernel.org/process/submitting-patches.html)

**So:** the DCO/CLA layer forces the choice. A `Signed-off-by` line **must** be a human. That is precisely why projects push AI into an *`Assisted-by`/tool* slot rather than an *author/signer* slot.

---

## 4. Provenance / supply-chain: honest metadata beats a fake co-author

Software supply-chain frameworks want **accurate, machine-readable provenance** — which is an argument for a dedicated AI-tool field over a human-author line.

| Framework | Relevant point | URL |
|---|---|---|
| **SLSA** (build provenance) | "verifiable information about software artifacts describing **where, when, and how** the artifacts were produced," via in-toto attestations / DSSE. Provenance is about *how it was made*, not *who is a legal author* — a natural home for "made with tool X, model Y." | https://slsa.dev/spec/v1.2/ |
| **SBOM minimum elements** (NTIA 2021 / CISA 2025) | SBOM records **provenance/pedigree and chain of custody**; when producer info is unknown, the author must "**explicitly indicate … unknown provenance**." Ethos = honesty about origin. | https://www.ntia.gov/sites/default/files/publications/sbom_minimum_elements_report_0.pdf ; https://www.cisa.gov/resources-tools/resources/2025-minimum-elements-software-bill-materials-sbom |

Implication *(analysis, consistent with the above sources)*: a stable, parseable trailer like `Assisted-by: <tool>:<model>` (or an internal metadata field) is better provenance than `Co-authored-by: <AI>` — it is honest about *how* the code was produced, is greppable (`git log --grep='Assisted-by:'`), and can feed SBOM/attestation tooling, all **without** asserting a false authorship/ownership claim.

---

## 5. Why a company may prefer "assisted with" or an internal field

Ranked most-sourced → most-analysis:

1. **Clean CLA/DCO chain (sourced).** Only a human can sign; keeping the human as sole author/signer avoids any ambiguity about who made the §5/DCO(a) representation. (kernel docs; DCO; Apache ICLA — above)
2. **No false authorship/ownership signal (sourced).** AI-only output isn't copyrightable and AI can't be an author (§1); a co-author line implies a status that doesn't exist and could be read as conceding "part of this is un-owned / co-owned." Better to state the human authored it *using* a tool.
3. **Accurate provenance for supply-chain/audit (sourced-adjacent).** Dedicated tool/model metadata slots into SLSA/SBOM thinking (§4) and is auditable for internal AI-usage/ROI policy.
4. **Avoids graph/identity corruption (sourced).** Placeholder-email co-authors have been misattributed on GitHub. (§2)
5. **Indemnity / liability posture (analysis — flag).** Firms want a clear human owner accountable for reviewing AI output (relevant to license-compliance and any IP-indemnity commitments to customers). Vendor IP-indemnity programs exist for *using* AI assistants, but that is separate from *how you attribute the commit*; keeping a human as the responsible author aligns cleanly with "I am responsible for what the tool produced." No single primary spec mandates a commit-attribution style for liability — treat as reasoned analysis, not a sourced rule.

---

## 6. Bottom line for wording choices

| Phrase | Legal reading | Verdict |
|---|---|---|
| `Co-authored-by: <AI>` | Names a legal non-entity as author; can corrupt contribution graph; muddies CLA/DCO accountability. | **Avoid** for legal/provenance honesty (still just metadata, not unlawful). |
| `Assisted-by: <tool>:<model>` | Human stays sole author/signer; matches USCO "assisting instrument" and kernel policy. | **Preferred** for OSS with disclosure norms. |
| `Generated-by: <tool>` | Honest for the rare "tool produced ~all of it, minimal human input" case — where the block may **not be copyrightable** at all. | Use narrowly; signals low human authorship. |
| "generated with" / internal metadata field | Same honesty, keeps public author line human; auditable internally. | **Preferred** for commercial/closed source. |

---

## Source quality / confidence

**High confidence — primary / authoritative, quoted directly:**
- USCO 2023 Registration Guidance (copyright.gov PDF; = 88 Fed. Reg. 16190) — "author … excludes non-humans," assisting-instrument test, disclosure duty.
- USCO *Part 2: Copyrightability* report (Jan 2025, copyright.gov PDF) — no copyright for purely AI-generated material; prompts alone insufficient.
- *Thaler v. Perlmutter*, 23-5233 (D.C. Cir., Mar 18 2025), official court PDF — human-authorship-in-the-first-instance holding.
- Developer Certificate of Origin 1.1 (developercertificate.org); Apache ICLA v2.0 §§4–7 (apache.org PDF) — human representation/original-creation requirements.
- Linux kernel `coding-assistants.html` + `submitting-patches.html` — "AI agents MUST NOT add Signed-off-by … only humans can certify the DCO"; `Assisted-by:` is the AI-attribution tag. *(Verified: the page is live/normative under docs.kernel.org. It originated as a July 2025 RFC by Sasha Levin that initially used `Co-developed-by:` for AI; that was changed to `Assisted-by:` before merge — `Co-developed-by:` remains a **human**-only tag because it requires a following Signed-off-by.)*
- GitHub docs on `Co-authored-by:` — convention is for crediting people; needs account email.

**Authoritative, secondary framing (not the legal owner of the claim):**
- SLSA spec v1.2 (slsa.dev) and NTIA/CISA SBOM minimum-elements docs — cited for the *provenance-honesty* norm; the application to AI commit trailers is my analysis, not their text.
- SCOTUS cert denial (Mar 2, 2026, No. 25-449) — date corroborated across five independent law-firm notes (Mayer Brown, Baker Donelson, Holland & Knight, Reed Smith, Finnegan) plus the official SCOTUS docket page; high confidence. (Firm notes are secondary, but the docket URL is the primary record.)

**Analysis / flagged (not a sourced rule):**
- The indemnity/liability rationale (§5.5) — reasoned, aligns with sources, but no single primary source mandates a commit-attribution style for liability.
- UK CDPA §9(3) "computer-generated works" claim (§1 note) — stated from general knowledge; **primary text not fetched this pass**.
- Commentary blogs (fabiorehm, All Things Open) are opinion/reporting, used for the misattribution failure mode and the "tool, not co-author" argument — corroborated by the kernel's normative policy, so directionally reliable.

**Contested / evolving:** The *line* between "AI-assisted (copyrightable human work)" and "AI-generated (not copyrightable)" is explicitly **case-by-case** per USCO and unsettled in litigation; do not overstate any bright rule beyond "AI-only = no copyright, no human author."
