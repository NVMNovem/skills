# Settings

Use this guidance when adding preferences or changing their keys, defaults,
or storage access.

## Keys and defaults

Keep storage keys and default values in separate types under
`Backend/Settings/`, following the project's names. Group both types by the
same domain so a preference's key and default are easy to locate together.

For example:

```swift
// MyModuleSettings.swift
enum MyModuleSettings {
    enum Cart {
        static let showsUnavailableProducts =
            "mymodule.cart.showsUnavailableProducts"
    }
}

// MyModuleSettingsDefault.swift
enum MyModuleSettingsDefault {
    enum Cart {
        static let showsUnavailableProducts = false
    }
}
```

Use the project's existing key-generation helper when it has one. The
literal above illustrates a key for a new generic module; it does not replace
an established key format.

Views reference the centralized key and default:

```swift
import SwiftUI

struct CartOptionsView: View {
    @AppStorage(MyModuleSettings.Cart.showsUnavailableProducts)
    private var showsUnavailableProducts =
        MyModuleSettingsDefault.Cart.showsUnavailableProducts

    var body: some View {
        Toggle(isOn: $showsUnavailableProducts) {
            Text("Show unavailable products", tableName: "CartOptionsView")
        }
    }
}
```

Localize the label through the owning type or feature's named table. Share
the key and default across consumers rather than repeating literals in each
view.

## Persistence boundaries

Treat stored key strings as persisted identifiers. Renaming a Swift property
or moving it to another nested type should preserve the stored key. Check
helpers that derive strings from type names before reorganizing those types.
When a key or stored representation genuinely changes, account for existing
values in the project's migration approach.

Keep transient view state local. Follow existing `@SceneStorage` usage for
scene-specific restoration and the project's storage mechanism for durable
preferences. This organization does not require moving every stored value
into `@AppStorage` or replacing an established settings abstraction.
