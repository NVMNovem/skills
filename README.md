# Novem Skills

A growing collection of reusable skills for working on Novem projects.

This repository brings together the conventions, workflows, and practical knowledge that guide our work. Each skill captures a specific area of expertise in a format an AI coding assistant can follow, helping turn shared knowledge into consistent results across projects.

## What is a skill?

A skill is a focused set of instructions for a particular type of task. Its `SKILL.md` explains when to use it, how to approach the work, and which requirements to follow. Supporting files can provide references, examples, assets, or scripts when needed.

Skills can cover different subjects, from design and development to documentation, project setup, and recurring workflows. This collection will expand as new needs and practices emerge.

## Available skills

| Skill | Purpose | Status |
| --- | --- | --- |
| [Novem Architecture](novem-architecture/SKILL.md) | Apply Novem's Swift architecture and naming conventions when writing or reviewing application code and tests. | ![Ready to use](https://img.shields.io/badge/status-ready%20to%20use-2ea44f) |
| [Novem README](novem-readme/SKILL.md) | Create or revise practical, repository-grounded README files for Novem software projects. | ![Not ready](https://img.shields.io/badge/status-not%20ready-d97706) |
| [Novem Skills](novem-skills/SKILL.md) | Create or update reusable skills in the Novem skills repository, including their references and README usage prompts. | ![Not ready](https://img.shields.io/badge/status-not%20ready-d97706) |
| [Novem Swift Packages](novem-swift-packages/SKILL.md) | Design, create, or maintain Novem Swift packages, including their manifests, target boundaries, dependencies, tools, and tests. | ![Not ready](https://img.shields.io/badge/status-not%20ready-d97706) |

## Usage

Choose a skill below and copy its prompt into your AI assistant. Replace the bracketed task with your desired outcome, project context, and any specific constraints. Each skill has its own heading so you can link directly to its usage section on GitHub.

### Architecture

Use this prompt when writing or reviewing Swift application code and tests with Novem's architecture and naming conventions, including test folders and localization resource paths that mirror their owning modules and components.

```text
Read and follow the Novem Architecture skill:
https://github.com/NVMNovem/skills/blob/main/novem-architecture/SKILL.md

Also read the linked reference files relevant to the task, resolving their paths relative to the skill's folder on GitHub. Apply the skill's Swift architecture and naming conventions to the code and tests you write or review. Mirror each tested module's folder tree in its test module and append Tests to corresponding source filenames. Place localization catalogs under Resources using the owning component's relative folder path and table name. Preserve persisted identifiers, and ask for clarification when conflicting project patterns leave the intended convention unclear.

Task: [Describe what to build, change, or review in this project.]
```

### README

This skill is a starter and is not ready for general use yet. Use this prompt to try it when creating or updating a project's README.

```text
Read and follow the Novem README skill:
https://github.com/NVMNovem/skills/blob/main/novem-readme/SKILL.md

Also read the linked reference files relevant to the task, resolving their paths relative to the skill's folder on GitHub. Ground the README in the repository's actual setup and usage, and preserve correct existing content.

Task: [Describe the project and what to create or update in its README.]
```

### Skills

This skill is a starter and is not ready for general use yet. Use this prompt to try it when creating or updating a skill in this repository.

```text
Read and follow the Novem Skills skill:
https://github.com/NVMNovem/skills/blob/main/novem-skills/SKILL.md

Also read the linked reference files relevant to the task, resolving their paths relative to the skill's folder on GitHub. Follow the repository's skill structure and writing conventions. Whenever creating or editing a skill, also update its usage example under Usage in the repository's README, keeping the skill listing in sync and Usage subsections alphabetical.

Task: [Describe the skill to create or update and the conventions it should capture.]
```

### Swift Packages

This skill is a starter and is not ready for general use yet. Use this prompt when creating a Swift package or changing its manifest, products, targets, dependencies, tools, or package-level tests.

```text
Read and follow the Novem Swift Packages skill:
https://github.com/NVMNovem/skills/blob/main/novem-swift-packages/SKILL.md

Also read the linked reference files relevant to the task, resolving their paths relative to the skill's folder on GitHub. Preserve deliberate compatibility constraints and unrelated project conventions. Keep the package graph cohesive and acyclic, expose only intended products, isolate maintainer tooling and host-only dependencies, and verify the applicable package, target, platform, plugin, or generated-output paths.

Task: [Describe the Swift package to create, change, or review and its supported consumers and platforms.]
```

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

Add the skill to the table above and give it a subsection under Usage with a copyable prompt that links to its `SKILL.md` on GitHub. Keep instructions actionable, examples relevant, and references up to date as the collection grows.
