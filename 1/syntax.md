# Syntax

## Attributes

```
#![attribute_to_the_module_or_crate]

#[attribute]
item;

#[attribute()]
item;

// non_unique_key may appear multiple times.
#[attribute(non_unique_key)]
item;

// key-value pairs use an arbitrary expression
// as value.
#[attribute(non_unique_key = expr)]
item;

// two types of entries:
// - key-value pairs
// - named groups
#[attribute(named_group())]
item;
```