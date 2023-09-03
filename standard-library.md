# Standard library

DS has its standard library at the `air` crate.

## Prelude

The `ds::prelude` module re-exports frequent items from the standard library. It's included by default in the parent scope of the crate.

Notice _parent_ scope. That means you can override anything that `ds::prelude` exports in your module.

## Overview

- `ds` (root module) contains macros such as
  - `print!`
  - `assert!`
  - `assert_eq!`
  - `assert_ne!`
  - `debug_assert!` and its `_eq` and `_ne` versions
  - `panic!`
  - `format!`
  - `compile_error!`
- `ds::array`
  - `Array<T>`
    - The `[T]` type
- `ds::bytes`
  - `Bytes`
    - Equivalent to ActionScript `flash.utils.ByteArray`.
- `ds::delegate`
  - `Delegate` trait
- `ds::fmt`
  - Similiar to the Rust language.
- `ds::future`
  - `Future` structure
  - `Future::all`
  - `Future::race` returns (_value_, _index_)
- `ds::iter`
  - `Iterator`
  - `IntoIterator` (iterable)
- `ds::ops`
  - Range operators
- `ds::option`
- `ds::lazy`
  - `Lazy<T>`
    - Implements `Delegate`
    - It is not implemented using `Option<T>` since `T` can also be another `Option<T>` and that's not allowed in the language. It's implemented natively in the language.
  - `lazy!`
    - `let v = lazy! {};`
- `ds::cast`
  - `CastError`
  - Cast operators for inheritance
- `ds::convert`
  - `TryFrom` and `TryInto` result in a `Result` (not `Option`), having an associated `Error`
- Regular expressions
  - `regexp!` literal
- `ds::ser`: serialization
- `ds::str`
  - `ToString`

## `str` and `char`

The `char` primitive type is an Unicode Scalar Value and the `str` primitive type is a string of UTF-16 code units.

`str` implements `From<char>` and `From<[char]>`.

## Collections

- Optimization:
  - `Option<T>`, structures with `#[derive(RefEq)]` and primitive types lookup directly, while other types require a call to `partial_eq()` or `eq()`.
    - This relies on `Option<T>` performing interning for primitive types, covered in [Option type](option-type.md).

## Delegate

```
pub trait Delegate {
    type Target;
    fn delegate(self) -> Self::Target;
}

// the type system emulates:
// impl<T: Delegate> From<T> for T::Target
```