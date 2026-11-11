# Manifest and dependencies

Read this reference when creating or changing `Package.swift`, dependency
requirements, supported platforms, resources, SwiftPM traits, local dependency
overrides, or `Package.resolved` policy.

## Language and platform declarations

- Set `swift-tools-version` to the minimum toolchain required by the manifest
  features and supported source. Do not raise it only to make an old manifest
  look current.
- Preserve the package's established Swift language mode unless the task
  includes a migration. For new Swift 6 packages, make the intended Swift 6
  mode unambiguous when the selected tools version or surrounding project
  requires it.
- Declare only platforms the package actually supports, with the lowest
  versions the code and dependencies are maintained against. Omit a platform
  list only for an intentionally portable package and verify the relevant
  non-Apple or cross-compilation target separately.
- Treat a dependency's higher toolchain or platform floor as a real package
  requirement even when the local manifest can be parsed by an older version.

## Products, targets, and dependencies

- Keep package identities lowercase and kebab-case for new repository packages.
  Match product and target names to the importable module unless a CLI product
  intentionally uses the command users type.
- Use a local target name such as `"ExampleCore"` for a target in the same
  package. Use `.product(name:package:)` for a product supplied by an external
  package, including another Novem repository.
- Give every target only the dependencies its source imports. Do not rely on a
  sibling target's transitive dependency.
- Prefer tagged semantic-version requirements for released dependencies. A
  branch requirement is temporary and should carry a comment explaining the
  missing release and the intended exit. Use an exact version or bounded range
  only for a deliberate compatibility constraint.
- Explain non-obvious graph decisions, measured size constraints, and temporary
  pins. Remove template comments that merely restate SwiftPM syntax.

## Local multi-repository development

Keep the normal manifest path reproducible with versioned repository URLs. If
sibling checkouts need an opt-in local mode, select path dependencies through a
clearly named `Context.environment` flag or path variable while leaving remote
dependencies as the default.

Document the flag and ensure CI, releases, and production builds use the remote
graph. Never commit a developer-specific absolute path. SwiftPM edit or local
checkout overrides remain valid alternatives when the manifest does not need a
shared local mode.

## Optional dependency graphs

Use a SwiftPM trait only when a capability is genuinely optional and disabling
it prevents irrelevant dependencies from being resolved or built. Keep the
default trait set minimal, gate the affected target dependencies, and verify
both enabled and disabled graphs. A trait does not replace a clean target
boundary.

Use conditional compiler or linker settings only for a demonstrated platform
need. Keep unsafe flags narrowly scoped and document why the supported toolchain
requires them.

## Resources and lockfiles

Use `.process` for runtime resources SwiftPM should transform or optimize. Use
`.copy` when exact bytes or directory layout matter, including fixtures and
schemas. Keep a resource under the target that owns it and verify the path from
that target's root.

Treat `Package.resolved` according to the repository's role. A deployable root
application or tool can commit it for reproducible builds. A library's lockfile
does not constrain consumers, so do not describe it as a published dependency
guarantee. Preserve the repository's existing lockfile policy unless the task
explicitly changes it.
