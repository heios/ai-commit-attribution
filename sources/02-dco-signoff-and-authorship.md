# The DCO, `Signed-off-by`, and Why an AI Cannot Be a Certifying Co-Author

*Focus: the actual DCO text, what a sign-off legally asserts, the Linux kernel's trailer tags, and the reasoned conclusion that an AI cannot make the certification.*

Research date: 2026-07-08 (re-verified against primary sources on this date).

> **Sourcing note.** Primary sources throughout: the canonical DCO at developercertificate.org, the Linux kernel's `submitting-patches.rst` (rendered + GitHub mirror), Git's own `SubmittingPatches`, the LKML thread that introduced `Co-developed-by:`, the DCO GitHub App, and GitHub's own `Co-authored-by` docs. Where the application to AI is inference (not a direct quote), it is flagged as synthesis.
>
> **TL;DR for the report.** Two different things wear similar names: (1) GitHub's `Co-authored-by:` is a **bare attribution trailer** — no legal assertion, GitHub docs never mention rights or certification. (2) The kernel/Git `Signed-off-by:` (and the `Co-developed-by:` that must be paired with one) **is** the act of certifying the DCO — a first-person legal attestation of rights by a named, accountable person. An AI can be *credited* but cannot *certify*, so it can appear in a bare `Co-authored-by:` but cannot be a `Co-developed-by:` co-author or sign off.

---

## 1. The Developer Certificate of Origin 1.1 (full text)

The DCO is maintained by The Linux Foundation. Full text ([developercertificate.org](https://developercertificate.org/)):

> Developer's Certificate of Origin 1.1
>
> By making a contribution to this project, I certify that:
>
> **(a)** The contribution was created in whole or in part by me and I have the right to submit it under the open source license indicated in the file; or
>
> **(b)** The contribution is based upon previous work that, to the best of my knowledge, is covered under an appropriate open source license and I have the right under that license to submit that work with modifications ... under the same open source license ... ; or
>
> **(c)** The contribution was provided directly to me by some other person who certified (a), (b) or (c) and I have not modified it.
>
> **(d)** I understand and agree that this project and the contribution are public and that a record of the contribution (including all personal information I submit with it, including my sign-off) is maintained indefinitely and may be redistributed consistent with this project or the open source license(s) involved.

The preamble "By making a contribution ... **I certify that:**" and clause (a)'s "**I have the right** to submit it" are quoted verbatim from [developercertificate.org](https://developercertificate.org/). Clauses (a)/(b)/(c) are alternatives ("or"); (d) always applies.

**What it asserts:** a **first-person legal attestation** of (a) authorship + right to submit, (b) a lawfully-submittable derivative, or (c) an unmodified pass-through whose chain of prior certifications is intact — plus (d) a standing agreement that the contribution and the submitter's personal information become a **permanent public record**. Load-bearing point: the DCO asserts rights ("I have the right to submit it") that only a **rights-holding legal actor** can hold, and (d) binds a person's identity into that record. ([developercertificate.org](https://developercertificate.org/))

---

## 2. `Signed-off-by:` and the kernel's trailer tags

Source: the kernel's "Submitting patches" guide, section "Sign your work — the Developer's Certificate of Origin" ([kernel.org rendered](https://www.kernel.org/doc/html/latest/process/submitting-patches.html); verbatim confirmed against the GitHub mirror [`submitting-patches.rst`](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst)).

**Why sign-off exists / what it certifies.** *"To improve tracking of who did what ... we've introduced a 'sign-off' procedure."* And: *"The sign-off is a simple line at the end of the explanation for the patch, which certifies that you wrote it or otherwise have the right to pass it on as an open-source patch."* ([kernel.org](https://www.kernel.org/doc/html/latest/process/submitting-patches.html))

**Sign-off = certifying the DCO.** The guide reproduces DCO 1.1 (clauses (a)–(d)) and says that if you can certify it, *"you just add a line saying: `Signed-off-by: Random J Developer <random@developer.example.org>` using a known identity (sorry, no anonymous contributions.)"* — so adding the trailer **is** the act of certifying the DCO. ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst))

**Ordering.** *"The last Signed-off-by: must always be that of the developer submitting the patch."* Sign-offs accumulate as a patch passes through maintainers, forming the certification chain clause (c) references. ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst))

**The kernel uses `Co-developed-by:`, NOT `Co-authored-by:`** — and it is bound to a sign-off:

> *"Co-developed-by: states that the patch was co-created by multiple developers; it is used to give attribution to co-authors ... when several people work on a single patch. Since Co-developed-by: denotes authorship, every Co-developed-by: must be immediately followed by a Signed-off-by: of the associated co-author."* ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst))

