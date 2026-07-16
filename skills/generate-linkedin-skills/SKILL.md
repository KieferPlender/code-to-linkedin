---
name: generate-linkedin-skills
description: Automatically generate a focused, ranked LinkedIn Skills section by analyzing one or more codebases and the user's actual Git contributions. Use when a developer wants to discover which technical, engineering, tooling, domain, and professional skills their code demonstrates; choose the strongest skills to add or prioritize on LinkedIn; omit incidental repository technologies; or refresh their LinkedIn skills for a general profile or target role.
---

# Generate LinkedIn Skills

Turn verified engineering work into a focused LinkedIn Skills section. Analyze code and contribution history deeply enough to distinguish substantive capability from repository exposure. Lead with the skill recommendations, not a methodology report.

## 1. Run an automatic intake

Treat a bare invocation such as `$generate-linkedin-skills` or `/code-to-linkedin:generate-linkedin-skills` as sufficient.

Discover defaults before asking anything:

- use the current repository as the initial scope;
- inspect `git config user.name`, `git config user.email`, and `git shortlog -sne --all` for likely identities;
- infer output language from the conversation;
- apply public-safe confidentiality defaults;
- create a coherent general developer skills list unless the user requests target-role optimization.

Use the product's native question interface when available: `request_user_input` in Codex, `AskUserQuestion` in Claude Code, or a concise conversational question otherwise.

Ask only when identity or repository scope is genuinely ambiguous. Combine necessary choices into one round with no more than two questions:

1. confirm identity only when multiple plausible authors remain;
2. confirm scope only when multiple likely repositories are available or requested.

Do not ask for a target role unless the user requests role-specific optimization. Do not request or analyze the user's current LinkedIn profile. This skill generates recommendations from code and Git evidence; it does not compare, synchronize, or edit LinkedIn.

## 2. Identify the user's work

Inspect high-signal repository areas first: documentation, manifests, architecture, service boundaries, schemas, infrastructure, CI/CD, representative implementation, and tests. Avoid unrelated repository-wide reading.

Use Git history to attribute relevant work:

```bash
git shortlog -sne --all
git log --author='<identity>' --date=short --format='%ad%x09%h%x09%s' --name-only
git log --follow --date=short --format='%ad%x09%an%x09%s' -- <path>
```

Use targeted `git blame` to determine whether the user introduced, evolved, or maintained relevant implementation. Account for renamed identities, co-authorship, squashed commits, pair programming, shared accounts, and work missing from Git. Do not use commit or line counts as a skill score.

For each material candidate, inspect its relevant implementation, configuration, tests, documentation and runbooks, infrastructure, CI/CD, history, blame, and adjacent files. Record the most precise conclusion the evidence supports.

Do not ask the user to interpret accessible code or Git evidence. The default run must finish without post-scan ownership questions. Rank only the scope that the accessible evidence demonstrates.

## 3. Infer demonstrated skills and scope

Generate candidates from substantive evidence, including:

- languages, frameworks, APIs, data models, integrations, and system boundaries;
- debugging, performance, security, reliability, concurrency, and migrations;
- tests, automation, CI/CD, observability, deployment, cloud, and infrastructure;
- databases, messaging, data pipelines, developer tooling, and documentation;
- repeated ownership of product or domain problems;
- architecture, standards, review, or technical leadership when directly supported.

Do not add a technology merely because it appears in a dependency, lockfile, generated file, template, or neighboring module.

Assess evidence strength separately from demonstrated scope.

Evidence strength:

- **Strong**: deep, repeated, recent, or broad contribution evidence, supported by multiple material examples or one clearly substantial implementation.
- **Moderate**: meaningful implementation or maintenance with narrower depth or frequency.
- **Emerging**: real but early or limited demonstrated use.
- **Uncertain**: attribution or interpretation remains unresolved after targeted inspection; omit it from the primary list.

Judge every skill independently. Do not default most candidates to **Strong** merely because they appear in the same substantial feature or repository.

Demonstrated scope:

- **Incidental exposure**: dependency, generated file, copied configuration, or neighboring code only. Omit it.
- **Applied use**: used the technology to implement real product behavior.
- **Implementation and integration**: designed or maintained meaningful implementation, configuration, automation, integrations, or delivery workflows.
- **Administration and operation**: provisioned, secured, upgraded, observed, debugged, or recovered an operable service or platform.
- **Architecture and ownership**: made recurring cross-cutting design decisions or led the capability across systems.

