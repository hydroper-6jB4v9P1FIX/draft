# AVM

There are operations for interoperating with ActionScript and some of the untyped parts taken from ECMAScript.

_Properties:_ The following exist for `Any`: `o.get!(k)`, `o.set!(k, v)`, `o.delete!(k)` and `o.has!(k)`.

_Obtain constructor:_ For `Any`, `o.constructor()` equals exactly `o.get!("constructor")`.

_Iteration:_ The following is available for `Any`:

```
for k in o.keys!() {
    // k: Any
}
for v in o.values!() {
    // v: Any
}
```

_Errors:_

```
es_try!(
    try {},
    catch (e: E1) {},
    catch (e: E2) {},
    finally {},
);
es_throw!(v);
```

_Array:_ `EsArray` is the `Array` type from ECMAScript.

_Function:_ `EsFunction` is the ActionScript `Function` type. It's different from function types in that it's entirely dynamic, like being testable as a subtype. It must be called as `f.call([args])`.
  - `constructor_fn.construct!(arg_1, arg_2, arg_n)` invokes the function as a constructor

_Extern:_ The `extern` modifier is [explained here](extern.md).

_Classes:_ `Class`, `class.construct!(arg_1, arg_2, arg_n)`.