# BMad Method for Xcode >=26.3 (Codex + Claude)

This repo packages the BMad Method as a skill that can be used inside Xcode agents configuration.

The skill does **not** bundle BMad sources. It always reads from the BMad files in your **project** (`{project-root}/_bmad`), so when BMad updates, you’re automatically current.

## What’s inside

- `bmad/` — the Codex/Claude skill folder
- `bmad/SKILL.md` — routing and usage rules
- No bundled BMad resources. The skill reads from your project’s `./_bmad` directory.

## How Xcode skills work (short version)

Xcode’s agentic coding looks for a `codex` or `ClaudeAgentConfig` directory and loads skill folders inside it. A skill is just a folder containing a `SKILL.md` file that tells Codex or Claude how to use the skill.

This skill routes prompts that begin with `bmad` to the correct BMad workflow/persona/task.

## Prerequisite: Install BMad in your project

Before using the skill, install BMad in the project you’re working on. The BMad install must create:

`{project-root}/_bmad`

The skill reads all manifests, personas, workflows, and tasks from that location.

## Install (Codex)

1. Create the Codex skills directory if it doesn’t exist:

```bash
mkdir -p ~/Library/Developer/Xcode/CodingAssistant/codex/skills
```

2. Copy the `bmad` folder into it:

```bash
cp -R ./bmad ~/Library/Developer/Xcode/CodingAssistant/codex/skills/
```

3. (Recommended) Add `AGENTS.md` next to the skill folder to enforce BMad routing:

```bash
cat <<'EOF' > ~/Library/Developer/Xcode/CodingAssistant/codex/AGENTS.md
# Xcode Codex Agent Policy

This Codex agent must operate specifically through the BMad Method.

Rules:
1. Treat any prompt prefixed with `bmad` as a BMad command and route it to the matching workflow, persona, or task.
2. Xcode Codex does not support slash commands. If any BMad docs mention `/bmad-help`, treat it as `bmad help`.
3. If intent is unclear, default to the BMad Master persona and follow its activation steps.
EOF
```

## Install (Claude)

1. Create the Claude skills directory if it doesn’t exist:

```bash
mkdir -p ~/Library/Developer/Xcode/CodingAssistant/ClaudeAgentConfig/skills
```

2. Copy the `bmad` folder into it:

```bash
cp -R ./bmad ~/Library/Developer/Xcode/CodingAssistant/ClaudeAgentConfig/skills/
```

3. (Recommended) Add `AGENTS.md` next to the skill folder to enforce BMad routing:

```bash
cat <<'EOF' > ~/Library/Developer/Xcode/CodingAssistant/ClaudeAgentConfig/AGENTS.md
# Xcode Claude Agent Policy

This Claude agent must operate specifically through the BMad Method.

Rules:
1. Treat any prompt prefixed with `bmad` as a BMad command and route it to the matching workflow, persona, or task.
2. Xcode Claude does not support slash commands. If any BMad docs mention `/bmad-help`, treat it as `bmad help`.
3. If intent is unclear, default to the BMad Master persona and follow its activation steps.
EOF
```

## Where the BMad files live (explicit path)

Inside each project:

`{project-root}/_bmad`

## Usage examples

- `bmad help`
- `bmad code-review`
- `bmad scrum master`
- `bmad create-prd`
- `bmad quick-spec`
- `bmad bmad-master`

## Notes

- Xcode agents does not support slash commands. Use `bmad help` instead of `/bmad-help`.
- All names and behaviors are sourced from the BMad manifests; nothing is renamed or simplified.
