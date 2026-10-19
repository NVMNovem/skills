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
