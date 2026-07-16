---
name: generate-linkedin-skills
description: Generate or synchronize a developer's LinkedIn Skills section from one or more codebases and their actual Git contributions. Use when a user wants to create a LinkedIn-ready skills list, compare demonstrated skills with an existing LinkedIn profile, decide what to add, keep, reorder, demote, or remove, associate skills with Experience entries, optimize for a target role, or use the evidence to draft a consistent developer headline, About section, or experience description.
---

# Generate LinkedIn Skills

Turn verified engineering work into a focused, LinkedIn-native action plan. Analyze code and contribution history deeply enough to distinguish substantive capability from repository exposure. Lead with changes the user can make, not a methodology report.

## 1. Select the workflow automatically

Treat a bare invocation such as `$generate-linkedin-skills` or `/code-to-linkedin:generate-linkedin-skills` as sufficient.

Use one of two modes:

- **Create mode**: build a new LinkedIn Skills section when the current profile is unavailable.
- **Sync mode**: compare demonstrated skills with the user's current LinkedIn profile and recommend exact additions, ordering, associations, demotions, and removals.

Enter sync mode automatically when the user supplies a profile export, PDF, screenshot, pasted profile, or accessible profile page. Enter create mode automatically only when the user explicitly asks to create a new profile or says no current profile exists.

For a bare invocation with neither profile context nor an explicit mode, ask this before scanning repositories: “Do you want to improve an existing LinkedIn profile or create a new Skills plan from your code?” Offer:

- **Sync existing profile**: read the current profile and produce an exact change plan.
- **Create from code**: propose a new Skills section without reading LinkedIn.

Do not silently choose create mode, and do not begin the repository scan until the user selects a mode.

Discover defaults before asking anything:

- use the current repository as the initial scope;
- inspect `git config user.name`, `git config user.email`, and `git shortlog -sne --all` for likely identities;
- infer output and profile language from the conversation or supplied profile;
- apply public-safe confidentiality defaults;
- generate a coherent general developer profile unless the user asks for role-specific optimization.

Use the product's native question interface when available: `request_user_input` in Codex, `AskUserQuestion` in Claude Code, or a concise conversational question otherwise.

Ask only for a decision that cannot be discovered. Combine necessary choices into one intake round, with no more than three questions:

1. choose create or sync mode when it is not already clear;
2. confirm identity only when multiple plausible authors remain;
3. confirm scope only when multiple likely repositories are available or requested.

Use general positioning by default. Ask about a target role only when the user requests role-specific optimization.

If the user selects sync mode but no profile is accessible, ask how to access it. Offer an available user-authorized browser first, then an attached export/PDF, then pasted current skills and Experience entries. Do not require a metadata template. Do not fall back to create mode unless the user chooses to continue without profile access.

## 2. Read the current LinkedIn state in sync mode

Use only information the user supplied or made accessible through an authorized tool. Capture, when available:

- profile language;
- current skill labels and display order;
- visible endorsement signals;
- Experience titles, organizations, dates, and descriptions;
- existing skill-to-Experience associations;
- visible Featured items or Connected Apps that already substantiate relevant skills.

Do not change the profile while gathering state. If the current profile cannot be read reliably, continue in create mode and clearly say that the result is a proposed profile rather than a diff.

When current LinkedIn limits, fields, or UI behavior materially affect the result and web access exists, verify them against official LinkedIn Help or Microsoft Learn documentation.

## 3. Identify the user's work

Inspect high-signal repository areas first: documentation, manifests, architecture, service boundaries, schemas, infrastructure, CI/CD, representative implementation, and tests. Avoid unrelated repository-wide reading.

Use Git history to attribute relevant work:

```bash
git shortlog -sne --all
git log --author='<identity>' --date=short --format='%ad%x09%h%x09%s' --name-only
git log --follow --date=short --format='%ad%x09%an%x09%s' -- <path>
```

Use targeted `git blame` to determine whether the user introduced, evolved, or maintained relevant implementation. Account for renamed identities, co-authorship, squashed commits, pair programming, shared accounts, and work missing from Git. Do not use commit or line counts as a skill score.

