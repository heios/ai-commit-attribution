# The `Co-authored-by:` Trailer — Mechanics & Semantics

*Focus: what the `Co-authored-by:` trailer actually is, how git treats it, how GitHub renders it, and what it does to the contribution graph.*

Research date: 2026-07-08.

> **Sourcing note.** All claims below rest on primary documentation: GitHub Docs, the `git` reference manual (`git-scm.com`), and GitHub's own contribution-graph reference pages. Direct quotes are used where the exact wording is load-bearing. Confidence is flagged per section.

---

## 1. The GitHub format and behavior (the trailer's "owner")

`Co-authored-by:` is **not a git-native concept**; it is a **GitHub convention** layered on top of git's generic trailer mechanism (see §2). GitHub owns its semantics.

**Exact required format.** GitHub's canonical page ("Creating a commit with multiple authors") gives the template `Co-authored-by: name <name@example.com>` — the literal token `Co-authored-by:`, a display name, and an email in angle brackets. For multiple co-authors, add one `Co-authored-by:` line per person; GitHub's instruction is explicit: *"Do not add blank lines between each co-author line."* ([GitHub Docs](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors))

**Placement.** The trailer must sit at the very end of the commit message, separated from the body by a blank line. GitHub's Tip box: *"If you're using a text editor on the command line to type your commit message, ensure there is a blank line (two consecutive newlines) between the end of your commit description and the `Co-authored-by:` commit trailer."* ([GitHub Docs](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors))

**Which email.** GitHub: *"For the co-author's commit to count as a contribution, you must use the email associated with their account on GitHub.com."* For privacy, *"If a person chooses to keep their email address private, you should use their GitHub-provided `no-reply` email to protect their privacy."* ([GitHub Docs](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors))

