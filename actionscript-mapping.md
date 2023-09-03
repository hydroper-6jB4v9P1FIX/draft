# ActionScript mapping

## Output Names

**Module separator:** DS outputs unstructured types except for fields. Probably AVM allows arbitrary characters in property names, which would make sense because it inherits ECMAScript; if not, `::` translates to underscore and any duplicates will have to get a number suffix.

**Fields:** Field names never matter, therefore they might get simplified to `_0`, `_1` or `_n`.

**Extern clash:** To not clash with `extern` definitions, non-extern names belong to the `public` namespace of a special package that never conflicts with others: it can be generally `$` or `(ds)` (if AVM allows parentheses in package names). For example, when you see an error call stack, you might see:

```plain
Error: Message
    at (ds)::custom_crate::custom_fn:1
    ...
```

Generally this doesn't matter, since there's no runtime type reflection. It only matters for debugging. This special package could 

## Output Functions

Functions and instance methods anywhere are always unstructured. For example, the following DS script:

```
struct S;

impl S {
    fn f(self) {}
}
```

Translates to this ABC:

```
(ds)::["S.f"] = function(self: S): void {};
```

## Output Modules

Modules don't go to output, i.e., although they _look_ like ActionScript packages, they're entirely unrelated.

## Constructing structures

Structures that inherit from another (not `Object`) will always generate an AVM `constructsuper` instruction when constructing, including because of fields whose initial value can't be expressed into AVM.

## Traits

Refer to [Traits](traits.md#actionscript-mapping).

## Enumerations

- Variants translates to ordinary ActionScript classes, matched via AVM subtype instructions.

## Equality and Comparison

- For `#[derive(RefEq)]` structures and primitive types, equality (`==`, `!=`) is always a reference equality.
- For `Option<T>` where `T` is a reference type and `T` is a `#[derive(RefEq)]` type or `Any`, (`==`, `!=`) is always a reference equality.
- For `Option<T>` where `T` is a reference type and `T` is a non-`#[derive(RefEq)]` structure, (`==`, `!=`) is always a conditional equality invoking `Eq` or `PartialEq`.
- For `Option<T>` where `T` is a primitive type, equality is done with some AVM instructions involving checks for `Some` and the actual values.
- For `Any`, equality is always a reference equality.

## Function types

Function types always map to ActionScript `Function`.