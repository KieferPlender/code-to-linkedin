---
name: generate-linkedin-skills
description: Automatically generate a ranked, copy-ready LinkedIn Skills section by analyzing one or more codebases and the user's actual Git contributions. Use when a user wants to discover which technical, engineering, tooling, domain, and professional skills their code demonstrates; choose the strongest skills to add or pin on LinkedIn; or refresh their LinkedIn skills from work they wrote, maintained, designed, or owned.
---

# Generate LinkedIn Skills

Turn the user's actual engineering work into a ranked LinkedIn skills list. Analyze code and contribution history deeply enough to distinguish substantive demonstrated skills from incidental repository exposure. Make the default result immediately usable in LinkedIn's Skills section.

## 1. Run an automatic interactive intake

Do not require the user to paste an intake template. Treat a bare invocation such as `$generate-linkedin-skills` or `/code-to-linkedin:generate-linkedin-skills` as sufficient.

Discover sensible defaults before asking anything:

- use the current repository as the default scope;
- inspect `git config user.name`, `git config user.email`, and `git shortlog -sne --all` to find likely Git identities;
- infer output language from the user's conversation;
- apply public-safe confidentiality defaults automatically;
- generate a general LinkedIn skills profile unless the user asks for role-specific optimization.

Use the product's native interactive question interface when available:

- in Codex, use `request_user_input` when that tool is available;
- in Claude Code, use `AskUserQuestion`;
- otherwise ask a concise conversational question.

Ask at most three intake questions in one round, and only when needed:

1. **Identity**: show likely Git identities for confirmation when more than one plausible author exists.
2. **Scope**: ask whether to include other accessible repositories only when multiple likely repositories are available or requested.
3. **Goal**: offer “General LinkedIn skills” as the default and “Optimize for a target role” as the alternative. Ask for the role only when the second option is selected.

Do not ask separately for confidentiality or output language unless the user requests unusual handling. Do not ask the user to type information that can be discovered from the workspace.

## 2. Identify the user's work

Understand each repository through high-signal files: documentation, manifests, architecture, service boundaries, schemas, infrastructure, CI/CD, representative implementation, and tests.

Use Git history to focus the analysis on work the user actually contributed:

```bash
git shortlog -sne --all
git log --author='<identity>' --date=short --format='%ad%x09%h%x09%s' --name-only
git log --follow --date=short --format='%ad%x09%an%x09%s' -- <path>
```

Use targeted `git blame` when it helps determine whether the user introduced, evolved, or maintained relevant implementation. Account for renamed identities, co-authorship, squashed commits, pair programming, shared accounts, and work missing from Git. Do not use commit or line counts as a skill score.

Before asking a post-scan question about any candidate skill, complete a targeted evidence audit for that skill. Inspect the relevant implementation, configuration, tests, documentation and runbooks, infrastructure, CI/CD, commit history, and blame. Search adjacent files that can distinguish how the technology was used, but do not scan unrelated parts of the repository. Record the most precise conclusion the available evidence supports.

Never ask the user to interpret evidence that is available in the accessible codebases or Git history. Questions are for facts that cannot be discovered there, such as responsibilities performed in a live environment, work in an inaccessible repository, or collaboration that Git does not capture.

## 3. Extract demonstrated skills

Generate skill candidates from substantive evidence such as:

- implementation in a language or framework;
- design of APIs, data models, integrations, or system boundaries;
- debugging, performance, security, reliability, or concurrency work;
- tests, automation, CI/CD, observability, deployments, and migrations;
- cloud, infrastructure, databases, messaging, and data pipelines;
- architecture, technical writing, developer experience, and reusable tooling;
- repeated ownership of product or domain problems;
- review, mentoring, standards, or technical leadership when supported by history or user context.

Do not add a technology merely because it appears in a dependency, lockfile, generated file, or neighboring module. Look for meaningful implementation, configuration, tests, maintenance, or explicit confirmation.

For each candidate, assess:

| Factor | Meaning |
|---|---|
| Depth | complexity and sophistication of demonstrated use |
| Frequency | recurring work versus a one-off touch |
| Recency | how recently the skill was exercised |
| Ownership | implementation, design, maintenance, or end-to-end responsibility |
| Breadth | use across projects, layers, or problem types |
| Target relevance | value for the user's desired next role, when provided |

Use qualitative ratings—strong, moderate, emerging, or uncertain—rather than fake numerical precision.

### Classify the demonstrated capability

Distinguish the level of demonstrated work instead of treating technology ownership as a yes-or-no question:

