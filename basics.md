# Basics

The language strives to be close to [Rust](https://rust-lang.org), but simplified and interoperable with ActionScript. DS imports existing ActionScript, but doesn't export to readable ActionScript: the output SWF is mostly unstructured and might be cryptic.

DS is a language limited to Adobe AIR.

```
print!("Debugging");

let x = ();

// mutable variable
let mut x = ();

// shadowing variables is useful for different types
let y: f64 = 0;
let y: str = "";
```

## Variables

There are three different keywords for defining variables:

- `let`
  - Defines a variable scoped to the enclosing block. `let` never appears within a module.
- `static`
  - Defines a variable scoped to the enclosing module or block. A `static` variable is always a single location belonging to the lifetime of the program even if it appears inside a function or block. It is lazily-initialized.
- `const`
  - If used inside a function, equivalent to a `let`. If used outside a function, equivalent to a `static`.

## Entry point

In ActionScript, to work with Adobe AIR, you had to define at least a main class inheriting `Sprite`. DS does it automatically and you can refer to the root container like this:

```
use air::display::*;

fn main() {
    let root = stage().root();
}
```

## Formatting and Debugging

The built-in formatter and debugging is based on the Rust language.

```
let x = 64;
assert_eq!(format!("x equals {x}"), "x equals 64");

print!("{}", 10);
print!("{:?}", Some(10));
```

## Nullability

For ActionScript newcomers, nothing is nullable by default. The `Option<T>` type contains two variants `Some(v)` and `None`. It might look like it wraps `T`, but it only wraps it for primitive types except `str`. [Technical information about `Option`.](option-type.md)

```
// optional chaining
try { o?.f()?.f() }

// dynamic typing: type cast
let mut value: Option<Any> = Some("string");
if let Ok(value) = try_cast!(value, str) {
    // value: f64
}
```

## Collections

- `Map`
  - Internally uses AS3 `flash.utils.Dictionary` for all key types that use reference equality. However:
    - The iterator will internally create an array of entries.
    - String keys are internally always prefixed with a `_` to prevent conflict with properties like `hasOwnProperty`, `constructor` and `toString`.
  - Does not use AS3 `flash.utils.Dictionary` for key types that do not use reference equality. This is important because there are compound types that may be used in pratice, such as `struct MyCustomId(str);`. Specialized map types are auto-generated for these.
  - `is_empty()`
    - Uses the AVM2 `hasnext` instruction.
  - `map!` literal (`map! { k => v }`)
  - `WeakMap`
    - Forbidden for key types that do not use reference equality.
- `Set`
  - For non-reference equality key types, all `Set` operations to lookup a key have an additional cost (such as for `struct MyCustomId(str);`).
- `[T]`, the array type, is equivalent to an ActionScript `Vector`. It uses some of the specialized ActionScript vectors such as `Vector$uint` for some primitive `T`.
- `AvmArray`

## Value types and mutability

Mutability is the same as in ActionScript. In ActionScript you use `var` for writable variables, while in DS you use `let mut`.

Elements from tuples are immutable by default.

```ds
let u = (0, 0);
u.0 += 1; // ERROR!

let u: (mut i32, mut i32) = (0, 0);
u.0 += 1; // ok
```

## Structure initialization

General example:

```ds
struct P {
    x: f64,
    y: f64 = 0.0, // `y` does not need to be specified.
}
let mut p = P { x: 0.0 };

// updating an immutable structure:
// say the structure name were long and you were assigning it
// to a previous variable and you wanted to omit it.
// you can do this with `_`
p = _ { x: p.x + 10.0, ..p };
```

Notes:

- The inferred structure initializer has this syntax because it would otherwise be ambiguous with the block expression.
- If you know Rust, notice that mutability isn't structural.
- The `..p` (rest) passes a `p` value to retrieve missing fields from. Only one rest can appear in the initializer.

## Functions and Lambdas

```
fn f(x: f64, mut y: f64) {}

let f: fn(f64) = |_| {};
let f: fn(f64) -> f64 = |_| 0.0;
```

## Object Methods

```
struct S;

impl S {
    fn f(self) {}
}

let obj = S;
obj.f();
obj.f(); // unlike Rust, does not move.
```

## Pattern Matching

Like Rust:

```
let (x, y) = some_tuple;

let Variant(y) = x else {
    // this block must explicitly panic or return.
}

let matched: bool = matches!(v, Variant(x) if x > 2 | AnotherVariant);
let f: fn(U) = |(x, y)| x + y; 
```

## Types

- `()`
  - Maps to ActionScript `undefined`
- `str`, `char`, `bool`, `i32`, `u32` and `f64`
- Unions
  - `(E1, E2, ...)`
- Function types
  - `fn()`
  - `fn() -> R`

## Structures

```
struct S1; // unit
struct S2(f64); // tuple
struct S3 { x: f64 }; // record

// fields can be mutable
struct S4 {
    mut x: f64,
}
```

`PartialEq + Eq` can be auto implemented for reference-compared (`o1 == o2`) types like this:

```
#[derive(RefEq)]
struct S;
```

## Enumerations

```
enum E {
    Variant1, // unit
    Variant2(f64), // tuple
    Variant3 { x: f64 }, // record
}
```

## Traits

Traits are the same as ActionScript interfaces, with support for:

- Provided methods.
- Generics.

## Aliasing, Importing and Exporting

```
type B = A;
use q::T;
use q::{T1, T2, T3 as T};
use q::*;
```

## Reflection

Unlike ActionScript, DS has no runtime type reflection, like lookuping arbitrary fields or methods. Instead you use macros for such purposes.

## Docs

Similiar to Rust, doc comments use Markdown and support shortcut reference links.
There are different forms of styles.

```
/*!

The surrounding module.

*/

//! The surrounding module.

/// Some item.
pub mod some_item {}

/**
 * Another item.
 */
pub mod another_item {}
```

## Modules

In regards to visibility:

- `pub` for public
- `pub(crate)` for public for the crate only
- Internal is the default visibility

The topmost module is called the crate. The terms _crate_ and _library_ are interchangeable sometimes.

You can use three special keywords either to refer to a module or as a prefix for another module:

- `crate` refers to the crate's topmost module.
- `self` refers to the enclosing module.
- `super` refers to the parent module.

Declaring modules:

```
mod module_here {}

// a_module_at_another_file.ds
mod a_module_at_another_file;

#[module(path = "./yet.ds")]
mod yet_another_one;
```

## Conditional compilation

Conditional compilation attributes, `#[cfg]`, are currently used for testing against _features_ and whether it is a test build. They can only be applied to blocks at the moment.

```
#[cfg(feature = "some_feature", feature = "another_feature")] {
    //
}
#[cfg(test)]
mod tests {
    #[test]
    fn some_test() {}
}
```

## Futures

`Future<T>` is a structure that represents the eventual completion of an asynchronous operation and its resulting value.

Futures can be awaited for via `.await`.

```
future.await;
```

Async blocks evaluate to a future.

```
let future = async { /* ... */ };
```

Async functions with a return type can be defined in two different ways:

```
async fn f() -> f64 {
    ...
}
async fn f() -> Future<f64> {
    ...
}
```

## Generators

Generator functions are functions that can suspend at any point. When called, a generator function returns an `Iterator`.

```
fn f() -> i32 {
    for i in 0..10 {
        yield i;
    }
}

// equivalent to
fn f() -> Iterator<i32> {
    ...
}
```

Asynchronous generators are possible:

```
async fn f() -> i32 {
    for task in tasks {
        yield task.await;
    }
}
```

## Generics

- Type annotations can argument a type as `G<...>` or `G::<...>`
- An expression can argument types of a generic type or function using `::<...>`

## Array initializer

Brackets can initialize:

- `[T]`
- `AvmArray`
- Sets

It supports spread (`...other`) for non-fixed types.

## Subtype relationship

```
let subtype_node = cast!(node, T);
let subtype_node = try_cast!(node, T); // Option<T>
let is_of_t = is_type!(node, T); // bool
```

## Including SWC libraries

This is used mainly for reusing libraries written in ActionScript.

```
include_swc!("./lib.swc");
```

## Inherited structures

`fn new(self, ...)` is a reserved method as is described in [Structure Inheritance](./structure-inheritance.md#constructing-subtype).

## Main differences from Rust

- There are no borrow types and no lifetimes. Even no `&str`.
- `Any`
  - `Any` is not a trait and exactly the ActionScript `*` type.
- `Copy` and `Clone`
  - There is no `Copy` trait on DS. Everything is copiable by default.
  - The `Clone` trait isn't frequent.
- Traits
  - The _trait objects_ concept isn't the same.
- Function types
  - Function types are entirely simplified.
- Futures
  - Futures are entirely different and unrelated.