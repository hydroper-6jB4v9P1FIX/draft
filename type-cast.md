# Type cast

There are different ways to cast a type to another and one way to test for a subtype:

```
// `as` always succeeds. casting to `Any` is valid as well.
let cast = node as T;

// `From`, `TryFrom`, `Into`, `TryInto`
let y: T = x.into();

// cast operations introduced due to inheritance.
// `cast!` may panic.
let subtype_node = cast!(node, T);
let subtype_node = try_cast!(node, T); // Result<T, CastError>
let is_of_t = is_type!(node, T); // bool
```

Restrictions:

- `Option<T>` cannot be used in any of these operations.
- `[T]` cannot be used in any of these operations.
- `fn(...) -> ...` cannot be used in any of these operations.
- `Map<T>`
- `WeakMap<T>`
- `Set<T>`

Special behavior for `is_type!`, `cast!` and `try_cast!`:

- It naturally takes an `Option<T>` value as `T`.
- If `T` is an ActionScript non-nullable primitive type and the value is `Option<T>`, a cast will unbox the value, since it's wrapped into an ActionScript class. For example, `Option<f64>` translates to an interned ActionScript class `OptionF64`.

## Any type

The `Any` type is not a supertype of any type. The cast operators work though mostly as if `Any` were supertype of any other type.