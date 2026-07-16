---
name: build-linkedin-profile
description: Analyze one or more local codebases and optional Git history to draft an evidence-based LinkedIn headline, About section, job-title options, experience bullets, and skills list. Use when a user wants to infer or refresh their professional profile, role description, career narrative, or technical skills from code they wrote or maintained, including when authorship must be checked with Git log or blame.
---

# Build LinkedIn Profile

Create credible profile copy from repository evidence and the user's own context. Treat code as evidence of demonstrated work, not proof of business impact, seniority, employment, or sole ownership.

## 1. Establish scope and identity

Ask only for information that cannot be discovered safely. Confirm:

- which repositories or directories are in scope;
- the user's Git author names/emails when history contains multiple identities;
- the target role, audience, tone, and desired output language;
- whether current employer/client and project names may be disclosed.

If the request already supplies enough context, begin inspection and defer remaining questions until they are material. Never search outside the paths the user placed in scope.

## 2. Inspect the codebase efficiently

Start with relevant, high-signal files rather than reading the entire repository:

1. Inspect top-level structure and repository status.
2. Read project documentation, manifests, dependency files, service boundaries, schemas, CI/CD configuration, and representative tests.
3. Identify languages, frameworks, architecture, product domains, operational responsibilities, and quality practices.
4. Follow references into implementation files only where needed to substantiate a claim.

Prefer `rg --files` and targeted `rg` searches. Exclude generated files, vendored dependencies, lockfiles, build output, and secrets. Do not expose proprietary source, credentials, internal URLs, customer data, vulnerability details, or confidential names in the final copy.

## 3. Attribute contributions when needed

Use Git history only when authorship or recency changes the conclusions. Start with low-cost checks such as:

```bash
git shortlog -sne --all
git log --date=short --format='%ad%x09%an%x09%ae%x09%s' -- <path>
git diff --stat <relevant-range>
```

Use `git blame` last, on specific lines or files, to resolve a concrete attribution question. Account for renames, pair programming, squashed commits, generated code, shared accounts, and work performed outside Git. Do not equate commit volume or lines changed with importance or skill.

Classify evidence as:

- **Verified contribution**: directly attributable through history or explicit user confirmation.
- **Repository exposure**: present in a repository the user works in, but personal ownership is unclear.
- **User-reported context**: supplied by the user and not independently visible in code.
- **Inference**: plausible interpretation that requires confirmation.

## 4. Build and confirm an evidence ledger

Summarize material evidence before drafting public claims. For each candidate claim, record:

| Candidate claim | Evidence | Attribution | Confidence | Public-safe? |
|---|---|---|---|---|
| Example: designed typed API integrations | client modules, schemas, contract tests | verified contribution | high | confirm client naming |

Ask a compact set of follow-up questions focused on gaps that materially improve the profile, typically:

1. What was your official title, employment type, and date range?
2. Which outcomes did your work enable, and which metrics can you substantiate?
3. What did you personally lead, own, or collaborate on?
4. Who used the system and at what meaningful scale?
5. Which direction do you want the profile to optimize for next?

Never invent metrics, scope, leadership, production usage, or business outcomes. Replace unsupported specifics with neutral language or mark them `[confirm]`.

## 5. Draft the profile

Produce the sections the user requests; otherwise provide:

1. **Positioning summary**: two or three sentences explaining the strongest defensible narrative.
2. **Headline options**: three variants, each concise and keyword-aware without keyword stuffing.
3. **Job-title options**: conservative, market-standard, and aspirational variants. Clearly separate an official title from a positioning title.
4. **About section**: first-person, specific, readable, and free of unsupported superlatives.
5. **Experience entry**: a short role overview followed by outcome-oriented bullets. Use evidence-backed actions when outcomes are unknown.
6. **Skills**: prioritize 15-25 relevant skills, grouped as core, supporting, and only-if-confirmed. Use product/domain capabilities as well as tools.
7. **Verification notes**: list assumptions, `[confirm]` items, sensitive details removed, and evidence gaps.

Distinguish familiarity from proficiency. Do not claim a technology merely because it appears in a dependency file; require meaningful implementation, configuration, tests, or explicit confirmation.

## 6. Quality check

Before delivery:

- trace every material claim to the ledger;
- remove confidential implementation detail and unsupported causality;
- use plain, natural language rather than generic AI phrasing;
- avoid repetitive verbs and vague phrases such as “results-driven”;
- keep tense, dates, title framing, and point of view consistent;
- explain that the user should approve all public claims before publishing.

When evidence is insufficient, deliver a provisional draft with clearly labeled gaps instead of overstating certainty.
