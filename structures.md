# Structures

```
// garbage-collected structure.
struct CustomReferenceType;
```

## Reference equality

If a structure is equal to another by reference and not by content, use the `#[derive(RefEq)]` attribute to derive `PartialEq + Eq`:

```
#[derive(RefEq)]
struct S;
```

## Inheritance

Inheritance is covered in [Structure Inheritance](./inheritance.md).