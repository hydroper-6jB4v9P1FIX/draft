# Extern

The `extern` modifier is used to import ActionScript functionality. DS and ActionScript have incompatible syntax. For example, DS doesn't know that `flash.utils` is a package.

Basics:

```
#[actionscript(package = "com.q")]
extern fn f();

#[actionscript(package = "com.q", name = "someFoo")]
extern fn some_foo();

#[actionscript(package = "com.q", name = "C")]
extern struct C;

impl C {
    #[actionscript(name = "doSomething")]
    extern fn do_something(self);
}
```

## Virtual properties

```
impl S {
    #[actionscript(get, name = "x")]
    extern fn get_x(self) -> T;

    #[actionscript(set, name = "x")]
    extern fn set_x(self, value: T);
}
```

## Rest

Variadic functions contain a final `...` token:

```
#[actionscript(name = "f")]
extern fn f(...);
```