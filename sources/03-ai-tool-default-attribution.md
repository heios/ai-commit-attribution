# 03 — What AI Coding Tools Emit by Default (Commit Attribution)

**Scope:** For each major AI coding tool, the *default* commit/PR attribution it writes, whether that identity is machine-parseable, and how to turn it off — cited to each tool's own docs/changelog/source. Verified July 2026.

---

## Summary table

| Tool | Default attribution (out of the box) | Machine-parseable? | How to disable | Primary source |
|------|--------------------------------------|--------------------|----------------|----------------|
| **Claude Code** | Commit: `Co-Authored-By: Claude <model> <noreply@anthropic.com>` git trailer. PR body: `🤖 Generated with [Claude Code](https://claude.com/claude-code)`. Web/Remote sessions also add a `Claude-Session` trailer. | **Yes** — RFC-822-ish `Co-Authored-By` trailer + `Claude-Session` trailer | Set `attribution.commit` and `attribution.pr` to `""` (and `attribution.sessionUrl:false`) in `settings.json`; legacy `includeCoAuthoredBy:false` (deprecated). | [code.claude.com/docs/en/settings](https://code.claude.com/docs/en/settings) |
| **GitHub Copilot coding agent** (cloud agent) | Commits authored by the bot identity **`Copilot` / `copilot-swe-agent[bot]`** (noreply `…-Copilot@users.noreply.github.com`); commits are cryptographically **signed / Verified**. | **Yes** — real GitHub bot account is the git author identity | Not a user setting; author identity is the agent. Some teams add a human `Co-authored-by` trailer or rewrite author on squash-merge. | [github.blog/changelog 2026-04-03](https://github.blog/changelog/2026-04-03-copilot-cloud-agent-signs-its-commits/) |
| **GitHub Copilot in VS Code** (chat/agent edits, human commits) | Appends `Co-authored-by: Copilot <copilot@github.com>` trailer when the working commit contains AI-generated code. | **Yes** — `Co-authored-by` trailer | Set `git.addAICoAuthor` to `"off"` (values: `off`, `chatAndAgent`, `all`). | [github.com/microsoft/vscode #314311](https://github.com/microsoft/vscode/issues/314311) |
| **Aider** | Adds `Co-authored-by: aider (<model>) <…>` trailer **and** appends `(aider)` to git author + committer name. | **Yes** — `Co-authored-by` trailer + name-field suffix | `--no-attribute-co-authored-by`, `--no-attribute-author`, `--no-attribute-committer` (or `AIDER_ATTRIBUTE_*=false`, or YAML). | [aider.chat/docs/config/options.html](https://aider.chat/docs/config/options.html) (current defaults) · [HISTORY](https://aider.chat/HISTORY.html) |
| **Cursor** (Agent commits/PRs) | Adds a **`Made with Cursor`** trailer to commits/PRs the Agent creates. **On by default.** | Partial — free-text trailer line, *not* a `Co-authored-by` identity | Toggle off in **Settings → Agent → Attribution** (v3.11+: **Git & PRs → Attribution**); Enterprise admins can force off. | [cursor.com/help/integrations/git](https://cursor.com/help/integrations/git) |
| **Google Jules** | **Jules is sole author of every commit by default** (its own git identity). | Yes — git author identity | User-level setting offers 3 modes: Jules-only (default), you+Jules co-authored, or you-only (Jules commits under your identity). | [jules.google/docs/changelog/2026-02-19](https://jules.google/docs/changelog/2026-02-19/) |
| **OpenAI Codex CLI** | **No commit attribution by default.** The experimental `codex_git_commit` feature (which added a `Co-authored-by: Codex <noreply@openai.com>` trailer) is `default_enabled: false` and now marked **Removed** in source. | N/A by default | Nothing to disable by default; the feature flag is defunct. | [openai/codex features/src/lib.rs](https://github.com/openai/codex/blob/main/codex-rs/features/src/lib.rs) |
| **Windsurf (Codeium)** | **No documented default commit-attribution trailer.** Generates commit *messages* on request; no evidence it inserts a co-author/byline. | — | N/A (none documented) | (no primary doc found — see confidence notes) |
| **Sourcegraph Cody / Amp** | **No documented default commit attribution.** Sourcegraph's own repo guidelines actually discourage `Co-Authored-By` noise in commit bodies. | — | N/A (none documented) | [sourcegraph.com/docs/cody](https://sourcegraph.com/docs/cody) |
| **Google Gemini Code Assist** | No documented default commit-attribution trailer for the IDE assistant (distinct from Jules). | — | N/A (none documented) | (no primary doc found — see confidence notes) |

> "Machine-parseable" = a structured git trailer (`Key: Value` line in the commit footer) or a real git author/committer identity, as opposed to prose. Only trailers and identities are reliably queryable via `git log --format` / `git interpret-trailers`.

---

## Per-tool detail (with exact strings)

### Claude Code (Anthropic)
Claude Code adds attribution to **both** commits and PRs, configured separately under the `attribution` setting.

- **Default commit trailer** (exact, from docs): `Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>` — "The model name in the trailer reflects the active model for the session." So the identity varies by model (`Claude Sonnet 4.6`, `Claude Sonnet 5`, etc.), but the email is always `noreply@anthropic.com`.
- **Default PR body attribution** (exact): `🤖 Generated with [Claude Code](https://claude.com/claude-code)`.
- **Session link:** in web / Remote Control sessions it also appends the claude.ai session link as a **`Claude-Session`** trailer on commits (governed by `attribution.sessionUrl`, default `true`).
- **Disable:** set `attribution.commit` and `attribution.pr` to `""` and `attribution.sessionUrl` to `false` in `settings.json`. The docs state the `attribution` object "takes precedence over the deprecated `includeCoAuthoredBy` setting." `includeCoAuthoredBy:false` still works but is deprecated (as of ~v2.0.62).
- Custom example from docs:
  ```json
  { "attribution": { "commit": "Generated with AI\n\nCo-Authored-By: AI <ai@example.com>", "pr": "" } }
  ```
- Source: <https://code.claude.com/docs/en/settings> (the "Attribution settings" section). Note: multiple GitHub issues report the trailer intermittently persisting despite the setting (e.g. anthropics/claude-code #6848, #7543, #7666) — a known-bug caveat, not the documented behavior.

### GitHub Copilot — two distinct products, don't conflate them
1. **Copilot coding agent / "cloud agent"** (the autonomous agent that opens PRs): commits are made under the **`Copilot` bot account** (`copilot-swe-agent[bot]`), with a GitHub noreply email of the shape `…-Copilot@users.noreply.github.com`. As of the 2026-04-03 changelog it **signs every commit** so they show as *Verified*. Consequence: on **Squash & merge**, GitHub keeps the last commit's author, so the merged commit is authored by *Copilot*, not the human — a widely-reported `git blame` complaint (GitHub community discussions #179983, #184395). Primary: <https://github.blog/changelog/2026-04-03-copilot-cloud-agent-signs-its-commits/>. (The official docs pages — docs.github.com "About/Use cloud agent" — describe the workflow but do **not** spell out the exact author string; confidence on the exact email shape is medium, sourced to community discussions + the changelog.)
2. **Copilot in the VS Code editor** (you commit, Copilot helped): VS Code appends **`Co-authored-by: Copilot <copilot@github.com>`** via the `git.addAICoAuthor` setting. Values: `off`, `chatAndAgent` (attribute only commits containing chat/agent-generated code), `all`. This shipped enabled and triggered a public backlash (VS Code #314311, The Register 2026-05-04), after which Microsoft adjusted defaults. Primary: <https://github.com/microsoft/vscode/issues/314311>.

### Aider
Default behavior (from aider's own git docs + options reference):
- `--attribute-author` **default True** → appends `(aider)` to the git **author** name for aider-authored changes.
- `--attribute-committer` **default True** → appends `(aider)` to the git **committer** name.
- `--attribute-co-authored-by` **default True** → appends a `Co-authored-by:` trailer; when True it **takes precedence** over author/committer name modification unless those are explicitly set True.
- `--attribute-commit-message-author` **default False**, `--attribute-commit-message-committer` **default False** → prefix messages with `aider: `.
- The trailer takes the form `Co-authored-by: aider (<model-name>) <…>`. ⚠ **unverified format:** aider's docs (options reference, git integration page, HISTORY) confirm the co-authored-by trailer is default-on but do **not** document that the trailer embeds the model name, nor the exact email address. The `(<model-name>)` and email portions are inferred from observed behavior/third-party reports, not pinned in aider's own docs — low confidence on the literal string.
- **Disable:** `--no-attribute-author`, `--no-attribute-committer`, `--no-attribute-co-authored-by`; env `AIDER_ATTRIBUTE_*=false`; or `.aider.conf.yml`.
- Sources: <https://aider.chat/docs/config/options.html> (authoritative for current defaults), <https://aider.chat/HISTORY.html> (v0.85.0: "Co-authored-by attribution is now enabled by default for commit messages"). ⚠ Note: the prose on <https://aider.chat/docs/git.html> still describes the *older* behavior (co-authored-by off, `(aider)` name-suffix on by default) and contradicts the current default — rely on options.html + HISTORY for the True default.

### Cursor
- Cursor's Agent, when it creates commits or PRs, adds a **`Made with Cursor`** trailer, and "**Attribution is on by default.**"
- This is a free-text trailer/byline, **not** a `Co-authored-by` identity — so it flags AI involvement but does not add a machine-identity co-author.
- **Disable:** *Cursor Settings → Agent → Attribution* (v3.11+: *Git & PRs → Attribution*); Enterprise admins can disable org-wide via Admin Dashboard.
- Source: <https://cursor.com/help/integrations/git>. (Community threads confirm the on-by-default and initial lack of an off toggle: Cursor forum #150096, #151724.)

### Google Jules
- Per the 2026-02-19 changelog, Jules offers **three authorship modes**, applied at user level across all repos:
  1. **Jules as sole author** — *default, existing behavior* (Jules's own git identity authors every commit).
  2. **Co-authored** — "co-authored by both you and Jules."
  3. **You as sole author** — "Jules applies its changes under your identity."
- The changelog does **not** publish the exact author name/email string Jules uses — medium confidence on the literal identity, high confidence on the default = Jules-sole-author.
- Source: <https://jules.google/docs/changelog/2026-02-19/>.

### OpenAI Codex CLI
- **By default Codex CLI adds no commit attribution.** The relevant feature flag `codex_git_commit` is declared in source as `default_enabled: false` and `stage: Stage::Removed` (Stage::Removed = "the feature flag is useless but kept for backward compatibility").
- When it *had* existed as an experimental feature it injected a `Co-authored-by: Codex <noreply@openai.com>` trailer via prompt injection, configurable through a `commit_attribution` string. That behavior is **not the current default and is effectively gone.** Third-party blogs describing `commit_attribution = "Codex <noreply@openai.com>"` as a live default are describing this since-removed experimental feature — treat as historical.
- Note distinct: the Codex *git identity* misattribution issue (`codex@example.com`) reported in openai/codex #18095 is a separate concern from co-author trailers.
- Sources: <https://github.com/openai/codex/blob/main/codex-rs/features/src/lib.rs> (feature spec), <https://github.com/openai/codex/issues/19799> (clarification issue, closed). The official config reference (developers.openai.com/codex/config-reference) and CLI docs do **not** document a default commit trailer.

### Windsurf (Codeium), Sourcegraph Cody/Amp, Gemini Code Assist
- **No primary documentation** was found stating any of these three inject a *default* commit co-author/byline. They generate commit *messages* on request (Windsurf's "Generate commit message"), but that is message text, not an attribution trailer.
- Sourcegraph's own dev guidelines *discourage* `Co-Authored-By` clutter in commit bodies (<https://docs.sourcegraph.com/dev/background-information/commit_messages>), which is weak evidence Cody/Amp do not add one by default.
- **Absence of a documented default ≠ proof it never adds one** — flagged as an open question below.

---

## Source quality / confidence

| Claim | Confidence | Basis |
|-------|-----------|-------|
| Claude Code default trailer `Co-Authored-By: Claude <model> <noreply@anthropic.com>` + PR line + disable via `attribution`/`includeCoAuthoredBy` | **High** | Anthropic's own settings docs, exact strings quoted. |
| Aider defaults (co-authored-by True, `(aider)` name suffix, flags) | **High** (defaults/precedence), **Low** (literal trailer string) | options reference + HISTORY confirm co-authored-by default flipped to True (v0.85.0) and precedence; but the model-name-in-trailer and email portions are **not** documented by aider. NB: git.html prose is stale and contradicts the current True default. |
| Jules default = Jules sole author; 3 modes | **High** (default), **Medium** (literal identity string) | Jules changelog quotes modes but not the exact author/email. |
| Cursor `Made with Cursor`, on by default, disable path | **High** | Cursor's own Git help page. |
| Copilot **coding agent** commits authored by `copilot-swe-agent[bot]` / `…-Copilot@users.noreply.github.com`, signed | **Medium-High** | Signing = official changelog (high); exact bot email shape = community discussions (medium); official docs don't spell out the string. |
| Copilot **VS Code** `git.addAICoAuthor` → `Co-authored-by: Copilot <copilot@github.com>`, values off/chatAndAgent/all | **High** | Microsoft's own vscode repo issue #314311 + release notes; exact email medium. |
| Codex CLI: no default attribution, `codex_git_commit` removed & default-off | **High** | openai/codex source (`features/src/lib.rs`) directly inspected. |
| Windsurf / Cody / Gemini Code Assist: no documented default attribution | **Medium** | Negative finding — no primary doc found; cannot prove absence. |

**Contested / watch-outs:**
- Claude Code's setting has repeatedly been reported as *not fully respected* (trailer/footer reappearing) — anthropics/claude-code #6848, #7543, #7666, #4224. The documented behavior is clear; the runtime reliability is buggy.
- Copilot's VS Code co-author default was rolled out, hit backlash (heise, The Register, May 2026), and defaults were changed — verify the current default value against the running VS Code version rather than assuming.
- Codex's `commit_attribution`/`Codex <noreply@openai.com>` appears in third-party blogs as if current; per source it is a removed experimental feature. Do not cite the blogs as authoritative for present behavior.

**Open questions:**
1. Exact literal git author name/email Jules and the Copilot coding agent write (docs omit the precise string).
2. Whether Windsurf, Cody/Amp, or Gemini Code Assist add any attribution in their newer *agent* modes (as opposed to inline assist) — no primary doc located.
3. Aider's exact co-authored-by email field (model name is documented; the address is not pinned in docs).
