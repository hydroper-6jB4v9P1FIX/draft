# Functions

## Variadic functions

Only `extern` functions can be variadic. Such functions, when called, accept additional `Any` values. If an array of arguments needs to be specified

## Function type

A function type can be variadic, indicating further arguments than what it takes are accepted and delivered to AVM.

A function type, when involving with dynamic typing, gets translated into `EsFunction`.

## Override

```
impl S {
    pub override fn f(self) {}
}
```

## Constructor function

The `fn new` structure method is reserved for constructors and always contains a signature like `fn new(self, ...) -> ()`.