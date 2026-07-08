# Glossary

Plain-language definitions of the less-common terms used in **[REPORT.md](REPORT.md)** — the git-trailer mechanics, open-source governance instruments, software-provenance specs, and copyright case law behind the *credit AI as a tool, not a co-author* argument. Everyday git/GitHub basics (commit, branch, pull request, merge, the contribution graph) are left out on purpose.

Ordered roughly by how central each term is to the report's reasoning.

---

### DCO

**Developer Certificate of Origin.** A short, standardized statement a contributor affirms — by adding a Signed-off-by line to a commit — certifying that they wrote the code or otherwise have the right to submit it under the project's open-source license. It's a lightweight, per-commit alternative to signing a full legal agreement.

*In this report:* the DCO is the legal instrument behind Signed-off-by, and the core argument is that an AI can't make this certification because it isn't an accountable legal person.

Primary source: <https://developercertificate.org/>

---

### Signed-off-by

A git commit trailer naming a person; in DCO projects, adding the line IS the act of certifying the Developer Certificate of Origin for that patch — a first-person legal attestation by a named, accountable human.

*In this report:* it's the certification line an AI cannot supply, and the Linux kernel rule that 'AI agents MUST NOT add Signed-off-by tags' is central.

Primary source: <https://git-scm.com/docs/SubmittingPatches>

---

### Co-authored-by

A GitHub commit-message convention that credits additional authors of a commit; when the email matches a real account, GitHub renders each as a linked co-author and gives contribution credit.

*In this report:* it's the tool default the whole argument pushes back on for AI, because it asserts authorship a machine can't legally hold.

Primary source: <https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors>

---

### Co-developed-by

A Linux-kernel commit trailer marking genuine shared authorship of a patch, which the kernel requires to be immediately followed by that co-author's own Signed-off-by line.

*In this report:* it's semantically broken for AI (the AI can't supply the mandatory paired sign-off), so the kernel rejected it and Kubernetes bans it by name.

Primary source: <https://docs.kernel.org/process/submitting-patches.html>

---

### CLA

**Contributor License Agreement.** A legal agreement a contributor signs granting a project (or its foundation) rights to use their contribution, typically requiring them to represent that they have the authority to grant those rights. It's heavier-weight than a one-line DCO sign-off.

*In this report:* like the DCO, a CLA assumes a human legal person who can make representations — something an AI can't do — reinforcing the case for keeping the author line human.

Primary source: <https://www.apache.org/licenses/contributor-agreements.html>

---

### ICLA

**Individual CLA.** The individual (as opposed to corporate) version of a Contributor License Agreement, in which a person represents that each contribution is their own original creation.

*In this report:* it cites Apache's ICLA §5 ('Your original creation') to show contribution agreements are anchored on human original authorship an AI can't satisfy.

Primary source: <https://www.apache.org/licenses/icla.pdf>

---

### git trailer

Git's generic machinery for 'Token: value' lines (email-header style) in a blank-line-separated block at the end of a commit message; git-interpret-trailers adds or parses them, and git itself assigns them no special meaning — they're just plain, greppable metadata.

*In this report:* it's the foundation of the whole analysis — every attribution phrasing (Co-authored-by, Assisted-by, and the rest) is just an ordinary trailer to git; only GitHub and projects layer extra meaning on top.

Primary source: <https://git-scm.com/docs/git-interpret-trailers>

---

### Conventional Commits

A specification for structuring commit-message subject lines with a type prefix like feat:, fix:, or chore: so history is machine-parseable and can drive changelogs and version bumps.

*In this report:* it's the reference point for 'structured, greppable commit metadata' — a useful contrast to trailers, which developers sometimes conflate with it.

Primary source: <https://www.conventionalcommits.org/en/v1.0.0/>

---

### Assisted-by

A commit trailer that credits an AI tool as a mere assistant (format 'Assisted-by: AGENT:MODEL [tools]') while keeping the human as sole author and signer. Adopted by the Linux kernel and other foundations.

*In this report:* it's the headline recommendation — modeled on non-authorial kernel credit trailers, so it records the AI's help without triggering the DCO certification problem.

Primary source: <https://docs.kernel.org/process/coding-assistants.html>

---

### Generated-by

A commit trailer recording that AI produced substantial or whole chunks of the change — a provenance label, not an author claim.

*In this report:* it's the recommended escalation from Assisted-by for heavier AI output; Apache recommends it and OpenInfra uses it for substantial AI help.

Primary source: <https://www.apache.org/legal/generative-tooling.html>

---

### kernel credit trailers

The Linux kernel's standardized credit/review trailers: Acked-by (acknowledges acceptance), Reviewed-by (formal review per the Reviewer's Statement), Tested-by (verified it works), and Reported-by / Suggested-by (pure credit for finding a bug or suggesting an idea). Some carry a factual claim; others are just credit.

*In this report:* Assisted-by is deliberately modeled on these non-authorial tags, which is why it records involvement without certifying anything the way Signed-off-by does.

Primary source: <https://docs.kernel.org/process/submitting-patches.html>

---

### DCO App

A Probot-based GitHub App that enforces the DCO by failing a pull request's status check unless every commit has a valid Signed-off-by line matching the commit author's email, blocking the PR otherwise.

*In this report:* it mechanically ties sign-off to an accountable human account, so there's no path for an AI to satisfy the DCO check on its own behalf.

Primary source: <https://github.com/dcoapp/app>

---

### noreply

A placeholder email (like noreply@anthropic.com or a GitHub-provided ...@users.noreply.github.com address) that either protects a real address or matches no account, so GitHub links the commit to no profile.