**What GitHub does with it.** GitHub reads the email, matches it to a registered account, and if it matches, attributes the commit to that account (profile link, avatar, contribution credit). Co-authored commits "are visible on GitHub" and appear "the next time you push." The account-linking mechanism is the general one for all commit identities: *"GitHub uses the email address in the commit header to link the commit to a GitHub user."* ([GitHub: why commits link to the wrong user](https://docs.github.com/articles/why-are-my-commits-linked-to-the-wrong-user))

*Confidence: high on format/placement/email. The multiple-authors page does not itself spell out avatar rendering; that behavior comes from the commit-linking doc and applies to any resolvable email.*

---

## 2. Git's native trailer machinery — and why `Co-authored-by` is not special to git

**A trailer, formally.** `git interpret-trailers` exists to *"Add or parse *trailer* lines that look similar to RFC 822 e-mail headers, at the end of the otherwise free-form part of a commit message."* In its own example, *"the last two lines starting with `Signed-off-by` are trailers."* ([git-scm: git-interpret-trailers](https://git-scm.com/docs/git-interpret-trailers))

**Trailer-block rules (heuristic, blank-line separated, at the end).** *"Existing trailers are extracted from the input by looking for a group of one or more lines that (i) is all trailers, or (ii) contains at least one Git-generated or user-configured trailer and consists of at least 25% trailers. The group must be preceded by one or more empty (or whitespace-only) lines. The group must either be at the end of the input or be the last non-whitespace lines before a line that starts with `---`."* ([git-scm](https://git-scm.com/docs/git-interpret-trailers)) This block-and-blank-line requirement is exactly why GitHub insists on the blank line before `Co-authored-by:`.

**`token: value` form.** The key and value *"will be separated by "`:` " (one colon followed by one space)."* On keys: *"there can be no whitespace before or inside the `<key>`, but any number of regular space and tab characters are allowed between the `<key>` and the separator."* Values may contain whitespace and may fold across continuation lines (RFC-822 style). ([git-scm](https://git-scm.com/docs/git-interpret-trailers)) **Implication:** a trailer token may contain hyphens (`Co-authored-by`, `Signed-off-by`) but **not spaces** — so `AI-Assisted:` is a valid token, `AI Assisted:` is not.

**Is `Co-authored-by` git-native? No.** The `git interpret-trailers` doc never mentions it; its only example tokens are `Signed-off-by`, `Acked-by`, `Cc`, `Reviewed-by`, `Helped-by`, `Reference-to`, `See-also` (and `Fix`). The string "Co-authored-by" does not appear anywhere in the doc. ([git-scm](https://git-scm.com/docs/git-interpret-trailers)) The `git commit` doc supports a generic `--trailer <token>[(=|:)<value>]` option — *"Specify a (\<token>, \<value>) pair that should be applied as a trailer"* — and points to `trailer.*` config for dedup/ordering, but its worked examples are `Signed-off-by` and `Helped-by`, and "Co-authored-by" does not appear there either. ([git-scm: git-commit](https://git-scm.com/docs/git-commit)) In other words, you produce a `Co-authored-by:` trailer with the *same* generic machinery you'd use for any custom trailer: `git commit --trailer "Co-authored-by:name <email>"` or a hand-typed line in the message body.

**Conclusion:** git parses `Co-authored-by:` only as an *ordinary trailer* and assigns it no meaning. The single git-native "author" is set by `git config user.name` / `user.email`. The trailer is just message text; **any provider-specific meaning (avatars, contribution credit) is added by GitHub, not git.** *Confidence: high.*

---

## 3. Is the identity real/required, or free-form text?

**It is free-form text.** Git records the commit *author* from local config and does not verify it; the email in local config determines author email. ([GitHub](https://docs.github.com/articles/why-are-my-commits-linked-to-the-wrong-user)) The `Co-authored-by:` trailer is likewise just body text — nothing in git's trailer spec requires the value to be a resolvable identity ([git-scm](https://git-scm.com/docs/git-interpret-trailers)). So an arbitrary string like `Co-authored-by: Claude <noreply@anthropic.com>` is syntactically valid git and is stored verbatim.

**What GitHub does when the email matches no account.** Matching is purely email-based. If the email matches no registered account, the co-author is **not** linked to any profile and accrues **no** contribution credit. For the avatar: if the unrecognized email has a Gravatar, *"the Gravatar will be displayed next to the commit, rather than the default gray Octocat"*; otherwise the default gray Octocat is shown. ([GitHub](https://docs.github.com/articles/why-are-my-commits-linked-to-the-wrong-user)) The string `noreply@anthropic.com` matches no GitHub account, so a Claude co-author line renders as an unlinked name with a gray Octocat / Anthropic Gravatar and does **not** pollute any real user's graph via that address. *Confidence: high.*

---

## 4. The contribution graph

**What counts as a contribution.** GitHub's Profile contributions reference lists contribution types; "Making a commit" *sometimes* counts, alongside opening issues/PRs, PR reviews, and discussions. ([GitHub: profile contributions reference](https://docs.github.com/en/account-and-profile/reference/profile-contributions-reference))

**Conditions a commit must meet (all required):**

- *"Commits must be made with an email address that is connected to your account on GitHub, or the GitHub-provided `noreply` email address ... in order to appear on your contributions graph."* ([GitHub: troubleshooting missing contributions](https://docs.github.com/en/account-and-profile/how-tos/contribution-settings/troubleshooting-missing-contributions))
- Commits must be in a standalone repo, not a fork (a fork's commits count only once merged via PR to the parent). ([GitHub](https://docs.github.com/en/account-and-profile/how-tos/contribution-settings/troubleshooting-missing-contributions))
- *"Commits are only counted if they are made in the default branch or the `gh-pages` branch."* ([GitHub](https://docs.github.com/en/account-and-profile/how-tos/contribution-settings/troubleshooting-missing-contributions))
- Plus a relationship to the repo (collaborator/org member, or you forked it, or opened a PR/issue). ([GitHub](https://docs.github.com/en/account-and-profile/reference/profile-contributions-reference))

**Generic / unresolvable emails do not link.** *"Generic email addresses, such as `jane@computer.local`, cannot be added to GitHub accounts and linked to commits. If you've authored any commits using a generic email address, the commits will not be linked to your GitHub profile and will not show up in your contribution graph."* ([GitHub](https://docs.github.com/en/account-and-profile/how-tos/contribution-settings/troubleshooting-missing-contributions))

**Co-authors and the graph.** A co-author gets graph credit via the *same* email-to-account rule: the `Co-authored-by:` email must be connected to a real account (or be that user's GitHub `noreply` address) **and** the commit must meet the branch/repo conditions. That is the meaning of *"you must use the email associated with their account on GitHub.com."* ([GitHub](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors))

**Net effect for AI co-author trailers:** because `noreply@anthropic.com` (and analogous AI emails) resolve to no account, an AI `Co-authored-by:` line does **not** inflate any human's contribution graph. The "contribution-graph pollution" complaint against AI co-authorship is therefore **mostly about human-facing noise in commit logs and the *implication* of authorship, not about literal graph inflation** — the AI address earns no green squares because it links to no account. (Reasoned inference from the email-matching rule above; *confidence: high on the mechanism, medium on how often critics conflate "log noise" with "graph pollution."*)

---

## 5. Other forges: GitLab and Bitbucket

`Co-authored-by:` is stored identically everywhere (it's just commit-message text — see §2), but **rendering/attribution is per-forge** and varies.

| Forge | Parses `Co-authored-by:`? | Behavior |
|---|---|---|
| **GitHub** | Yes | Links each co-author by email, shows avatar, gives contribution-graph credit, "co-authored" indicator on the commit. Canonical implementation. ([GitHub Docs](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors)) |
| **GitLab** | Yes (partial) | GitLab recognizes co-authors in the commit body and shows the co-author's avatar and a link to their profile, but they are not surfaced in the standard commit view without expanding the commit body; open issue [#538959](https://gitlab.com/gitlab-org/gitlab/-/issues/538959) tracks making the display more prominent. ([GitLab issue #538959](https://gitlab.com/gitlab-org/gitlab/-/issues/538959)) |
| **Bitbucket (Server / Data Center)** | No | Feature request [BSERV-10529](https://jira.atlassian.com/browse/BSERV-10529) ("Support 'Co-Authored-By' in bitbucket web-interface") is **Closed – Answered** (resolved 17 Jan 2018). Atlassian's posted status — which the issue page frames as its stance *"as of June 2019"* — is that they are *"not currently planning to address this suggestion in near future."* The trailer is stored but shown as plain text; only the primary Author field is linked to a profile. ([Atlassian BSERV-10529](https://jira.atlassian.com/browse/BSERV-10529)) |

**Takeaway:** GitHub is the only forge that gives full first-class treatment (avatar + graph credit + badge). GitLab links co-authors' profiles/avatars but buries them. Bitbucket Server/DC does not parse the trailer at all. Because the trailer is plain git text, using it never *breaks* anything on a non-supporting forge — it just isn't rendered specially. *Confidence: high on GitHub and Bitbucket-Server; medium on the exact current GitLab UI, since #538959 is an evolving issue and GitLab has not published a dedicated "co-authored commits" docs page comparable to GitHub's.*

*(Scope note: the Bitbucket primary source is for Bitbucket **Server / Data Center**. I did not find an equally authoritative Atlassian statement for **Bitbucket Cloud**; treat Cloud behavior as unconfirmed.)*

---

## Confidence & conflicts

- **High:** trailer definition and `token: value` rules ([git-scm](https://git-scm.com/docs/git-interpret-trailers)); GitHub format/placement/email requirements and email-based account matching.
- **Doc reorganization:** GitHub moved the old "why are my contributions not showing up" content to `profile-contributions-reference` and `.../troubleshooting-missing-contributions`; the old URL still resolves but the verbatim conditions now live at the newer pages cited above. Substance unchanged (high confidence).
- **Medium-high:** avatar rendering for co-authors specifically is inferred from the general commit-linking doc, not a co-author-specific quote.
- **Git-version nuance:** the 25%-trailer heuristic reflects current `git-scm.com` docs; exact heuristics have shifted across git versions, but the "RFC-822-style `token: value` lines in a blank-line-separated block at the end" definition is stable.
- **GitLab:** partial support confirmed from the wording of open issue #538959 (which states GitLab already "recognizes co-authors ... and displays the avatar and a link to the profile"); medium confidence because GitLab lacks a dedicated docs page and the UI is actively being changed.
- **Bitbucket:** high confidence for **Server / Data Center** (official "not planning" answer on BSERV-10529, resolved 17 Jan 2018; the "not planning" language is the status Atlassian shows *as of June 2019*). **Bitbucket Cloud** is unconfirmed from a primary Atlassian source.
- **Cross-forge portability:** high confidence that the trailer is inert plain text on non-supporting forges (follows directly from git storing it as ordinary message text, §2).
