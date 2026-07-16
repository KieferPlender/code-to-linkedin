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

## 4. Normalize for LinkedIn

Convert repository-specific evidence into recognizable LinkedIn skill names:

- prefer common market terminology over internal component names;
- merge aliases and duplicates, such as `Postgres` and `PostgreSQL`;
- separate a broad skill from a specific tool only when both are independently demonstrated;
- include capabilities such as API Design, Distributed Systems, Test Automation, or CI/CD alongside technologies;
- keep confidential business domains generic while preserving useful expertise;
- avoid vague filler such as “Hard Working,” “Problem Solving,” or “Technology” unless the user explicitly wants soft skills.

Rank skills by demonstrated strength first, then target-role relevance and recency. Do not rank alphabetically.

## 5. Ask focused follow-up questions

After the initial scan, use the same native question interface to ask only about gaps that could add, remove, or substantially reorder skills. Ground questions in observed work, for example:

- “The deployment configuration suggests Kubernetes, but your commits only touch application manifests. Did you operate the clusters or only deploy workloads to them?”
- “You repeatedly changed authentication flows. Should this be represented as Identity and Access Management, Application Security, or both?”
- “The repository shows model integrations but not model training. Is Generative AI the right skill, rather than Machine Learning?”

Update the list from the answers without discarding the repository evidence.

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
