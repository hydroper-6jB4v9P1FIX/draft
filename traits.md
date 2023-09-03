# Traits

## Subtype relationship

```
let subtype_node = cast!(node, T);
let subtype_node = try_cast!(node, T); // Option<T>
let is_of_t = is_type!(node, T); // bool
```

## From, Into and Delegate

- `impl From<A> for B` implies `impl Into<B> for A`
- `impl TryFrom<A> for B` implies `impl TryInto<B> for A`
- `impl Delegate for T` implies `impl<T: Delegate> From<T> for T::Target` (expressed in the type system)

## ActionScript mapping

Traits basically translate to ActionScript interfaces, however non-extern methods aren't part of the ActionScript interface.

All of the trait's instance methods, whether provided or not, translate to ActionScript bytecode as static functions. Instance methods marked `extern` will use an actual ActionScript's interface method.

Besides this, a trait inheriting another will duplicate all methods for itself. In this case, when generating ABC, the compiler must append counter suffixes to the method name for avoiding name clash. This takes both trait's static methods and instance methods into consideration.

## Serializable

The `Serializable` trait is not allowed in an `impl`.