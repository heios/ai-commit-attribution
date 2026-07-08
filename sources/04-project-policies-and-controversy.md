# Project Policies & the AI-Attribution Controversy

*Focus: how real open-source projects treat AI-generated contributions and AI-as-author, drawn from primary policy files, mailing-list posts, and maintainer blogs. Which projects **ban** AI, which **require disclosure**, and what that implies for `Co-authored-by:` vs `Assisted-by:` wording.*

Research date: 2026-07-08.

> **Sourcing note.** Every claim below is tied to a primary source: the actual policy document, the kernel doc in-tree, the QEMU `.rst`, the Gentoo wiki, the NetBSD guidelines page, or Daniel Stenberg's own blog. Secondary tech-press pieces are used only to locate primaries, never as the cited authority. Confidence is flagged per row; contested/evolving items are marked.

---

## Summary table

| Project | Stance on AI-generated contributions | Stance on AI as co-author / how to attribute | Primary source | Date |
|---|---|---|---|---|
| **Linux kernel** | **Allowed** with human in the loop; AI is "the new normal" | **`Assisted-by:` trailer**, NOT `Co-authored-by:`/`Co-developed-by:`. AI **must not** add `Signed-off-by:` (only a human certifies the DCO). | [coding-assistants.rst (in-tree)](https://github.com/torvalds/linux/blob/master/Documentation/process/coding-assistants.rst) | merged early Jan 2026 |
| **QEMU** | **Banned by default** — declines any contribution believed to derive from AI-generated content | No AI attribution; the problem is that AI output can't satisfy the DCO at all. Narrow exceptions under active debate (2026). | [Code provenance doc](https://www.qemu.org/docs/master/devel/code-provenance.html) · [`.rst`](https://github.com/qemu/qemu/blob/master/docs/devel/code-provenance.rst) | policy standing; relax-patch May 2026 |
| **Gentoo** | **Banned** — "expressly forbidden" to contribute NLP-AI-created content | N/A — content is refused outright, not attributed | [Project:Council/AI policy](https://wiki.gentoo.org/wiki/Project:Council/AI_policy) | Council vote 2024-04-14 |
| **NetBSD** | **Presumed "tainted"** — LLM-generated code must not be committed without prior written core approval | N/A — treated as a provenance/licensing hazard, not an authorship question | [Commit Guidelines §2](https://www.netbsd.org/developers/commit-guidelines.html) | updated May 2024 |
| **curl** (project + bug bounty) | AI assistance **tolerated if verified & disclosed**; unverified "AI slop" security reports **banned + publicly ridiculed** | Focus is on report provenance/honesty, not commit co-authorship | [Stenberg blog](https://daniel.haxx.se/blog/2025/07/14/death-by-a-thousand-slops/) · [bug-bounty end](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/) | 2025–2026 |

---

## 1. Linux kernel — the "disclose, don't ban" model (`Assisted-by:`)

The kernel merged an official in-tree doc, `Documentation/process/coding-assistants.rst` (a ~59-line reST file merged early January 2026, growing out of Sasha Levin's mid-2025 RFC). It is the most consequential primary source for this report because it **explicitly chooses `Assisted-by:` over `Co-authored-by:`/`Co-developed-by:`.**

Verbatim rules ([in-tree source](https://github.com/torvalds/linux/blob/master/Documentation/process/coding-assistants.rst)):

- **AI cannot certify the DCO.** *"AI agents MUST NOT add Signed-off-by tags. Only humans can legally certify the Developer Certificate of Origin (DCO)."*
- **Attribution trailer format:** *"Assisted-by: AGENT_NAME:MODEL_VERSION [TOOL1] [TOOL2]"* — with the concrete example *"Assisted-by: Claude:claude-3-opus coccinelle sparse"*. Basic tools (git, gcc, make, editors) are not listed; specialized analysis tools (coccinelle, sparse, smatch, clang-tidy) are.
- **Human responsibility list:** the submitter is responsible for *"Reviewing all AI-generated code; Ensuring compliance with licensing requirements; Adding their own Signed-off-by tag to certify the DCO; Taking full responsibility for the contribution."*

**Why this matters for wording.** The kernel deliberately does **not** treat the AI as a co-author. `Co-developed-by:` in kernel convention *must* be immediately followed by that co-developer's own `Signed-off-by:` (it denotes co-authorship carrying DCO weight — see the [Submitting Patches guide](https://www.kernel.org/doc/html/latest/process/submitting-patches.html)). An AI cannot supply a `Signed-off-by:`, so it cannot be a `Co-developed-by:`. `Assisted-by:` is a non-authorial credit/tracking tag that sidesteps the DCO problem. This is the sharpest primary-source data point in the whole "Co-authored-by vs Assisted-with" debate.

> **Note on tech-press confusion.** Some reporting (e.g. [LWN Article 1031473](https://lwn.net/Articles/1031473/), describing the *July 2025 RFC*) mentions `Co-developed-by:`. The **merged** doc uses `Assisted-by:`. Cite the in-tree `.rst`, not the RFC-era summaries. *Confidence: high (verified against the master-branch source).*

*Confidence on kernel section: high.*

---

## 2. QEMU — hard ban, rooted in the DCO

QEMU's contributor docs include a dedicated "Code provenance" page whose current standing policy is a blanket decline. Verbatim ([qemu.org](https://www.qemu.org/docs/master/devel/code-provenance.html) / [`.rst`](https://github.com/qemu/qemu/blob/master/docs/devel/code-provenance.rst)):

- **The rule:** *"Current QEMU project policy is to DECLINE any contributions which are believed to include or derive from AI generated content. This includes ChatGPT, Claude, Copilot, Llama and similar tools."* Contributors are asked to *"refrain from using AI content generators on patches intended to be submitted to the project,"* and QEMU *"will decline any contribution if use of AI is either known or suspected."*
- **Named tools:** *"GitHub's CoPilot, OpenAI's ChatGPT, Anthropic's Claude, and Meta's Code Llama, and code/content generation agents which are built on top of such tools."*
- **Rationale = the DCO, not aesthetics.** *"With AI content generators, the copyright and license status of the output is ill-defined with no generally accepted, settled legal foundation."* Training data includes *"large volumes of material under restrictive licensing/copyright terms,"* so *"how contributors could comply with DCO terms (b) or (c) for the output of AI content generators commonly available today is unclear,"* and the project is unwilling to accept the legal risk.
- **Escape hatch:** exceptions are handled case-by-case via the qemu-devel list; *"This policy may evolve as AI tools mature and the legal situation is clarified."*

**Evolving / contested (flag).** A May 2026 patch by Paolo Bonzini, *"docs/devel: relax policy on AI-generated contributions"* ([qemu-devel 2026-05 msg07614](https://lists.nongnu.org/archive/html/qemu-devel/2026-05/msg07614.html), dated ~2026-05-28), proposes narrow carve-outs: mechanical changes, **small bug fixes ≤20 lines (excluding tests)**, tests, and documentation — with an **`AI-used-for:`** commit trailer (values like `code`, `tests`, `docs`) and the human still fully on the hook via `Signed-off-by:`. A follow-up series clarified *"AI exceptions are in addition to DCO"* ([mail-archive](https://www.mail-archive.com/qemu-devel@nongnu.org/msg1140116.html)). **Merge status not confirmed** from the thread excerpts; treat the ≤20-line threshold and `AI-used-for:` trailer as *proposed*, not settled policy. *Confidence: high on the standing ban; medium on the exact status of the relaxation.*

---

## 3. Gentoo — the earliest formal ban

Gentoo's elected Council adopted a policy (proposed by Michał Górny) via unanimous vote. Verbatim from the [Gentoo wiki policy page](https://wiki.gentoo.org/wiki/Project:Council/AI_policy):

- **The ban:** *"It is expressly forbidden to contribute to Gentoo any content that has been created with the assistance of Natural Language Processing artificial intelligence tools."*
- **Adopted:** Council vote **2024-04-14** (date stated on the wiki policy page itself). LWN reports it passed **unanimously** ([LWN 970072](https://lwn.net/Articles/970072/)); ⚠ neither the wiki nor LWN 970072 gives a numeric tally, so the earlier "6–0, one absent" figure was unverified and has been removed.
- **Three stated motivations** (exact section headers): **"Copyright concerns"**, **"Quality concerns"**, **"Ethical concerns"** — spanning unsettled copyright/copyleft risk, LLMs producing "plausibly looking but meaningless" output that shifts review burden onto humans, and industry-level ethics (training-data copyright violations, energy/water use, labor harm, spam/scams).
- **Scope/exceptions:** does **not** ban packaging AI software or software developed upstream with AI help; only direct Gentoo contributions authored with such tools. The Council noted the policy *"can be revisited"* if a tool emerges that avoids the copyright, ethical, and quality concerns.
- **Enforcement:** trust-based; Gentoo does not attempt to detect AI use in submissions (per Górny, reported in [LWN 970072](https://lwn.net/Articles/970072/)).

*Confidence: high.*

---

## 4. NetBSD — AI code is "tainted" by presumption

NetBSD folded AI into its existing licensing/provenance discipline rather than writing a bespoke AI policy. The relevant text sits in **Guideline #2 of the Commit Guidelines** ("Do not commit tainted code to the repository"). Verbatim ([netbsd.org](https://www.netbsd.org/developers/commit-guidelines.html)):

> *"Code generated by a large language model or similar technology, such as GitHub/Microsoft's Copilot, OpenAI's ChatGPT, or Facebook/Meta's Code Llama, is presumed to be tainted code, and must not be committed without prior written approval by core."*

The framing is licensing hygiene: like copied third-party code of unknown license, AI output is *presumed* to carry uncertain copyright/licensing and is therefore inadmissible absent explicit core sign-off. It is a provenance rule, not an authorship/attribution rule — there is no notion of crediting the AI. Change dated to the May-2024 guidelines update. *Confidence: high on the quoted text; the exact commit date of the guideline edit is from tech-press reporting, so treat "May 2024" as approximate.*

---

## 5. curl / Daniel Stenberg — "AI slop," provenance, and the honest-disclosure line

curl is the clearest case that the objection is to **undisclosed, unverified** AI, not AI as such. Stenberg's primary posts:

- **"The I in LLM stands for Intelligence" (2024-01-02)** — the foundational post. He explicitly does **not** oppose AI assistance: *"Sometimes reporters use AIs or other tools to help them phrase themselves or translate what they want to say... I can't find anything wrong with that."* And: *"I do however suspect that if you just add an ever so tiny (intelligent) human check to the mix, the use and outcome of any such tools will become so much better."* The problem is hallucinated, unverified reports submitted as genuine. ([daniel.haxx.se](https://daniel.haxx.se/blog/2024/01/02/the-i-in-llm-stands-for-intelligence/))
- **"Death by a thousand slops" (2025-07-14)** — quantifies the burden: ~20% of submissions were AI slop; only ~5% of 2025 submissions were genuine vulnerabilities; *"Every report thus engages 3-4 persons... up to an hour or three. Each."* ([daniel.haxx.se](https://daniel.haxx.se/blog/2025/07/14/death-by-a-thousand-slops/))
- **"The end of the curl bug-bounty" (2026-01-26)** — ends the paid bounty (effective ~2026-01-31): *"Previous years we have had a rate of somewhere north of 15% of the submissions ending up confirmed vulnerabilities. Starting 2025, the confirmed-rate plummeted to below 5%."* 87 confirmed vulns / >\$100k paid since April 2019. Bounties gave *"too strong incentives to find and make up 'problems' in bad faith."* ([daniel.haxx.se](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/))
- **Enforcement posture:** curl *"continue[s] to immediately ban and publicly ridicule everyone who submits AI slop to the project."* (This exact quote is from the **2026-01-26 bug-bounty-end post**, not the 2024 post — the 2024 post itself only says Stenberg would have used a HackerOne "ban the reporter" button "if it existed." ⚠ Do not attribute the ban/ridicule line to the 2024 essay.)
- **Disclosure guidance (narrower than a formal rule):** curl's [vulnerability disclosure policy](https://curl.se/dev/vuln-disclosure.html) warns against lazily pasting *"massive, AI-generated explanations,"* placing the burden on the reporter to make the message digestible. ⚠ The page does **not** (as fetched 2026-07) contain an explicit clause requiring reporters to *disclose* AI use or to *verify all AI-stated facts* — that stronger characterization was an overstatement and has been softened to what the page actually says.

> **Interpretation.** curl's line is the *provenance/honesty* line: AI is fine as a drafting/translation aid **if disclosed and human-verified**; deception (hidden, unverified AI) is the violation. This is the practical mirror of the kernel's `Assisted-by:` philosophy — credit/disclose rather than hide. *Confidence: high on the blog quotes; medium on the precise current wording of curl's disclosure page (retrieved indirectly).*

---

## 6. The transparency / provenance counter-argument (case FOR disclosing AI in metadata)

Against the outright-ban camp (QEMU, Gentoo, NetBSD) sits a "disclose-don't-ban" position, embodied most concretely by the **Linux kernel's `Assisted-by:`** and by curl's honest-disclosure stance. The general argument:

- **Provenance beats prohibition.** Bans rely on undetectable self-reporting (Gentoo itself concedes it can't police this — [LWN 970072](https://lwn.net/Articles/970072/)). A disclosure trailer creates an auditable trail instead of driving AI use underground.
- **Human accountability is preserved either way.** In every disclosure model the human keeps the DCO `Signed-off-by:` and full responsibility; the AI tag is credit/tracking, not authorship. The kernel doc is explicit that AI *"MUST NOT add Signed-off-by."* ([coding-assistants.rst](https://github.com/torvalds/linux/blob/master/Documentation/process/coding-assistants.rst))
- **Academic backing.** Hora & Robbes, *"AI Policy, Disclosure, and Human in the Loop: How Are Contribution Guidelines Adapting to GenAI?"* (arXiv:2605.16706, 2026) analyze 1,000 popular GitHub repositories and 118 AI policies, finding that **78% allow** GenAI-generated contributions (22% discourage), **51% require disclosure** of AI use, and **74% mandate human oversight** — i.e. across the ecosystem, *allow-with-disclosure* dominates outright bans. ⚠ The abstract frames this as a **cross-sectional snapshot**, not an explicit temporal "shift from banning to transparency," and does **not** name `Co-authored-by:`/`Assisted-by:` as the disclosure markers; the earlier "documents a shift... proposing markers like Co-authored-by/Assisted-by" phrasing overstated the abstract and has been corrected to the paper's actual figures. ([arxiv.org/abs/2605.16706](https://arxiv.org/abs/2605.16706))

**Wording implication for the report.** The primary sources converge on a fork: (a) **ban** projects (QEMU/Gentoo/NetBSD) don't attribute AI at all, because their objection is DCO/licensing risk that no trailer cures; (b) **disclose** projects prefer a **non-authorial** trailer (`Assisted-by:`) precisely because `Co-authored-by:`/`Co-developed-by:` imply an author who can sign off — and an AI can't. So the "Co-authored-by vs Assisted-with" choice maps directly onto whether you consider the AI an *author* (kernel says no) or merely a *tool that assisted* (kernel says yes → `Assisted-by:`).

---

## Source quality / confidence

| Claim area | Source type | Confidence |
|---|---|---|
| Kernel `Assisted-by:` format, no-`Signed-off-by` rule, human responsibility | In-tree `.rst` on master branch (primary) | **High** — verified against source; corrected RFC-era `Co-developed-by:` confusion |
| QEMU standing ban + DCO (b)/(c) rationale + named tools | qemu.org doc + GitHub `.rst` (primary) | **High** |
| QEMU relaxation (≤20-line, `AI-used-for:` trailer) | qemu-devel mailing-list patch (primary) | **Medium** — proposed; merge status unconfirmed |
| Gentoo "expressly forbidden" wording + 2024-04-14 vote + 3 concerns | Gentoo wiki policy page (primary) + LWN for vote tally | **High** |
| NetBSD "presumed tainted" quote | netbsd.org Commit Guidelines §2 (primary) | **High** on text; **Medium** on exact edit date (~May 2024) |
| curl blog quotes (2024 / 2025 / 2026) + stats | daniel.haxx.se (primary, author-owned) | **High** |
| curl vuln-disclosure AI clause exact current wording | curl.se policy page (primary) | **Medium** — page warns against "massive, AI-generated explanations" but has **no** explicit disclose-AI / verify-AI-facts clause; claim softened |
| Academic ecosystem framing (allow-with-disclosure) | arXiv 2605.16706 (primary preprint) | **Medium** — preprint, not peer-reviewed; abstract = cross-sectional (78% allow / 51% disclosure / 74% oversight), **not** a temporal "shift" and does **not** name Co-authored-by/Assisted-by; framing corrected |

**Open items / caveats.**
- Confirm whether the QEMU relaxation patch (≤20 lines, `AI-used-for:`) actually merged into master, and whether the threshold/trailer name changed. As of this research the standing policy remains the blanket decline.
- NetBSD's exact guideline-edit date should be pinned from the netbsd-www CVS history rather than press reports.
- curl's live vuln-disclosure page wording on AI disclosure should be re-read directly (the HackerOne program page is JS-gated and not fetchable as text).
