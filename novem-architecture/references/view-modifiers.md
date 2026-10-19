# View modifiers

Use this guidance when extracting reusable view behavior or adding a fluent
API to `View`. Keep modifier implementations in `Frontend/ViewModifiers/`.

## Call-site API

Expose the behavior through a descriptive `View` extension, such as
`.closable(isPresented:)` or `.edgePadding(id:visibleIDs:)`. Keep implementation
types `private` or `fileprivate` when callers only need the extension.

Name the file for the behavior or primary modifier, following nearby files.
Related implementation variants can share that file and one extension API;
they do not each need a separate file.

For example, `Closable.swift` can contain a hidden implementation and the
method used by callers:

```swift
import SwiftUI

fileprivate struct ClosableModifier: ViewModifier {
    @Binding private var isPresented: Bool

    init(isPresented: Binding<Bool>) {
        self._isPresented = isPresented
    }

    func body(content: Content) -> some View {
        content.toolbar {
            ToolbarItem(placement: .cancellationAction) {
                Button(role: .cancel) {
                    isPresented = false
                } label: {
                    Text("Close", tableName: "Closable")
                }
            }
        }
    }
}

extension View {
    func closable(isPresented: Binding<Bool>) -> some View {
        modifier(ClosableModifier(isPresented: isPresented))
    }
}
```

The parent supplies the presentation binding. The modifier changes the same
state; it does not maintain a separate presentation flag. Keep intrinsic labels
in the component's named localization table.

## Extraction boundaries

Extract behavior when it has a useful reusable meaning, such as closing a
presentation or applying position-dependent padding. A modifier can accept
values, bindings, or closures according to who owns its inputs and actions.

Keep placement-specific policy at the composition site. A shared modifier
should not silently take ownership of unrelated navigation, selection, or
layout state. For tab and toolbar builders, use the more specific modifier
ownership rules in [Content builders](content-builders.md).

The separate-file convention for `TabContent` and `ToolbarContent` does not
automatically extend to other content protocols as part of this guidance.
