# Navigation

Use this guidance when adding destinations, tab selections, navigation paths,
or shared navigation labels.

## Definitions and identity

Keep navigation definitions in `Backend/Navigation/`. Prefer typed cases and
associated values for destinations, following the existing navigation model.
Keep shared identity, titles, and symbol names on the navigation type rather
than repeating their switches in each consuming view.

For example, a new module can define stable cases like this:

```swift
enum MyModuleNavigation: String, CaseIterable, Identifiable, Hashable {
    case home
    case cart
    case search

    var id: String { rawValue }
}
```

Add conformances according to actual use: `CaseIterable` for iterable cases,
`Hashable` for typed selection or paths, and `Codable` when navigation is
serialized. Destinations with associated values need identity appropriate
to those values rather than this simple raw-value example.

Preserve established route IDs and serialized values. For a new raw-value
route that later gets renamed, retain its original raw string if persisted
state or customization depends on it. Keep tab customization IDs stable too.

## Frontend composition

Put view-producing helpers in frontend extensions of the navigation type:

```text
Backend/Navigation/
└── MyModuleNavigation.swift
Frontend/Navigation/MyModuleNavigation/
└── MyModuleNavigation+UI.swift
```

For example, if `title` and `systemImage` are shared metadata:

```swift
import SwiftUI

extension MyModuleNavigation {
    @MainActor
    @ViewBuilder
    func label() -> some View {
        Label {
            Text(title)
        } icon: {
            Image(systemName: systemImage)
        }
    }
}
```

Keep destination construction such as `view()` in the same frontend grouping
when the navigation type provides it. Tab builders can reuse these helpers;
their files stay in `Frontend/TabContent/`, while destination views composed
by `ContentView` belong in `Frontend/Views/Content/`.

## Selection and paths

Keep selection or path state with its existing owner. Use local state when
only one view needs it, or the environment's navigation manager when
multiple consumers coordinate it. Bind to that state rather than keeping a
second mutable copy of the selected destination.

Follow the project's typed path representation and stack behavior. This
convention does not require a new router abstraction or a stack for every tab.
