---
name: novem-architecture
description: Apply Novem's Swift architecture and naming conventions when writing or reviewing application code.
---

# Novem Architecture

Use the references relevant to the code being changed. Existing projects may
lag behind these conventions. When conflicting patterns leave the intended
preference unclear, ask before establishing a new rule. Examples use generic
names; preserve persisted identifiers when applying naming changes.

## Folder structure and file naming

Use `Backend/` and `Frontend/` inside each module folder. Put models, data
processing, state, and other non-UI code in `Backend/`; put views, UI
composition, and presentation behavior in `Frontend/`.
Group files by role first, then add domain or type subfolders when useful.

This representative overview uses `MyModule` as the module name. Add folders
as needed for the module's code rather than creating every category in advance:

```text
MyModule/
├── Backend/
│   ├── Attributes/
│   │   ├── PropertyWrappers/
│   │   └── ResultBuilders/
│   ├── Conformances/
│   │   └── Styles/
│   │       └── FormatStyles/
│   │           └── PriceStyle.swift
│   ├── Extensions/
│   │   ├── Calendar.swift
│   │   └── Models/
│   │       └── Catalog/
│   │           └── Product/
│   │               ├── Product+FetchDescriptor.swift
│   │               └── Product+Predicate.swift
│   ├── Managers/
│   │   └── NavigationManager.swift
│   ├── Models/
│   │   ├── Catalog/
│   │   │   └── Product.swift
│   │   └── Cart/
│   │       └── CartState.swift
│   ├── Navigation/
│   │   └── MyModuleNavigation.swift
│   ├── Protocols/
│   │   └── Models/
│   │       └── CartModel.swift
│   └── Types/
│       └── ProductCategory.swift
└── Frontend/
    ├── Commands/
    │   └── SearchCommands.swift
    ├── Conformances/
    │   └── Styles/
    │       └── GroupBoxStyles/
    │           └── ConfigurationStyle.swift
    ├── Extensions/
    │   └── TextField.swift
    ├── Navigation/
    │   └── MyModuleNavigation/
    │       └── MyModuleNavigation+UI.swift
    ├── TabContent/
    │   ├── HomeTab.swift
    │   ├── CartTab.swift
    │   └── SearchTab.swift
    ├── ToolbarContent/
    │   └── DismissToolbar.swift
    ├── ViewModifiers/
    │   └── ActionsModifier.swift
    └── Views/
        ├── ContentView.swift
        ├── Content/
        │   ├── HomeView.swift
        │   ├── CartView.swift
        │   └── SearchView.swift
        └── Settings/
            ├── SettingsView.swift
            └── Settings_General.swift
```

Follow these naming and placement conventions:

- Use descriptive folder names with the casing shown above. Use plural role
  names such as `Models`, `Managers`, `Protocols`, `Views`, and `ViewModifiers`;
  retain protocol names for `TabContent` and `ToolbarContent`.
- Name a Swift file after its primary type, preserving the type's casing and
  existing project prefixes, such as `Product.swift` or `SettingsView.swift`.
  Use names that express the type's role, such as `NavigationManager`,
  `MyModuleNavigation`, `SearchCommands`, and `DismissToolbar`.
- Keep related domain types together, such as `Models/Cart/` or
  `Views/Settings/`. Follow established naming families within those groups,
  such as `Settings_General.swift` for a settings section.
- Put views composed directly by `ContentView` in `Frontend/Views/Content/`,
  keeping `ContentView.swift` at the `Views/` root. For a `TabView`, this includes
  each tab's destination view, such as `HomeView`, `CartView`, and `SearchView`,
  even when an extracted `TabContent` builder composes it. Apply the same rule
  to direct child views in other kinds of `ContentView` composition. Keep the
  tab builders themselves in `Frontend/TabContent/`.
- Put extensions in the relevant layer. A general extension can use the
  extended type's name, such as `Extensions/Calendar.swift`. For extensions
  split by responsibility, use `Type+Responsibility.swift`, such as
  `Product+Predicate.swift` and `Product+FetchDescriptor.swift`.
- Group multiple extensions of a type in a folder named after that type;
  mirror its role and domain grouping when helpful, as in
  `Extensions/Models/Catalog/Product/`.
- Keep navigation definitions in `Backend/Navigation/` and their UI-specific
  extensions in `Frontend/Navigation/`, as illustrated above.
- Use `Conformances/` for concrete protocol implementations, such as format
  styles in the backend and view styles in the frontend. Use `Extensions/`
  for additions to existing types.

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

## State and dependencies

Name shared observable dependencies `Manager`, create them at their app,
scene, or root owner, and inject them through custom environment keys. When
deciding ownership, bindings, or registration, read
[State and dependencies](references/state-and-dependencies.md).

## View modifiers

Expose reusable view modifiers through readable `View` extensions, with
implementation types scoped to their file. When adding or changing these
APIs, read [View modifiers](references/view-modifiers.md).

## Model extensions

Group model-specific helpers by responsibility under the model's namespace.
When adding predicates, fetch descriptors, or mock data, read
[Model extensions](references/model-extensions.md).

## Settings

Keep settings keys and defaults grouped by domain, preserving persisted keys.
When adding or changing preferences, read [Settings](references/settings.md).

## Navigation

Keep navigation cases, identity, and shared metadata together, with view
construction in frontend extensions. When changing routes or selection,
read [Navigation](references/navigation.md).

## Localization

Use named localization tables for new user-facing text, grouped by the owning
type or feature. For resource lookup and text APIs, read
[Localization](references/localization.md).

## Content builders

When creating, extracting, or reviewing `TabContent` or `ToolbarContent` types,
read [Content builders](references/content-builders.md) for grouping, naming,
and modifier ownership conventions. The extraction rule is limited to those
two protocols.
