# Option type

## ActionScript mapping

- Primitive types are non-reference types not including ActionScript's `null`: `f64`, `i32`, `u32`, `char` and `bool`.
  - `char` is translated to `u32`, so there's no `OptionChar` and it ends up as `OptionU32`.
  - `String` is a reference primitive type and includes `null`, therefore `str` isn't included in this set.
- `Any` (ActionScript's `*`) is simply called a reference type because it includes `null` for instance.
- `Option<T>` is always unboxed and generates no wrapper ActionScript class for reference types, so it's simply ActionScript's `T`. For primitive types, it ends up always boxed with specialized interned data similiar to `OptionF64 {is_some: bool, value: f64}`.
  - Primitive interning: boxed primitive `Option`s are interned using a `flash.utils.Dictionary` with weak keys as `T` and the value as the boxed primitive.
  - The `is_type!`, `cast!` and `try_cast!` operators detect primitives properly, in that case `i32`, `u32`, `f64` and `bool`, which are boxed by a particular `OptionI32`-like class.