---
name: novem-architecture
description: Apply Novem's Swift architecture and naming conventions when writing or reviewing application code.
---

# Novem Architecture

## Initializer parameter names

Choose initializer argument labels for readability rather than copying private
property names verbatim.

For example, `homeNavigation` is the private property, while the initializer's
external argument label is `navigation`. `HomeTab` already supplies the home
context, so the shorter label keeps call sites clear:

```swift
struct HomeTab: TabContent {
    @Binding private var homeNavigation: [NVMHomeNavigation]

    init(navigation homeNavigation: Binding<[NVMHomeNavigation]>) {
        self._homeNavigation = homeNavigation
    }

    var body: some TabContent<NVMNavigation> {
        // ...
    }
}
```

### Keep context when the type name does not provide it

Use a more descriptive argument label when the surrounding type does not make
the parameter's role obvious. `CartTab` does not imply that the binding controls
home navigation, so `homeNavigation` is clearer than `navigation`:

```swift
struct CartTab: TabContent {
    @Binding private var homeNavigation: [NVMHomeNavigation]

    init(homeNavigation: Binding<[NVMHomeNavigation]>) {
        self._homeNavigation = homeNavigation
    }

    var body: some TabContent<NVMNavigation> {
        // ...
    }
}
```

## Content builders

Organize focused types that conform to a SwiftUI content protocol by the kind
of content they build. Group them by protocol rather than by the screen that
uses them:

This extraction rule is intentionally limited to `TabContent` and
`ToolbarContent`. Put conforming types in separate files, but do not
automatically extract other kinds of content builders. Keep them where they
are unless their protocol is explicitly added to this convention later.

```text
Frontend/
├── TabContent/
│   ├── HomeTab.swift
│   ├── CartTab.swift
│   └── SearchTab.swift
└── ToolbarContent/
    ├── CountryToolbar.swift
    └── DismissToolbar.swift
```

Follow these conventions:

- Apply the separate-file convention only to `TabContent` and
  `ToolbarContent` for now.
- Name the folder after the content protocol, including its `Content` suffix:
  `TabContent` and `ToolbarContent`.
- Keep one primary content-builder type per file and give the file the same
  name as that type.
- Name the type for its semantic role plus the shorter component kind, such as
  `HomeTab`, `CartTab`, `CountryToolbar`, or `DismissToolbar`.
- Omit a redundant `Content` suffix from the type name. The folder and protocol
  conformance already establish that context: prefer `HomeTab` over
  `HomeTabContent` and `CountryToolbar` over `CountryToolbarContent`.
- Keep builders that conform to the same content protocol together, even when
  different views or features consume them.

### Keep composition policy at the call site

Do not move every modifier into an extracted content-builder type. Keep a
modifier inside the builder only when it is intrinsic to that component and
should apply everywhere the component is used. This includes the tab's value,
label, destination, navigation behavior, and stable `customizationID`.

Keep modifiers at the call site when they control how the component
participates in its enclosing `TabView`, `TabSection`, or toolbar; depend on
layout or state owned by the parent; or may vary between placements. Examples
include:

- `defaultVisibility(_:for:)`
- conditional `hidden(_:)`
- `customizationBehavior(_:for:)`
- badges and their accessibility values when their data belongs to the parent
- modifiers applied to a surrounding `TabSection`

For example, `AccountTab` owns its stable tab identity, while the parent owns
the size-class-dependent visibility policy:

```swift
// AccountTab.swift
struct AccountTab: TabContent {
    var body: some TabContent<NVMNavigation> {
        Tab(value: NVMNavigation.account) {
            AccountView()
        } label: {
            NVMNavigation.account.label()
        }
        .customizationID("Tab.\(NVMNavigation.account.id)")
    }
}
```

```swift
// Parent TabView composition
AccountTab()
    .defaultVisibility(.hidden, for: .tabBar)
    .hidden(!(horizontalSizeClass == .regular && verticalSizeClass == .regular))

CartTab(homeNavigation: $homeNavigation)
    .badge(orderProductCount)
    .accessibilityValue(orderProductCountAccessibilityValue)
```

When extracting a builder, preserve call-site modifiers unless they clearly
describe behavior that is invariant and owned by the extracted component.

The declaration should make the grouping explicit:

```swift
struct CountryToolbar: ToolbarContent {
    var body: some ToolbarContent {
        // ...
    }
}
```