*In this report:* every AI trailer uses one, which is why AI attribution never inflates a human's graph — though such addresses have misattributed commits to whoever registered them.

Primary source: <https://docs.github.com/en/account-and-profile/how-tos/contribution-settings/troubleshooting-missing-contributions>

---

### software provenance

Verifiable metadata about where, when, and how a software artifact was produced — its origin and build history — as distinct from who legally authored it.

*In this report:* it frames honest AI trailers as accurate provenance that feeds supply-chain tooling, versus a false authorship claim.

Primary source: <https://slsa.dev/spec/v1.0/provenance>

---

### SLSA

Supply-chain Levels for Software Artifacts — an OpenSSF framework of graded security levels and signed build-provenance attestations describing how an artifact was produced.

*In this report:* it's the provenance framework a machine-readable AI-tool trailer naturally feeds, unlike a human co-author line.

Primary source: <https://slsa.dev/spec/v1.0/>

---

### SBOM

Software Bill of Materials — a formal, machine-readable inventory of the components and provenance that make up a piece of software.

*In this report:* it argues a parseable Assisted-by/Generated-by trailer is better SBOM-style provenance than a fake AI co-author line.

Primary source: <https://www.cisa.gov/resources-tools/resources/2025-minimum-elements-software-bill-materials-sbom>

---

### SPDX

Software Package Data Exchange — an ISO-standard open format and license-identifier vocabulary for expressing SBOM data plus license and copyright metadata. Developers know its license IDs (MIT, Apache-2.0) more than the full spec.

*In this report:* it's part of the supply-chain/provenance ecosystem a structured AI-tool trailer could serve.

Primary source: <https://spdx.dev/use/specifications/>

---

### AI slop

Low-quality, unverified AI-generated content submitted as if it were genuine work — for example, hallucinated bug-bounty reports that waste maintainers' time.

*In this report:* it's curl's term for the undisclosed, unverified AI submissions that drove its bug-bounty shutdown, anchoring the 'disclose, don't hide' argument.

Primary source: <https://daniel.haxx.se/blog/2025/07/14/death-by-a-thousand-slops/>

---

### tainted code

Code of uncertain or encumbered copyright/license origin that a project treats as unsafe to include until its provenance is cleared; NetBSD presumes LLM-generated code to be tainted.

*In this report:* it's NetBSD's framing that treats AI output as a licensing-hygiene hazard rather than an authorship question.

Primary source: <https://www.netbsd.org/developers/commit-guidelines.html>

---

### IP indemnity

A contractual promise — often from an AI-assistant vendor — to defend or compensate a customer against intellectual-property claims arising from using the tool's output.

*In this report:* it's part of the commercial rationale for keeping a single accountable human author rather than implying AI co-ownership (flagged as reasoning, not a sourced rule).

Primary source: <https://www.apache.org/legal/generative-tooling.html>

---

### USCO

**U.S. Copyright Office.** The U.S. federal office that registers copyrights and issues authoritative guidance on what is copyrightable, including that only a human can be an author.

*In this report:* its 2023 guidance and 2025 report are the primary basis for 'AI is not an author' — the legal spine of the argument.

Primary source: <https://www.copyright.gov/ai/>

---

### work made for hire

A U.S. copyright doctrine where an employer or commissioning party — not the individual who created the work — is legally treated as the author and owner.

*In this report:* it's one of the legal 'author' concepts that gets muddied when an AI is labeled a co-author instead of the human or company.

Primary source: <https://www.copyright.gov/circs/circ30.pdf>

---

### joint work

A U.S. copyright term for a work prepared by two or more authors intending their contributions to merge into a unitary whole, giving each co-author a shared ownership interest.

*In this report:* it's the legal reality 'co-author' implies — a status an AI cannot hold, making 'Co-authored-by: <AI>' a claim to a relationship that doesn't exist.

Primary source: <https://www.copyright.gov/title17/92chap1.html#101>

---

### public domain

The status of material that copyright does not protect and anyone may freely use — here, purely AI-generated output that has no human author and so cannot be owned.

*In this report:* it's the consequence that makes a false AI co-author line risky — it could read as conceding part of the code is un-owned.

Primary source: <https://www.copyright.gov/help/faq/faq-definitions.html>

---

### Thaler v. Perlmutter

A D.C. Circuit case (decided 2025; cert. denied 2026) holding that the Copyright Act requires a work to be authored in the first instance by a human being, so a machine cannot be an author.

*In this report:* it's the controlling court decision behind the 'no legal AI author to co-attribute' conclusion.

Primary source: <https://media.cadc.uscourts.gov/opinions/docs/2025/03/23-5233.pdf>

---

### certiorari

The Supreme Court's discretionary agreement to hear an appeal; 'cert. denied' means the Court declined, leaving the lower court's ruling as controlling law.

*In this report:* the Supreme Court denied cert in Thaler (March 2026), which is what makes the human-authorship holding stick.

Primary source: <https://www.supremecourt.gov/docket/docket.aspx>

---

### Naruto v. Slater

The 'monkey selfie' case (9th Cir. 2018, 888 F.3d 418) holding that a non-human — a macaque — has no standing to hold or sue over a copyright.

*In this report:* it's the go-to precedent for 'non-humans cannot be authors,' extended by analogy from animals to AI.

Primary source: <https://cdn.ca9.uscourts.gov/datastore/opinions/2018/04/23/16-15469.pdf>

---

*Generated by the same automated Claude research workflow that produced the report (Claude Opus 4.8, `claude-opus-4-8`), under human direction and review. See [Provenance](REPORT.md#provenance).*