1. **Incidental exposure**: dependency, generated file, copied configuration, or neighboring code only. Omit it.
2. **Application use**: implemented product behavior with the technology or deployed an application to it.
3. **Configuration and integration**: designed or maintained meaningful configuration, automation, integrations, or delivery workflows.
4. **Administration and operation**: provisioned, secured, upgraded, observed, debugged, or recovered the underlying service or platform.
5. **Architecture and ownership**: made recurring cross-cutting design decisions or led the capability across systems.

Choose the most precise LinkedIn skill label and ranking supported by the highest demonstrated level. A user does not need to administer a platform for the base technology to be a valid skill. Do not inflate application use into platform administration, and do not underrank substantial application delivery merely because cluster or service provisioning lives elsewhere.

For Kubernetes, determine the level from evidence rather than asking the user to classify it:

- **Application delivery evidence** includes authored or maintained workload manifests, Helm charts or values, Kustomize overlays, Deployments, StatefulSets, Services, Jobs, probes, resources, application configuration, and CI/CD rollout or rollback workflows.
- **Platform operation evidence** includes cluster or node-pool provisioning, upgrades, add-ons, operators or controllers, cluster-wide RBAC or policy, admission control, ingress, networking, storage, autoscaling, observability, backup and recovery, operational tooling, incident fixes, and runbooks. Inspect infrastructure-as-code and platform repositories when they are accessible.
- Attribute that evidence through Git. If only application delivery is demonstrated, `Kubernetes` can still be ranked according to its depth, frequency, and recency, while claims such as `Kubernetes Administration` or platform ownership are excluded. If platform-operation evidence is demonstrated, elevate the ranking and the associated operational or platform-engineering capabilities.

## 4. Normalize for LinkedIn

Convert repository-specific evidence into recognizable LinkedIn skill names:

- prefer common market terminology over internal component names;
- merge aliases and duplicates, such as `Postgres` and `PostgreSQL`;
- separate a broad skill from a specific tool only when both are independently demonstrated;
- include capabilities such as API Design, Distributed Systems, Test Automation, or CI/CD alongside technologies;
- keep confidential business domains generic while preserving useful expertise;
- avoid vague filler such as “Hard Working,” “Problem Solving,” or “Technology” unless the user explicitly wants soft skills.

Rank skills by demonstrated strength first, then target-role relevance and recency. Do not rank alphabetically.

## 5. Ask only non-discoverable follow-up questions

Produce the initial ranked result from repository evidence without waiting for post-scan answers. Most runs should require no follow-up beyond any necessary identity or scope confirmation.

Use the native question interface only when all of these conditions are true:

1. the targeted evidence audit is complete;
2. the answer is not discoverable from any accessible codebase or Git history;
3. two or more interpretations remain genuinely plausible; and
4. the answer would add, remove, or substantially reorder a skill.

Ask the question after the copy-ready result as an optional refinement. State what the evidence already establishes and ask only for the missing off-repository fact. Valid examples include whether the user also operated production infrastructure whose configuration is in an inaccessible repository, handled incidents that left no repository artifacts, or mentored engineers outside Git-visible work.

Do not ask the user to choose between skill labels when the evidence can resolve the distinction. For example, authentication implementation should be classified from the changed flows, authorization models, threat controls, tests, and ownership history; model integration should be distinguished from model training by inspecting the implementation and dependencies. Update the list from any answer without discarding the repository evidence.

## 6. Produce copy-ready output

Return these sections by default:

1. **Top 5 skills to feature**: the strongest and most strategically useful skills, ordered for LinkedIn pinning.
2. **Skills to add**: 20-40 normalized LinkedIn skill names in ranked order, formatted one per line for easy entry.
3. **Why these skills**: a compact table mapping each top skill to contribution evidence and strength.
4. **Emerging or uncertain skills**: plausible skills that need confirmation or more evidence before adding.
5. **Skills to omit**: repository technologies that were incidental, generated, or not meaningfully used by the user.

When helpful, also group the ranked list into:

- languages and frameworks;
- architecture and engineering capabilities;
- data, cloud, infrastructure, and tooling;
- quality, security, reliability, and delivery;
- product, domain, and leadership skills.

Do not bury the copy-ready list beneath a long methodology explanation. Lead with the recommendations.

## 7. Optional LinkedIn writing

Only when the user requests it, use the skills analysis to draft:

- LinkedIn headline options;
- an About section;
- job-title suggestions;
- experience descriptions and accomplishment bullets.

Keep these optional outputs consistent with the ranked skills rather than rerunning an unrelated career assessment.

## 8. Protect private information

Do not reproduce source code, credentials, internal URLs, customer data, undisclosed vulnerabilities, or confidential names. Translate proprietary technology and domain work into public-safe skill terminology. Never invent experience with a skill; mark ambiguous candidates for confirmation.
