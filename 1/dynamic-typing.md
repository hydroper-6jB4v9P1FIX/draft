# Dynamic typing

_Any:_ `Any` is equivalent to ActionScript's `*` type. If you're used to Rust, it's entirely different and is rather another type and not a trait or structure. `Any` implements `PartialEq + Eq` as reference equality.

_Object:_ `Object` is equivalent to ActionScript's `Object` type, an `#[inheritable]` structure, inherited by all types by default. It's empty for not polluting all types.

## AVM

There are operations for interoperating with untyped ActionScript, covered in [AVM section](avm.md).