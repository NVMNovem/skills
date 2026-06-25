# Project Standards

A collection of reusable architecture, project structure, and AI collaboration standards used across my software projects.

Instead of rewriting the same architectural decisions for every repository, this project contains reusable documentation that both humans and AI coding assistants can follow.

The goal is consistency.

---

## Philosophy

Good software is not only about writing correct code.

It is also about:

- consistent project organization
- clear architectural boundaries
- predictable naming
- reusable conventions
- AI-assisted development without losing design consistency

These documents describe those conventions.

---

## Current Documents

### PROJECT_STRUCTURE.md

Defines how projects should be organized.

Topics include:

- Backend vs Frontend
- folder responsibilities
- naming conventions
- module extraction
- file organization
- package boundaries
- recommended refactoring workflow
- AI structural guidelines

---

## Planned Documents

This repository will gradually grow with additional reusable documents.

Examples include:

### ARCHITECTURE.md

General architectural principles independent of any specific project.

Examples:

- separation of responsibilities
- package ownership
- dependency direction
- rendering pipelines
- state management

---

### AI_GUIDELINES.md

General instructions for AI coding assistants.

Examples:

- how to approach refactors
- how to preserve architecture
- when to stop and ask questions
- how to introduce new abstractions
- testing expectations

---

### NAMING.md

Naming conventions.

Examples:

- folders
- files
- modules
- protocols
- extensions
- generic types
- Swift packages

---

### DOCUMENTATION.md

Documentation conventions.

Examples:

- DocC
- Markdown
- architecture documents
- code comments

---

### TESTING.md

Testing philosophy.

Examples:

- unit tests
- regression tests
- architecture tests
- snapshot tests

---

### RELEASE.md

Release workflow.

Examples:

- versioning
- changelogs
- migration notes

---

## Goal

Every new project should be able to reuse these documents with little or no modification.

The objective is to make both human contributors and AI coding assistants follow the same architectural language across every repository.
