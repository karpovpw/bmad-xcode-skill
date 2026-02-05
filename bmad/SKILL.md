---
name: bmad
description: "Full BMad Method personas, workflows, and tasks for Codex/Xcode. Use when the user prefixes a prompt with `bmad` (e.g., `bmad code-review`, `bmad scrum master`, `bmad help`), asks to run a BMad workflow/persona, or wants the BMad Method process."
---

# BMad Method (Codex Skill)

Use this skill to mirror the full BMad Method experience using the **project-local** BMad install.

Define `bmad_root` as:

`{project-root}/_bmad`

This skill intentionally does **not** bundle BMad sources. It always uses the BMad files inside the user’s project so updates to BMad take effect immediately.

## Quick Start

If the user prompt starts with `bmad`, treat the rest of the prompt as a BMad command and route it:

1. If it matches a **workflow**, run that workflow.
2. If it matches a **persona/agent**, load that persona and follow its activation steps.
3. If it matches a **task**, execute that task.
4. If ambiguous, show numbered choices with closest matches and ask the user to pick.
5. If blank or only `bmad`, default to the **BMad Master** persona.

**Xcode note:** Xcode Codex does not support slash commands. If any BMad docs mention `/bmad-help`, treat it as `bmad help`.

## Canonical Sources (Do Not Invent)

Always source names and metadata from these files at runtime:

- Workflows: `{bmad_root}/_config/workflow-manifest.csv`
- Personas/agents: `{bmad_root}/_config/agent-manifest.csv`
- Tasks: `{bmad_root}/_config/task-manifest.csv`
- Human-facing command map and friendly names: `{bmad_root}/_config/bmad-help.csv`

Never preload all workflows/personas. Only load the specific workflow or agent file needed for the user’s command.

## Routing Rules

### Workflow commands

Match against:

- `workflow-manifest.csv` `name` (e.g., `code-review`, `create-prd`)
- `bmad-help.csv` `name` or `command` fields (e.g., `bmad-bmm-code-review`)

Then open the workflow file at the `path` and follow its instructions exactly.

### Persona commands

Match against:

- `agent-manifest.csv` `name` (e.g., `sm`, `pm`, `dev`, `analyst`, `architect`)
- `displayName` or `title` (e.g., “Scrum Master”, “Product Manager”)

Then open the agent file at `path` and fully embody it. Follow all activation instructions in that file.

### Task commands

Match against:

- `task-manifest.csv` `name`
- `bmad-help.csv` task entries (e.g., `bmad-help`, `bmad-index-docs`)

Then open the task file at `path` and execute it.

## BMad Master Default

If the command is empty or ambiguous, default to the **BMad Master** persona:

`{bmad_root}/core/agents/bmad-master.md`

Follow its activation steps exactly. In particular, it requires loading `{bmad_root}/core/config.yaml` before any output.

## Listing & Help

If the user asks for help or wants a list:

- `bmad help` → run `{bmad_root}/core/tasks/help.md`
- `list workflows` → read workflow manifest and show a numbered list
- `list personas` → read agent manifest and show a numbered list
- `list tasks` → read task manifest and show a numbered list

## Xcode Slash-Command Translation

Some BMad workflows output slash-style commands (e.g., `/bmad:...`) that Xcode Codex cannot execute. When such output appears, translate it into an equivalent `bmad` command for the user.

Rules:
- Replace any leading `/bmad` (with or without `:`) with `bmad`.
- If the output contains a structured path like `/bmad:gds:workflows:create-story`, map it to `bmad create-story`.
- Preserve any referenced parameters (e.g., `story_key=...`) and present them as follow-up instructions for the user.

## Naming and Fidelity

Preserve all names and behaviors exactly as defined in the manifests and agent/workflow files. Do not rename, summarize, or “simplify” BMad concepts unless the user requests it.
