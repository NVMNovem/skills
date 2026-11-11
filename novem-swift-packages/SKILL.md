---
name: novem-swift-packages
description: Design, create, or maintain Novem Swift packages, including their manifests, target boundaries, dependencies, tools, and tests.
---

# Novem Swift Packages

Apply these conventions when creating a Swift package or changing its package
graph, manifest, targets, products, dependencies, tools, or package-level test
setup. Keep source-file architecture governed by the owning project's
conventions; this skill focuses on the SwiftPM boundary around that code.

## Establish the package's role

Before editing, inspect `Package.swift`, `Sources/`, `Tests/`, the README, and
relevant scripts or workflows. Classify the package and each target by its
actual role: library, executable, reusable testing support, host-side tool,
macro, plugin, platform adapter, or portable runtime.

Preserve deliberate compatibility constraints and unrelated local changes.
Prefer patterns from actively maintained Swift 6 packages over legacy package
templates, but do not modernize an existing package beyond the requested scope.

## Design a meaningful target graph

Create a target boundary only when it expresses a cohesive responsibility,
dependency boundary, platform boundary, or independently consumed module.
Keep the graph acyclic and declare only direct dependencies.

- Keep portable domain or runtime targets dependency-light. Move Foundation,
  UI frameworks, networking, persistence, compiler support, and platform APIs
  into targets that actually need them.
- Expose a product only when another package or end user should consume it.
  Keep implementation and generator-core targets internal by default.
- Keep executable, plugin, and command entry points thin. Put behavior that
  needs unit tests in a library or internal core target.
- Export a `<Name>Testing` product only when downstream packages genuinely need
  shared fakes, fixtures, or test helpers.

For manifest syntax, version requirements, local development overrides,
platform declarations, resources, traits, and lockfiles, read
[Manifest and dependencies](references/manifest-and-dependencies.md).

For executables, maintainer tools, plugins, macros, portable runtimes, and
testing-support modules, read
[Specialized targets](references/specialized-targets.md) when that target kind
is part of the task.

## Name and lay out modules consistently

For a new repository package, use a lowercase kebab-case SwiftPM package
identity such as `swift-example` or `example-server`. Use UpperCamelCase for
importable products and targets. A command-line product may use the lowercase
hyphenated name users type, while its executable target remains UpperCamelCase.

Use SwiftPM's conventional layout:

```text
Sources/<TargetName>/
Tests/<TargetName>Tests/
```

Name the corresponding test target `<TargetName>Tests`. Put a separate,
maintainer-only package under `Tools/<tool-name>/` when its dependencies or
build work should not enter the consumer package graph.

## Test package boundaries

Give each production target meaningful package-level coverage. New tests use
Swift Testing unless a framework integration, macro test API, supported
toolchain, or established project constraint requires XCTest. Do not migrate
unrelated XCTest suites merely to make the package uniform.

List every local target or external product a test imports in that test
target's dependencies. Test the portable core without its adapters, test
adapters at their real boundaries, and add integration coverage when a plugin,
generated source, resource, or cross-compilation workflow cannot be proven by
unit tests alone.

## Verify the result

Run the narrowest relevant checks while iterating, then validate the package
surface with the applicable commands:

```console
swift package describe
swift build
swift test
```

Also run target-specific, platform, cross-compilation, plugin, or generated
output checks required by the package's documented support. Confirm that every
product maps to an intended target, every target dependency is direct, test
targets match their modules, and relative resource paths exist. Report which
checks ran and any platform or toolchain limits.
