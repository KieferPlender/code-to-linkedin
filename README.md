# Code to LinkedIn

Code to LinkedIn is an installable skill for Codex and Claude Code that turns verified code contributions into credible LinkedIn profile copy.

It inspects selected parts of a codebase, uses targeted Git history when authorship matters, asks questions that code cannot answer, and drafts:

- LinkedIn headline options
- Conservative, market-standard, and aspirational job-title options
- A first-person About section
- Experience descriptions and bullets
- A prioritized skills list
- Verification notes for claims that need confirmation

## Why this exists

A repository can reveal technologies, architecture, testing practices, and areas of responsibility. It cannot reliably prove business impact, seniority, sole ownership, or production scale. This workflow keeps those boundaries explicit so the resulting profile is useful without becoming fictional.

## Install in Codex

Ask Codex to install the skill directly from this repository:

```text
Use $skill-installer to install the skill from
https://github.com/KieferPlender/code-to-linkedin/tree/main/skills/build-linkedin-profile
```

The skill is installed into your personal Codex skills directory and becomes available on your next turn. Open the repository you want analyzed and prompt:

```text
Use $build-linkedin-profile to analyze this repository and help draft my LinkedIn profile.

My Git author name/email:
Target roles:
Repositories in scope:
Details that must remain confidential:
Output language:
```

Codex may ask follow-up questions before it drafts public-facing copy.

## Install in Claude Code

Add this repository as a Claude Code marketplace and install the plugin:

```text
/plugin marketplace add KieferPlender/code-to-linkedin
/plugin install code-to-linkedin@code-to-linkedin
/reload-plugins
```

Invoke the installed skill from any project:

```text
/code-to-linkedin:build-linkedin-profile
```

The plugin is installed in your selected Claude Code scope; it does not need to be copied into each repository.

## Local development

Test the Claude Code plugin directly from a checkout:

```bash
claude --plugin-dir .
```

Then invoke `/code-to-linkedin:build-linkedin-profile`.

## Evidence and privacy model

The workflow distinguishes four evidence classes:

- **Verified contribution**: attributable through Git history or user confirmation.
- **Repository exposure**: visible in a repository, but personal ownership is unclear.
- **User-reported context**: provided by the user and not independently visible in code.
- **Inference**: plausible, but requires confirmation.

It is designed to avoid publishing secrets, internal URLs, customer data, confidential project names, proprietary implementation details, or unsupported metrics. Review every generated claim before publishing it.

## Repository structure

```text
.
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
└── skills/build-linkedin-profile/
    ├── SKILL.md
    └── agents/openai.yaml
```

## Development

Validate the Codex skill and plugin using the `quick_validate.py` and `validate_plugin.py` helpers supplied with Codex's built-in skill/plugin creator tools. Validate the Claude Code package with `claude plugin validate .` when Claude Code is installed.

## License

MIT
