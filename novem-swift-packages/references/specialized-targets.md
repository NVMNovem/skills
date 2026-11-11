# Specialized targets

Read the sections relevant to a package that contains executables, maintainer
tools, plugins, macros, portable runtimes, platform adapters, or reusable test
support.

## Executables

Keep the executable target as the composition root: parse configuration and
arguments, construct concrete services, and start the process. Put reusable
domain behavior, persistence, networking adapters, and orchestration in focused
library targets so they can be tested without launching the process.

Expose an executable product only when the package is meant to produce a user-
or deployment-facing command. Internal generator executables can remain tools
used by a plugin without becoming public products.

## Maintainer tools

Use a separate package under `Tools/<tool-name>/` when code generation, static
export, schema conversion, or maintenance work has dependencies consumers do
not need. Keep generated artifacts deterministic and document their source of
truth and regeneration command. The main package should consume checked-in
output or invoke the tool explicitly; maintainer tooling must not run during
ordinary consumer builds unless it is intentionally a build plugin.

## Build plugins and generators

Separate the responsibilities:

- a testable core target owns parsing and deterministic generation;
- a thin executable target handles command input and output;
- the plugin declares the executable as its tool and declares precise inputs
  and outputs.

Generate only inside the plugin work directory during a normal build. Do not
rewrite checked-in source or resources from a build-tool plugin. When a portable
runtime and a host-side generator share a package, do not attach the plugin in a
way that forces host tooling into every runtime or cross-compiled build.

Use a command plugin, rather than a build-tool plugin, for an explicit
maintainer action that must modify checked-in files, and request write access
only for that action.

## Macros

Keep the public macro declarations in an importable library target and the
compiler implementation in a `<Name>Macros` target. Depend on
`SwiftSyntaxMacros` and `SwiftCompilerPlugin` only from the implementation.
Macro expansion tests depend on the macro implementation and
`SwiftSyntaxMacrosTestSupport`.

Add a `<Name>Client` executable only when a runnable example materially helps
development or documents behavior; it is not required package scaffolding.
Choose the `swift-syntax` requirement that matches the supported Swift
toolchain rather than copying a version from an unrelated macro package.

## Portable runtimes and adapters

Keep a portable or Embedded-compatible runtime free of host-only dependencies.
Put Foundation, JavaScriptKit, UI, filesystem, networking, or compiler tooling
in separate adapters or host targets. Verify the portable target directly with
its supported SDK; `swift test` on the host does not prove cross-compilation.

When an optional adapter is trait-gated, keep its dependency and imports out of
the disabled graph and test both modes.

## Reusable testing support

Create a `<Name>Testing` target and product only when other packages need its
fakes, transports, fixtures, or assertions. Keep it separate from the main
runtime product and depend on the smallest production surface it supports.
Ordinary package-private helpers stay in the relevant test target.
