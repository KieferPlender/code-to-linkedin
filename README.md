# Code to LinkedIn

Code to LinkedIn is an installable skill for Codex and Claude Code that turns code you actually contributed into a focused, actionable LinkedIn Skills plan.

It supports two workflows:

- **Create** a new developer Skills section from one or more codebases.
- **Sync** demonstrated skills against an existing LinkedIn profile and decide what to add, keep, move up, demote, remove, and associate with each Experience entry.

The default result includes:

- An immediate action set of 8-15 high-value skills
- A focused desired display order of 15-25 skills
- Skill-to-Experience associations for LinkedIn's profile workflow
- Evidence and demonstrated capability level for material recommendations
- A concise positioning check so the skills support a coherent developer profile
- Lower-priority skills to add later
- Incidental, generated, misleading, or overly granular technologies to leave out

Headline options, About text, job-title suggestions, Experience descriptions, and Featured-project ideas are available from the same evidence when requested.

## Why this exists

Manually maintaining a LinkedIn profile is tedious, and selecting technologies from dependency files creates noisy or misleading results. This skill maps contribution history to recognizable LinkedIn skills, distinguishes application use from integration, operation, and platform ownership, and exhausts accessible code and Git evidence before asking about genuinely off-repository work.

It produces a LinkedIn-shaped change plan rather than a generic keyword dump. When an existing profile is available, it preserves useful history and endorsements by preferring reordering or demotion over unnecessary deletion.

## Install in Codex

Ask Codex to install the skill directly from this repository:

```text
Use $skill-installer to install the skill from
https://github.com/KieferPlender/code-to-linkedin/tree/main/skills/generate-linkedin-skills
```

The skill is installed into your personal Codex skills directory and becomes available on your next turn. Open the repository you want analyzed and invoke:

```text
$generate-linkedin-skills
```

The skill discovers likely Git identities and analysis defaults itself. When a decision cannot be inferred, it uses Codex's interactive question UI when available instead of requiring a boilerplate prompt.

## Install in Claude Code

Add this repository as a Claude Code marketplace and install the plugin:

```text
/plugin marketplace add KieferPlender/code-to-linkedin
/plugin install code-to-linkedin@code-to-linkedin
/reload-plugins
```

Invoke the installed skill from any project:

```text
/code-to-linkedin:generate-linkedin-skills
```

The plugin is installed in your selected Claude Code scope; it does not need to be copied into each repository.

## Interactive intake

A bare invocation is enough. The skill automatically:

- treats the current repository as the initial scope;
- discovers likely identities from Git configuration and history;
- uses the conversation language;
- applies public-safe confidentiality defaults;
- generates a general skills ranking unless role-specific optimization is requested.

When choices remain, Codex uses its native question interface when available and Claude Code uses `AskUserQuestion`. The user should never need to prepare a metadata block before running the skill.

## Create and sync modes

Create mode needs only the repositories. It produces a new LinkedIn Skills section with a focused action set, desired order, Experience associations when inferable, and evidence for the leading recommendations.

Sync mode also reads the current profile from an attached export, PDF, pasted profile, or an accessible user-authorized browser. It produces an exact diff using `Add`, `Move up`, `Keep`, `Demote`, and `Remove` actions. If the current profile cannot be read, the skill still returns a useful create-mode result.

The skill does not edit LinkedIn by default. If the user explicitly requests application and an authorized browser or official API is available, it previews the exact operations, warns before destructive changes, requires approval, and verifies the saved state. It does not scrape LinkedIn, extract session cookies, request credentials, or use unofficial private APIs.

This plugin does not bundle a LinkedIn MCP server or require LinkedIn credentials. MCP can expose an approved integration to an agent, but it does not grant LinkedIn permissions by itself. The core create workflow remains fully useful without LinkedIn access.

## Local development

Test the Claude Code plugin directly from a checkout:

```bash
claude --plugin-dir .
```

Then invoke `/code-to-linkedin:generate-linkedin-skills`.

## How the analysis works

The workflow combines:

- **Repository understanding**: architecture, product domains, infrastructure, tests, and delivery practices.
- **Contribution mapping**: Git history and targeted blame to identify what you introduced, evolved, or maintained.
- **Skill inference**: substantive technologies, engineering capabilities, tooling, domain knowledge, and professional skills.
- **Capability classification**: distinguishes application use, configuration and integration, platform operation, and architecture or ownership from the code and its history.
- **LinkedIn normalization**: filters routine tools and micro-libraries, validates exact skill labels when possible, and builds a coherent developer story instead of maximizing keywords.
- **Profile synchronization**: maps recommendations to current skills, display order, and Experience entries when the current profile is available.
- **Evidence-led clarification**: asks only about material responsibilities that cannot be discovered in accessible repositories or profile sources, without blocking the initial result.

Private implementation details are translated into public-safe language rather than copied into the profile.

## Repository structure

```text
.
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
└── skills/generate-linkedin-skills/
    ├── SKILL.md
    └── agents/openai.yaml
```

## Development

Validate the Codex skill and plugin using the `quick_validate.py` and `validate_plugin.py` helpers supplied with Codex's built-in skill/plugin creator tools. Validate the Claude Code package with `claude plugin validate .` when Claude Code is installed.

## License

MIT
