# Serialization

`ds::ser` is a general object serialization module based on a serialized form (`str`), a deserialized form (`T`) and the `Serializable` trait that can only be `#[derive]`d.

The `Serializable` trait is limited to JSON. It can't be implemented without a `#[derive]` as a special case.

## The `ser` attribute

The `ser` attribute is in the prelude since it can be applied to a type rather than a field.

## Derive

_Serialized name:_

When deriving the serialization traits, it is possible to specify a serialized name for a field.

```ds
#[derive(Serializable)]
pub struct S {
    #[ser(name = "fooQ")]
    pub foo_q: f64,
}
```

_Discriminant enumeration:_ The discriminant type of an enumeration that is serialized by discriminant must be either `str`, `f64`, `u32` or `i32`.

```ds
#[derive(Serializable)]
#[discriminant(str)]
pub enum E {
    Variant1 = "variant1",
}
```

_Structural enumeration:_ Structural enumerations must specify a type property and their discriminant type must be `str`.

```
#[derive(Serializable)]
#[ser(type_property = "type")]
#[discriminant(str)]
pub enum Enum {
    Point { x: f64, y: f64 } = "point",
}
```

## JSON

- `ds::ser::json`
  - `json::ser(data)` (serializes into `str`)
  - `json::ser_pretty(data)` (serializes into pretty-printed `str`)
  - `json::de(data)` (deserializes from `str`)
  - `json!` returns `Any` from a JSON-like notation with interpolations and trailling commas

The `json!` literal has the following restrictions:

- Values must be all `Serializable`
- Object keys must be `str`

```
let _ = json!([0, 1]);
let _ = json!({"x": 10});
```