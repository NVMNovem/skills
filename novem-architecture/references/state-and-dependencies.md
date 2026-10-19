# State and dependencies

Use this guidance when deciding who owns mutable state, how a child edits it,
or how views receive a shared observable dependency.

## Ownership

| Kind of state | Preferred placement |
| --- | --- |
| Transient state owned by a view, such as sheet visibility | `@State private` in that view |
| State owned by a parent that a child must edit | A `@Binding` supplied through the child's initializer |
| A value that a child only reads | An ordinary initializer parameter |
| Observable state shared across a view hierarchy | A `Manager`, supplied through a custom environment key |
| A persisted preference | The project's settings and storage conventions; see [Settings](settings.md) |

Keep a single source of truth for each value. Derive values from existing
state where practical, rather than maintaining a second mutable copy.

Create a manager at the app, scene, or root view whose lifetime it belongs
to, and inject that instance. Views that consume it receive that instance
rather than creating their own or reaching for a singleton. Consider
whether multiple windows or documents should share it before choosing its
owner.

## Environment access

Use custom environment keys, with entries under
`Backend/EntryValues/EnvironmentValues/`, grouped by role when helpful:

```text
Backend/EntryValues/EnvironmentValues/Managers/
└── CartManagerEnvironment.swift
```

Register the owner-created manager with `.environment(\.cartManager, manager)`
or a focused helper such as `.registerCartManager(manager)`. Consume it with
`@Environment(\.cartManager)`. Registration helpers pass the supplied instance
through to the environment.

For example, assuming the module declares a `cartManager` entry in
`EnvironmentValues`, a root view can own and inject the dependency:

```swift
import Observation
import SwiftUI

@MainActor
@Observable
final class CartManager {
    var showsUnavailableProducts = false
}

struct ModuleRootView: View {
    @State private var cartManager = CartManager()

    var body: some View {
        ContentView()
            .environment(\.cartManager, cartManager)
    }
}

struct CartOptionsView: View {
    @Environment(\.cartManager) private var cartManager

    var body: some View {
        @Bindable var cartManager = cartManager

        Toggle(isOn: $cartManager.showsUnavailableProducts) {
            Text("Show unavailable products", tableName: "CartOptionsView")
        }
    }
}
```

Use `@Bindable` when a control needs a binding to observable properties;
ordinary reads and method calls can use the environment instance directly.
Follow the project's actor isolation for UI-owned observable state.

Use `Manager` names for shared observable dependencies, such as `CartManager`
and `NavigationManager`. Older projects may use `Store`, type-based environment
access, or `.shared`; those examples do not change this preferred convention.
Keep the owning instance's lifetime explicit when applying the convention.
