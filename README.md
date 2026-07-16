# Code to LinkedIn

Code to LinkedIn is an installable skill for Codex and Claude Code that turns code you actually contributed into a focused, ranked LinkedIn Skills section.

It analyzes one or more codebases, uses Git history and targeted blame to focus on your contributions, and produces:

- An immediate action set of 10–12 high-value skills
- A focused recommended priority order of 15–20 skills
- Evidence and category-appropriate demonstrated scope for material recommendations
- Lower-priority demonstrated skills to add later
- Incidental, generated, misleading, vendor-inflated, or overly granular technologies to leave out

## Why this exists

Selecting skills from dependency files creates noisy and misleading profiles. This skill maps actual contribution history to recognizable LinkedIn-style skill names, distinguishes application use from integration, operation, and architecture, and ranks only what the accessible code and Git evidence support.

The plugin deliberately has one scope: generate LinkedIn Skills recommendations from developer work. It does not read, compare, synchronize, or edit an existing LinkedIn profile.

## Install in Codex

Ask Codex to install the skill from this repository:

```text
Use $skill-installer to install the skill from
https://github.com/KieferPlender/code-to-linkedin/tree/main/skills/generate-linkedin-skills
```

Open the repository you want analyzed in a new task and invoke:

```text
$generate-linkedin-skills
```

The skill discovers likely Git identities and analysis defaults itself. When identity or repository scope cannot be inferred, it uses Codex's interactive question UI when available instead of requiring a boilerplate prompt.

## Install in Claude Code

Add this repository as a Claude Code marketplace and install the plugin:

```text
/plugin marketplace add KieferPlender/code-to-linkedin
/plugin install code-to-linkedin@code-to-linkedin
/reload-plugins
```

Invoke the skill from any project:

```text
/code-to-linkedin:generate-linkedin-skills
```

## Automatic intake

A bare invocation is enough. The skill automatically:

- treats the current repository as the initial scope;
- discovers likely identities from Git configuration and history;
- uses the conversation language;
- applies public-safe confidentiality defaults;
- generates a coherent general developer skills list unless target-role optimization is requested.

Codex or Claude Code asks only when identity or repository scope is genuinely ambiguous. The user never needs to prepare a metadata block.

## How the analysis works

The workflow combines:

- **Repository understanding**: architecture, product domains, infrastructure, tests, and delivery practices.
- **Contribution mapping**: Git history and targeted blame to identify what you introduced, evolved, or maintained.
- **Skill inference**: substantive technologies, engineering capabilities, tooling, and domain knowledge.
- **Scope classification**: distinguishes applied use, implementation and integration, platform operation, and architecture or ownership.
- **LinkedIn normalization**: removes overlapping recommendations, filters routine tools and micro-libraries, avoids unsupported labels and vendor claims, and builds a coherent developer story instead of maximizing keywords.

Private implementation details are translated into public-safe language rather than copied into the recommendations.

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
