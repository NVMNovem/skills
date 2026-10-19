---
name: novem-skills
description: Create or update reusable skills in the Novem skills repository, including their references and README usage prompts.
---

# Novem Skills

Capture Novem conventions as instructions an AI assistant can apply across
projects. This is a starter skill; extend it as the collection's conventions
develop.

## Scope and structure

Read the repository's README and the skill being changed. Use nearby skills
as examples of format, without copying their subject-specific requirements.
Base new guidance on the user's instructions and established project patterns;
ask when conflicting examples leave a consequential convention unclear.

Create each skill in a root-level `novem-<subject>/` folder with a `SKILL.md`.
Use YAML frontmatter with a `name` matching the folder and a concise
`description` explaining what the skill does and when to use it. Start the
body with a descriptive heading such as `Novem Architecture`.

## Write focused instructions

Keep the main skill short and actionable. Describe its scope, the conventions
that affect decisions, and any context needed to apply them. Keep examples
generic enough to reuse across Novem projects. Do not turn incidental details
from one project into rules for every project.

Move substantial topic-specific detail into `references/` when needed. Link
each reference from `SKILL.md` using a relative path and explain when to read
it, so assistants can load only the relevant material. When adding references,
update that routing rather than listing every reference in the usage prompt.

Add examples, assets, or scripts only when they serve a concrete purpose.
A simple starter can consist of `SKILL.md` alone.

## Keep the repository README in sync

Add or update the skill's entry in the Available skills table, with a relative
link to its `SKILL.md`, a purpose matching its description, and a status badge.
Use the existing Not ready badge for a new starter. Preserve an existing
skill's readiness unless the task includes changing it.

Give each skill a subsection under Usage, ordered alphabetically by heading.
Include a brief explanation followed by a fenced `text` block containing a
prompt someone can copy into an AI assistant. Link to the skill using
`https://github.com/NVMNovem/skills/blob/main/<skill-folder>/SKILL.md`, ask the
assistant to read relevant linked references relative to that folder, and end
with a bracketed task placeholder. Keep the prompt usable as references grow.
GitHub provides heading anchors; omit separate "Link to this section" links.

## Check the result

Check that frontmatter matches the folder, relative file links resolve, and
the README's purpose, status, and usage prompt agree with the skill. Ensure
Usage subsections remain alphabetical and starters contain useful guidance
rather than unfinished scaffold text. Run an available skill validator when
possible and report any validation limits.
