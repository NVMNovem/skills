# Model extensions

Use this guidance when a model has related predicates, fetch descriptors,
or sample data. Keep these helpers discoverable through the model's namespace.

## Group by responsibility

Use a focused extension file and nested namespace for each substantial group:

```text
Backend/Extensions/Models/Catalog/Product/
├── Product+Predicate.swift
└── Product+FetchDescriptor.swift
```

Expose calls such as `Product.Predicate.available()` and
`Product.FetchDescriptor.available()`. Reuse meaningful query definitions
across views and data operations. Introduce these groups as the model needs
them; a small model does not require every helper category.

For an existing SwiftData `Product` model with an `isAvailable` property:

```swift
// Product+Predicate.swift
import Foundation

extension Product {
    enum Predicate {
        static func available() -> Foundation.Predicate<Product> {
            #Predicate<Product> { product in
                product.isAvailable
            }
        }
    }
}
```

```swift
// Product+FetchDescriptor.swift
import SwiftData

extension Product {
    enum FetchDescriptor {
        static func available() -> SwiftData.FetchDescriptor<Product> {
            SwiftData.FetchDescriptor(
                predicate: Product.Predicate.available()
            )
        }
    }
}
```

Qualify framework types when the nested namespace has the same name:
`Foundation.Predicate<Product>` and `SwiftData.FetchDescriptor<Product>`.
Keep sorting, limits, and prefetching with the descriptor that defines the
query. A view can consume it through
`@Query(Product.FetchDescriptor.available()) private var products: [Product]`.

This is a convention for model helpers, not a requirement to use SwiftData
in modules that use another persistence mechanism.

## Mock data

Follow an existing mock or test target's organization, using extension files
such as `Product+Mock.swift` and a nested `Product.Mock` namespace for named
fixtures. Keep related fixtures reusable across previews or tests that can
access that target. Preserve target boundaries; this convention does not
require adding a new target or exposing mock data in production APIs.
