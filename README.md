# Novem Skills

A growing collection of reusable skills for working on Novem projects.

This repository brings together the conventions, workflows, and practical knowledge that guide our work. Each skill captures a specific area of expertise in a format an AI coding assistant can follow, helping turn shared knowledge into consistent results across projects.

## What is a skill?

A skill is a focused set of instructions for a particular type of task. Its `SKILL.md` explains when to use it, how to approach the work, and which requirements to follow. Supporting files can provide references, examples, assets, or scripts when needed.

Skills can cover different subjects, from design and development to documentation, project setup, and recurring workflows. This collection will expand as new needs and practices emerge.

## Available skills

| Skill | Purpose | Status |
| --- | --- | --- |
| [Novem README](novem-readme/SKILL.md) | Create or revise practical, repository-grounded README files for Novem software projects. | ![Not ready](https://img.shields.io/badge/status-not%20ready-d97706) |
| [Novem Architecture](novem-architecture/SKILL.md) | Apply Novem's Swift architecture and naming conventions when writing or reviewing application code. | ![Ready to use](https://img.shields.io/badge/status-ready%20to%20use-2ea44f) |

## Using a skill

Choose the skill that matches your task and point your assistant to its `SKILL.md`. Include the project context, desired outcome, and any specific constraints so the instructions can be applied to your situation.

For example, this prompt applies the README skill to a project:

```text
Use the skill at `novem-readme/SKILL.md` to update this project's README.
Document the verified setup and normal usage path, and preserve correct existing content.
```

Each skill defines its own scope. Check its instructions and supporting resources before applying it to a new project.

## Repository structure

Each skill lives in its own folder at the repository root:

```text
skills/
|-- README.md
`-- <skill-name>/
    |-- SKILL.md
    |-- references/    # Optional supporting documentation
    |-- examples/      # Optional examples
    |-- assets/        # Optional files used by the skill
    `-- scripts/       # Optional helper scripts
```

## Adding a skill

Create a folder with a descriptive name and add a `SKILL.md` that clearly states the skill's purpose, when it applies, and the steps or standards to follow. Keep the main instructions focused and place detailed supporting material in the appropriate subfolders.

Add the skill to the table above so others can discover it. Keep instructions actionable, examples relevant, and references up to date as the collection grows.
