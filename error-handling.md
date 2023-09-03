# Error handling

## Error Propagation Operator

The postfix `?` propagates an erroneous value to the surrounding function.

- [Based on Rust](https://doc.rust-lang.org/stable/reference/expressions/operator-expr.html#the-question-mark-operator)

## Try Block

The `try` block is useful for optional chaining.

```
let _ = try { o?.f1()?.f2() };
```

## Panic

A panic stops script execution with an error.