Before asking a post-scan question about a candidate skill, complete a targeted evidence audit. Inspect its relevant implementation, configuration, tests, documentation and runbooks, infrastructure, CI/CD, history, blame, and adjacent files. Record the most precise conclusion the evidence supports.

Never ask the user to interpret accessible code or Git evidence. Reserve questions for facts that cannot be discovered there, such as live operational work, inaccessible repositories, incident response, mentoring, or collaboration absent from Git.

## 4. Infer demonstrated skills and scope

Generate candidates from substantive evidence, including:

- languages, frameworks, APIs, data models, integrations, and system boundaries;
- debugging, performance, security, reliability, concurrency, and migrations;
- tests, automation, CI/CD, observability, deployment, cloud, and infrastructure;
- databases, messaging, data pipelines, developer tooling, and documentation;
- repeated ownership of product or domain problems;
- architecture, standards, review, mentoring, or technical leadership when supported.

Do not add a technology merely because it appears in a dependency, lockfile, generated file, template, or neighboring module.

Assess evidence strength separately from demonstrated scope.

Evidence strength:

- **Strong**: deep, repeated, recent, or broad contribution evidence.
- **Moderate**: meaningful implementation or maintenance with narrower depth or frequency.
- **Emerging**: real but early or limited demonstrated use.
- **Uncertain**: attribution or interpretation remains unresolved after targeted inspection.

Demonstrated scope:

- **Incidental exposure**: dependency, generated file, copied configuration, or neighboring code only. Omit it.
- **Applied use**: used the technology to implement real product behavior.
- **Implementation and integration**: designed or maintained meaningful implementation, configuration, automation, integrations, or delivery workflows.
- **Administration and operation**: provisioned, secured, upgraded, observed, debugged, or recovered an operable service or platform.
- **Architecture and ownership**: made recurring cross-cutting design decisions or led the capability across systems.

A user does not need to administer a platform for its base technology to be a valid skill. Do not inflate application use into administration, and do not label a demonstrated skill “uncertain” merely because broader operational scope is absent.

Express scope with a category-appropriate phrase instead of a number. Examples include `Advanced Python implementation`, `API architecture and ownership`, `PostgreSQL application integration`, `CI/CD workflow configuration`, `Kubernetes application delivery`, and `Kubernetes cluster operation`. Use **administration and operation** only for an operable platform, service, database, runtime, network, or infrastructure system. Never describe a programming language, application framework, library, testing tool, engineering technique, or broad capability such as Backend Development as “administration and operation.”

For Kubernetes, treat authored workload manifests, Helm or Kustomize configuration, probes, resources, application configuration, and rollout workflows as application-delivery evidence. Treat cluster provisioning, upgrades, node pools, add-ons, operators, cluster-wide RBAC or policy, admission control, ingress, networking, storage, autoscaling, observability, recovery, incident fixes, and operational runbooks as platform-operation evidence. Rank `Kubernetes` from the demonstrated depth; add administration or platform-engineering claims only when their evidence exists.

## 5. Normalize and rank for LinkedIn

Translate evidence into recognizable, useful LinkedIn labels:

- prefer market-standard terminology over internal component names;
- merge aliases and duplicates;
- include a broad capability and a specific technology only when each adds distinct value;
- never replace a compatible protocol or API with a vendor-branded skill unless the user's attributed work demonstrates that vendor's actual service; prefer `Object Storage` over `Amazon S3` when only an S3-compatible implementation is evidenced;
- keep confidential domains generic while preserving useful expertise;
- exclude vague filler, language primitives, routine baseline tools, tiny utilities, and low-signal micro-libraries unless important to the target role;
- exclude generated artifacts and repository-wide technologies the user did not meaningfully use.

When a user-authorized LinkedIn browser is available, validate proposed labels by reading LinkedIn's skill suggestions without selecting or saving them. Use the exact suggested label. When validation is unavailable, use the most likely common label and flag only genuinely uncertain labels as unverified.

Rank by:

1. demonstrated strength and scope;
2. coherence with the developer identity the evidence supports;
3. market recognizability and usefulness;
4. recency and breadth;
5. target-role relevance when supplied.