So in the kernel, **attribution of authorship is not permitted without that co-author personally certifying the DCO.** This is the single most important fact for the AI question (see §4).

**Concrete interleaving (from the kernel doc's own examples).** Every `Co-developed-by:` is *immediately* followed by that co-author's `Signed-off-by:`, and the submitter's `Signed-off-by:` comes last. E.g. a patch submitted by the `From:` author ([docs.kernel.org](https://docs.kernel.org/process/submitting-patches.html)):

```
Co-developed-by: First Co-Author <first@coauthor.example.org>
Signed-off-by: First Co-Author <first@coauthor.example.org>
Co-developed-by: Second Co-Author <second@coauthor.example.org>
Signed-off-by: Second Co-Author <second@coauthor.example.org>
Signed-off-by: From Author <from@author.example.org>
```

*"The ordering of Signed-off-by: tags should reflect the chronological history of the patch insofar as possible ... the last Signed-off-by: must always be that of the developer submitting the patch."* Structurally, an AI co-author would need its own `Signed-off-by:` line interleaved here — which it cannot supply. ([docs.kernel.org](https://docs.kernel.org/process/submitting-patches.html))

**Origin of the tag.** `Co-developed-by:` was added to the kernel process docs by Greg Kroah-Hartman on 2017-11-16, precisely because *"git only can have one 'author' of a patch"* yet multiple people may co-create one; the introducing patch states the co-developer *"also needs to have a Signed-off-by: line in the patch as well."* So the sign-off requirement was baked in from day one, not a later add-on. ([LKML, 2017-11-16](https://lkml.iu.edu/hypermail/linux/kernel/1711.2/00256.html))

**Other enumerated trailer tags** ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst)):

| Tag | Meaning (kernel) | Carries a certification? |
|---|---|---|
| `Signed-off-by:` | Certifies the DCO; "you wrote it or ... have the right to pass it on." | **Yes** (the DCO) |
| `Co-developed-by:` | Denotes co-authorship; **must** be followed by that co-author's `Signed-off-by:`. | Yes, via the required paired sign-off |
| `Reviewed-by:` | Patch reviewed and acceptable per the Reviewer's Statement. | Yes (reviewer's statement) |
| `Acked-by:` | The acker has at least reviewed and indicated acceptance. | Weak (acknowledgement) |
| `Tested-by:` | Successfully tested by the named person. | Yes (a factual claim) |
| `Reported-by:` | Credits who found/reported a bug. | No (credit only) |
| `Suggested-by:` | The patch idea was suggested by the named person. | No (credit only) |
| `Fixes:` | Names the commit an issue was introduced in, e.g. `Fixes: 54a4f0239f2e ("KVM: MMU: ...")`. | No (a reference) |

Structural point: only `Signed-off-by:` (and the sign-off that must follow every `Co-developed-by:`) carries the DCO certification. Pure credit tags (`Reported-by`, `Suggested-by`) attribute contribution but certify nothing. ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst))

---

## 3. The identity requirement

