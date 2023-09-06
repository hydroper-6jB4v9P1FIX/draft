# Structure Inheritance

ActionScript supports class inheritance, therefore DS has to support it equally. It's not a pattern to be "abused", but for compliance with ActionScript, subtype conversions and `super.f()` are going to be supported.

Only structures with the `#[inheritable]` attribute can be inherited.

## Semantics

- Traits implementations are inherited by subtypes and cannot be implemented again.
- Super constructor must be specified when constructing subtype

## Defining Inheritable

Synopsis:

```
#[inheritable]
struct MyInheritableStructure;
```

## Subtype relationship

```
let subtype_node = cast!(node, T);
let subtype_node = try_cast!(node, T); // Option<T>
let is_of_t = is_type!(node, T); // bool
```

## Inheriting

Synopsis:

```
use air::display::DisplayObject;

struct CustomNode: DisplayObject;
```

## Constructing subtype

Any subtype not directly inheriting `Object` must define a constructor and call `super()` and has the following restrictions:

- Cannot be constructed via `S {}`
- Cannot be constructed via `S`
- Can only be constructed via `S(...)`. (If the constructor is not defined, it's auto-generated, taking same arguments from super type.)
- If it defines any constructor (`new`), it must contain `super(...)` at the top of the block.

```
struct MySubtype: SuperType {
    // all fields must have an initial default value.
    x: f64 = 0.0,
}

impl MySubtype {
    fn new(self) {
        super();
    }
}

let object = MySubtype();
```