For a general profile, build a coherent story rather than maximizing keyword count. Balance core identity, specialist differentiators, supporting technologies, and delivery or platform capabilities. Recommend 15–25 primary skills by default, with 8–15 in the immediate action set and no more than 10 optional additions.

Do not claim that LinkedIn has a special “top five pinning” model. Produce a desired display order.

## 6. Produce a LinkedIn-native action plan

Lead with the plan. Keep evidence compact and place methodology last or omit it.

### Create mode

Return:

1. **Add now**: 8–15 ranked skills.
2. **Desired display order**: the complete focused list of 15–25 labels, one per line.
3. **Experience associations**: which Experience entry should be selected for each skill when known.
4. **Why the leading skills belong**: compact evidence for the highest-priority skills.
5. **Add later**: demonstrated but less important skills.
6. **Do not add**: incidental, overly granular, misleading, or weakly attributed technologies.
7. **Positioning line**: one sentence describing the coherent developer identity represented by the list.

Use this table for the immediate action set:

| Order | LinkedIn skill | Associate with | Demonstrated scope | Evidence strength | Why it belongs |
|---:|---|---|---|---|---|

Use the exact visible Experience title when the profile is known. In create mode, write `Needs profile mapping` when the relevant role cannot be identified; never invent an employer or title.

### Sync mode

Return:

1. **Change summary**: counts for add, move up, keep, demote, and remove.
2. **Recommended changes** using the table below.
3. **Desired display order** after the changes.
4. **Experience associations** to add or correct.
5. **Evidence for material changes**.
6. **Profile alignment**: conflicts or gaps between leading skills and the visible headline, About, Experience, Featured items, or Connected Apps.
7. **Optional refinements** that depend on off-repository facts.

| Action | LinkedIn skill | Desired order | Associate with | Demonstrated scope | Reason |
|---|---|---:|---|---|---|

Use only these actions: `Add`, `Move up`, `Keep`, `Demote`, and `Remove`. Recommend `Remove` only when the current profile is known and the skill is misleading, obsolete, duplicated, or harmful to positioning. Prefer `Demote` when endorsements or useful history may be lost.

Do not mix demonstrated capability with missing higher-level scope. For example, list `Kubernetes` as demonstrated application delivery instead of placing it under “uncertain” because cluster administration is not shown.

## 7. Ask only non-discoverable follow-up questions

Produce the initial plan without waiting for post-scan answers. Most runs should require no follow-up beyond necessary identity, scope, goal, or profile-access confirmation.

Ask an optional refinement question only when all conditions hold:

1. the targeted evidence audit is complete;
2. the answer is absent from all accessible repositories and profile sources;
3. multiple interpretations remain genuinely plausible; and
4. the answer would materially change the plan.

State what the evidence already establishes and ask only for the missing off-repository fact. Never ask the user to choose a label that repository or profile evidence can resolve.

## 8. Apply changes only with explicit approval

Planning is the default. Do not mutate a LinkedIn profile unless the user explicitly asks to apply the plan and an authorized browser or official API is available.

Before applying:

1. show the exact add, reorder, association, demotion, and removal operations;
2. identify destructive actions and possible endorsement loss;
3. obtain explicit approval for the displayed operations;
4. preserve the profile language unless the user requests a change.

Apply only approved operations. Re-read the page after each save or coherent batch, verify the resulting state, and stop on any mismatch. Never scrape LinkedIn, bypass access controls, extract session cookies, request credentials, use unofficial private endpoints, or assume that an MCP server grants authorization. Use an MCP integration only when it relies on an official API with the required approved scopes.

## 9. Draft the rest of the developer profile when requested

Use the same evidence and positioning to produce any requested profile package:

- three concise headline options;
- an About section;
- accurate job-title suggestions;
- Experience descriptions and accomplishment bullets;
- Featured-project suggestions.

Keep terminology consistent with the selected skills and map important skills to the relevant Experience entry. Do not invent business impact, scale, leadership, or production responsibility; ask about non-discoverable results only when they would materially improve the writing.

## 10. Protect private information

Do not reproduce source code, credentials, internal URLs, customer data, undisclosed vulnerabilities, or confidential names. Translate proprietary work into public-safe capability language. Never invent experience, and never expose repository evidence merely to make the explanation sound specific.
