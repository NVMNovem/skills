# Tests: folder structure and file naming

Use this guidance when adding or reorganizing tests for a Swift module.
The test tree reflects the module being tested, so a source file and its
tests can be found through the same relative path.

## Module and folder mapping

Name the test module `<ModuleName>Tests`. For a Swift package, place its files
under `Tests/<ModuleName>Tests/`, corresponding to
`Sources/<ModuleName>/`. In an Xcode project, apply the same mapping beneath
the source and test target roots without introducing Swift package folders.

Preserve each source file's relative directories in the test module, including
layer, role, domain, and type folders. Tests for frontend code stay under
`Frontend/`; tests for backend code stay under `Backend/`.

This example shows the same tree on both sides:

```text
Sources/MyModule/
├── Backend/
│   ├── Extensions/
│   │   └── Models/
│   │       └── Product/
│   │           └── Product+Predicate.swift
│   └── Types/
│       └── ProductCategory.swift
└── Frontend/
    ├── ToolbarContent/
    │   └── DismissToolbar.swift
    └── ViewModifiers/
        └── LabelStyleModifier.swift

Tests/MyModuleTests/
├── Backend/
│   ├── Extensions/
│   │   └── Models/
│   │       └── Product/
│   │           └── Product+PredicateTests.swift
│   └── Types/
│       └── ProductCategoryTests.swift
└── Frontend/
    ├── ToolbarContent/
    │   └── DismissToolbarTests.swift
    └── ViewModifiers/
        └── LabelStyleModifierTests.swift
```

Create only the test files and folders needed for meaningful coverage. The
mirrored structure does not require an empty test file for every source file.
Place tests beside their corresponding tests in this tree rather than
collecting unrelated coverage in a single module-level test file. Remove a
generated placeholder when real tests replace it.

## File and suite names

Append `Tests` to the source filename's stem, preserving casing, prefixes,
and responsibility suffixes: `Product.swift` becomes `ProductTests.swift`,
and `Product+Predicate.swift` becomes `Product+PredicateTests.swift`.
For an extension named after a framework type, retain its enclosing type
folders: `Frontend/Extensions/ProductCategory/SwiftUIView.swift` maps to
`Frontend/Extensions/ProductCategory/SwiftUIViewTests.swift`.

Name the primary test suite after the tested type or responsibility with a
`Tests` suffix. Use a valid Swift identifier such as `ProductPredicateTests`
for `Product+PredicateTests.swift`. Include enclosing type context when needed
to distinguish suites for similarly named extension files, such as
`ProductCategorySwiftUIViewTests` for `SwiftUIViewTests.swift`.

When moving or renaming a source file, update the corresponding test path and
filename in the same change. Keep shared fixtures and helpers separate from
source-specific suites, following the project's existing support convention;
they do not need a counterpart in the source tree.
