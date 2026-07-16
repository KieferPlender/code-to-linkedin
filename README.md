# Code to LinkedIn

Code to LinkedIn is an installable skill for Codex and Claude Code that automatically generates your LinkedIn Skills section from code you actually contributed.

It analyzes one or more codebases, uses Git history and targeted blame to focus on your contributions, and produces:

- The top five skills to feature on LinkedIn
- A ranked, copy-ready list of 20-40 LinkedIn skills
- Technical capabilities as well as languages, frameworks, platforms, and tools
- Evidence explaining why the strongest skills were selected
- Emerging skills that need confirmation
- Repository technologies to omit because you did not meaningfully use them

LinkedIn headlines, About text, job titles, and experience descriptions are available as optional follow-up outputs rather than the primary workflow.

## Why this exists

Manually maintaining a LinkedIn skills list is tedious, and selecting skills from repository dependencies creates noisy or misleading results. This skill maps your actual contribution history to recognizable LinkedIn skill names, ranks them by depth, frequency, recency, ownership, and target-role relevance, and asks focused questions only when the evidence is ambiguous.

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
- **Evidence-led clarification**: focused questions that determine whether an ambiguous skill should be added, removed, or reranked.

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