Express scope with a category-appropriate phrase instead of a number. Examples include `Advanced Python implementation`, `API architecture and ownership`, `PostgreSQL application integration`, `CI/CD workflow configuration`, `Kubernetes application delivery`, and `Kubernetes cluster operation`.

Use implementation language by default. Use claims such as `production`, `strategy`, `architecture`, `ownership`, `leadership`, or `operation` only when attributed code, history, documentation, or runbooks directly demonstrate that exact scope. A work repository, deployment configuration, or comprehensive test suite does not by itself prove production responsibility, strategic ownership, or leadership.

Use **administration and operation** only for an operable platform, service, database, runtime, network, or infrastructure system. Never describe a programming language, application framework, library, testing tool, engineering technique, or broad capability such as Backend Development as “administration and operation.”

For Kubernetes, treat authored workload manifests, Helm or Kustomize configuration, probes, resources, application configuration, and rollout workflows as application-delivery evidence. Treat cluster provisioning, upgrades, node pools, add-ons, operators, cluster-wide RBAC or policy, admission control, ingress, networking, storage, autoscaling, observability, recovery, incident fixes, and operational runbooks as platform-operation evidence. Rank `Kubernetes` from the demonstrated depth; add administration or platform-engineering claims only when their evidence exists.

## 4. Normalize and rank for LinkedIn

Translate evidence into recognizable, useful LinkedIn-style labels:

- prefer market-standard terminology over internal component names;
- merge aliases and duplicates;
- include a broad capability and a specific technology only when each adds distinct value;
- prefer an established truthful label such as `Generative AI` over a novel compound label such as `AI Application Development` when both describe the same evidence;
- never replace a compatible protocol or API with a vendor-branded skill unless the user's attributed work demonstrates that vendor's actual service;
- prefer `Object Storage` over `Amazon S3` when only an S3-compatible implementation is evidenced;
- keep confidential domains generic while preserving useful expertise;
- exclude vague filler, language primitives, routine baseline tools, tiny utilities, and low-signal micro-libraries unless important to the target role;
- exclude generated artifacts and repository-wide technologies the user did not meaningfully use.

Use widely recognized market terms. Treat them as recommended search labels unless a live LinkedIn taxonomy is available. Do not claim that a label is selectable or validated against LinkedIn's live taxonomy. If an exact label is unavailable, recommend the nearest truthful standard label without increasing the demonstrated scope or introducing a vendor claim.

Before ranking, run a final overlap pass. When two labels would rely on substantially the same evidence and tell the same story, keep the more recognizable or differentiating label in **Add now** and move the other to the complete list, **Add later**, or omit it. In particular, review broad/specific pairs such as `Automated Testing` and `Pytest`, plus near-synonyms such as `System Integration` and `API Integration`. Retain both only when their rationales demonstrate meaningfully different capabilities.

Rank by:

1. demonstrated strength and scope;
2. coherence with the developer identity the evidence supports;
3. market recognizability and usefulness;
4. recency and breadth;
5. target-role relevance when supplied.

For a general profile, build a coherent story rather than maximizing keyword count. Balance core identity, specialist differentiators, supporting technologies, and delivery or platform capabilities. Recommend 15–20 primary skills by default, with 10–12 in the immediate action set. Use as few as 8 when the evidence is narrow, and include no more than 8 optional additions.

Do not claim that LinkedIn has a special “top five pinning” model or that the platform will preserve a particular order. Produce a recommended priority order.

## 5. Produce the LinkedIn Skills plan

Return these sections in order:

1. **Add now**: 10–12 ranked skills using the table below; use as few as 8 when evidence is narrow.
2. **Recommended priority order**: the complete focused list of 15–20 labels, one per line.
3. **Why the leading skills belong**: compact evidence for the highest-priority recommendations.
4. **Add later**: up to 8 demonstrated but less important skills.
5. **Do not add**: incidental, overly granular, misleading, vendor-inflated, generated, or weakly attributed technologies.

Use this table for the immediate action set:

| Order | LinkedIn skill | Demonstrated scope | Evidence strength | Why it belongs |
|---:|---|---|---|---|

Do not include an Experience-mapping column when no profile is in scope. Do not bury the copy-ready list beneath a long explanation.

## 6. Optimize for a target role when requested

When the user supplies a target role or job description, rerank demonstrated skills for that target. Do not add unsupported keywords merely because they appear in the role description. Separate strong matches, credible adjacent skills, and genuine gaps.

## 7. Protect private information

Do not reproduce source code, credentials, internal URLs, customer data, undisclosed vulnerabilities, or confidential names. Translate proprietary work into public-safe capability language. Never invent experience, business impact, scale, leadership, or production responsibility.
