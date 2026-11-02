# Localization

Use this guidance when adding user-facing text or moving localized labels
between components. Use named localization tables for new user-facing text,
grouped by the owning type or feature.

## Table ownership

Keep text with the type or feature that owns its meaning. Use the corresponding
table name, such as `CartView`, `MyModuleNavigation`, or `Closable`. Reuse an
appropriate existing type or feature table when one already owns the text.

Shared navigation titles belong with navigation metadata. Labels intrinsic
to a reusable component belong with that component. Text specific to one
placement can remain with the composing view.

Moving Swift code between files does not automatically require renaming its
table or keys. Preserve the resource lookup unless the change intentionally
moves ownership and updates the associated resources.

## Folder structure and file naming

For new localization catalogs, use `Resources/` inside the owning module.
Reflect the owning type or feature's source path beneath that resource root,
including `Backend/`, `Frontend/`, and any role, domain, or type subfolders.
Use the localization table's name as the catalog filename: a `DismissToolbar`
table belongs in `DismissToolbar.xcstrings`.

This example mirrors source and resource paths inside a Swift package target:

```text
Sources/MyModule/
├── Backend/
│   └── Navigation/
│       └── MyModuleNavigation.swift
├── Frontend/
│   ├── ToolbarContent/
│   │   └── DismissToolbar.swift
│   └── Views/
│       └── Settings/
│           └── SettingsView.swift
└── Resources/
    ├── Backend/
    │   └── Navigation/
    │       └── MyModuleNavigation.xcstrings
    └── Frontend/
        ├── ToolbarContent/
        │   └── DismissToolbar.xcstrings
        └── Views/
            └── Settings/
                └── SettingsView.xcstrings
```

Apply the same relative mapping inside an app module's folder. The resource
path follows the component that owns the text, even when a helper in another
source file performs the lookup. For example, a button-type enum can use the
`DismissToolbar` table without needing its own catalog.

Create catalogs only for tables that need localized text. When moving an
owning component, move its catalog to the matching resource path. A folder
move does not itself change the table name or keys. Keep table names distinct
within a resource bundle; subfolders do not namespace localization lookups.
Preserve established `.strings` or `.stringsdict` resources unless the task
includes migrating their format.

## Text versus String

Use `Text` localization for labels rendered directly by SwiftUI:

```swift
Text("Your cart is empty", tableName: "CartView")
```

Use `String(localized:)` when a property or another API needs a `String`,
such as a navigation title:

```swift
var title: String {
    String(localized: "Cart", table: "MyModuleNavigation")
}
```

Keep interpolation within the localized message so translators can change
word order. Follow existing pluralization and formatting resources for
counts, dates, and amounts. Treat user-entered content and externally supplied
names as data rather than adding them as localization keys.

Use the project's resource format and extraction workflow to add or update
the named table. Older default-table usage does not change this convention
for new text. Broader localization migrations and translations can follow
the scope of the requested work.

## Swift package resources

Keep reusable component text in its package; keep app-specific text in the
app. For a package target with localization resources, declare its default
language and process its resource directory in `Package.swift`:

```swift
let package = Package(
    name: "my-package",
    defaultLocalization: "en",
    targets: [
        .target(
            name: "MyModule",
            resources: [.process("Resources")]
        )
    ]
)
```

Choose the project's actual default language rather than assuming English.
SwiftPM generates `Bundle.module` for a target containing resources. Use it
explicitly for package-owned localization, including lookups in helper types:

```swift
String(localized: "Cancel", table: "DismissToolbar", bundle: .module)
Text("Cancel", tableName: "DismissToolbar", bundle: .module)
```

The earlier examples without an explicit bundle apply to text owned by the
main app. Package lookups must select the package's resource bundle.

## Languages and verification

Keep language translations together inside each `.xcstrings` file. Add a
language in Xcode's string catalog editor using the language sidebar's **+**
button, then enter its translations. Use language identifiers such as `nl`,
`fr`, and `de`; add a region only when region-specific wording is needed.
Adding a translation to an existing catalog does not require another catalog
or a change to `defaultLocalization`.

Add the languages requested by the project rather than treating an example's
language list as mandatory. Preserve intentional translation choices when
updating resources or their tests.

When verifying package localization, check that the built bundle contains the
requested languages and table entries. Looking up a missing key can return
the key itself, so an English result equal to its key does not prove the
resource was bundled. Keep localization tests under the mirrored test paths
specified in [Tests](tests.md).