The current canonical `submitting-patches.rst` requires a known identity: *"using a known identity (sorry, no anonymous contributions.)"* ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst); [kernel.org](https://www.kernel.org/doc/html/latest/process/submitting-patches.html))

**Important nuance — "known identity" ≠ "legal name," and pseudonyms are OK.** The bar is a *known, accountable, reachable* identity, **not** a real/legal name and **not** an absence of pseudonyms. Git's own `SubmittingPatches` (which uses the same DCO 1.1 sign-off) states this most explicitly:

> *"Please use a known identity in the `Signed-off-by` trailer, since we cannot accept anonymous contributions."*
> *"It is common, but not required, to use some form of your real name. We realize that some contributors ... prefer to contribute under a pseudonym or preferred name and we can accept your patch either way, as long as the name and email you use are distinctive, identifying, and not misleading."*
> *"The goal of this policy is to allow us to have sufficient information to contact you if questions arise about your contribution."* ([git-scm.com/docs/SubmittingPatches](https://git-scm.com/docs/SubmittingPatches))

So the earlier-circulated string "no pseudonyms or anonymous contributions" is **inaccurate** as a statement of the current rule: pseudonyms are explicitly accepted; only *anonymity* is barred. The kernelnewbies guide today just says to store your *"full, legal name"* in git config and does not ban pseudonyms ([kernelnewbies FirstKernelPatch](https://kernelnewbies.org/FirstKernelPatch)). The load-bearing requirement for the AI question is weaker but still fatal to a model: the signer must be a **contactable, accountable party who can answer questions about the contribution** — a bar an LLM/model name cannot meet. *(Confidence: high — Git's doc is explicit and primary.)*

The rationale ties back to DCO (d): the sign-off and personal information are kept indefinitely as a public record for "tracking of who did what," and (per Git) to "contact you if questions arise" — purposes that presuppose a reachable human, not a tool. ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst); [developercertificate.org](https://developercertificate.org/); [git-scm.com/docs/SubmittingPatches](https://git-scm.com/docs/SubmittingPatches))

---

## 4. Synthesis — why an AI/LLM cannot sign off (or be a certifying co-author)

Grounded in the quoted text above; the application to AI is reasoned inference, not a direct quote from the sources.

1. **The DCO is a first-person certification of legal rights.** "By making a contribution ... I certify that ... I have the right to submit it." Holding/asserting "the right to submit" under a license is a legal capacity. An LLM is not a legal person and cannot hold copyright or licensing rights, so it cannot truthfully assert (a), (b), or (c). ([developercertificate.org](https://developercertificate.org/))
2. **The kernel binds the certification to the signer.** "certifies that **you** wrote it or otherwise **have the right** to pass it on." ([kernel.org](https://www.kernel.org/doc/html/latest/process/submitting-patches.html))
3. **The identity requirement excludes non-persons.** Sign-off needs "a known identity (sorry, no anonymous contributions.)" — and, per Git, an identity reachable "if questions arise about your contribution." A model name is not a contactable, accountable party (even a pseudonym must map to a reachable someone). ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst); [git-scm.com/docs/SubmittingPatches](https://git-scm.com/docs/SubmittingPatches))
4. **The certification must be enforceable against the signer.** Clause (d) is an agreement the signer "understand[s] and agree[s]" to; agreement and accountability presuppose a legal actor who can be bound. ([developercertificate.org](https://developercertificate.org/))
5. **Kernel co-authorship *requires* the co-author's own sign-off.** Because "every Co-developed-by: must be immediately followed by a Signed-off-by: of the associated co-author," an entity that cannot sign off cannot be a `Co-developed-by:` co-author. An AI cannot sign off (points 1–4), so it cannot be a kernel co-author. ([mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst))

**Conclusion:** `Signed-off-by`/`Co-developed-by` carry a legally-meaningful attestation of rights by a named, accountable person. An AI can at most be *credited* (a bare, non-certifying trailer), but it **cannot be a certifying co-author**. This is precisely the distinction between GitHub's bare `Co-authored-by:` credit and the kernel's certification-bearing tags.

---

## 5. The key contrast — GitHub `Co-authored-by:` vs kernel `Co-developed-by:` + `Signed-off-by:`

These are the two trailers most likely to be confused in the report. They are **not** equivalent.

| | GitHub `Co-authored-by:` | Kernel `Co-developed-by:` (+ paired `Signed-off-by:`) |
|---|---|---|
| Owner / spec | GitHub product feature ([docs.github.com](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors)) | Linux kernel process docs ([docs.kernel.org](https://docs.kernel.org/process/submitting-patches.html)) |
| What it asserts | **Nothing legal.** Pure attribution/credit so a commit shows multiple authors. GitHub's docs describe format and email-matching only — no mention of rights, license, or certification. | A **DCO certification** of authorship + right to submit, by that named co-author, via the mandatory paired `Signed-off-by:`. |
| Requires a sign-off / certification? | No. | **Yes** — "every `Co-developed-by:` must be immediately followed by a `Signed-off-by:` of the associated co-author." |
| Email semantics | Must match the co-author's **GitHub account** email (or their `noreply`) for it to count as a contribution. | Sign-off email is matched to the **commit author**; identity must be known/contactable. |
| Can an AI plausibly appear here? | Mechanically yes (it's just text/credit) — but this is *credit*, not certification. | **No** — the AI would need to supply its own certifying `Signed-off-by:`, which it cannot. |

GitHub's "Creating a commit with multiple authors" doc specifies only the trailer format (`Co-authored-by: NAME <NAME@EXAMPLE.COM>`) and that the email must match the co-author's GitHub account (or their no-reply address) to count as a contribution; it contains **no** language about rights, licensing, or a certificate. ([docs.github.com](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors)) This is why `Co-authored-by: Claude ...`-style trailers are legally inert credit, whereas the kernel's tags are not.

---

## 6. Machine enforcement — the DCO GitHub App

The Probot-based DCO App ([github.com/dcoapp/app](https://github.com/dcoapp/app)) mechanically enforces sign-off in many repos:

- It describes the DCO as *"a lightweight way for contributors to certify that they wrote or otherwise have the right to submit the code they are contributing."* ([README](https://raw.githubusercontent.com/dcoapp/app/main/README.md))
- **Enforcement:** on each PR it verifies **every commit** has a valid `Signed-off-by:` line and sets a pass/fail status check; unsigned commits block the PR until fixed. ([github.com/dcoapp/app](https://github.com/dcoapp/app))
- **Format & matching:** `Signed-off-by: Full Name <email>`, with the sign-off email matched to the **commit author's** email. ([README](https://raw.githubusercontent.com/dcoapp/app/main/README.md))

**Implication:** because the bot ties `Signed-off-by:` to an accountable human account, there is no mechanism (or intent) for an LLM to satisfy the check on its own behalf. ([github.com/dcoapp/app](https://github.com/dcoapp/app))

---

## Source quality / confidence

All load-bearing claims trace to primary sources: the canonical DCO ([developercertificate.org](https://developercertificate.org/)), the Linux kernel process docs ([docs.kernel.org](https://docs.kernel.org/process/submitting-patches.html) / [torvalds/linux mirror](https://raw.githubusercontent.com/torvalds/linux/master/Documentation/process/submitting-patches.rst)), Git's own [`SubmittingPatches`](https://git-scm.com/docs/SubmittingPatches), the LKML patch that introduced the tag ([1711.2/00256](https://lkml.iu.edu/hypermail/linux/kernel/1711.2/00256.html)), the DCO App ([dcoapp/app](https://github.com/dcoapp/app)), and GitHub's own [multiple-authors doc](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors). Re-verified 2026-07-08.

- **High confidence:** DCO 1.1 verbatim text and clause meanings; DCO is "The Linux Foundation and contributors" (2004, 2006), verbatim-copy-only license; sign-off = DCO-certification; the `Co-developed-by:` → mandatory paired `Signed-off-by:` rule and interleaving examples; "last Signed-off-by must be the submitter"; the full trailer taxonomy; DCO App enforcement (per-commit `Signed-off-by:` check, email matched to commit author, blocks PR); GitHub `Co-authored-by:` carries no legal/certification language.
- **Corrected in this pass (was previously mis-stated):** the rule is a *known/contactable* identity, **not** "no pseudonyms." Git's `SubmittingPatches` explicitly accepts pseudonyms; only anonymity is barred. The old "no pseudonyms or anonymous contributions" phrasing is inaccurate for the current rule and was dropped.
- **Synthesis vs. quote:** §4's application to AI is reasoned inference from the quoted DCO/kernel/Git text, not a direct source quote — but QEMU makes essentially the same argument explicitly (see [`04-project-policies-and-controversy.md`](04-project-policies-and-controversy.md)).
- **Not independently re-fetched this pass:** the exact `www.kernel.org/doc/html/latest/...` rendered-HTML string (used the `docs.kernel.org` render and the GitHub raw mirror instead — content is identical). Low risk.
