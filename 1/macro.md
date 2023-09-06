# Macro

DS aims to support:

- Declarative macros
- Procedural macros
  - `#[proc_macro]`
  - Attribute macros (`#[proc_macro_attribute]`)
  - Derive macros (`#[proc_macro_derive]`)

## Native macros

The standard library's macros are natively implemented instead of using procedural macros. It may implement a few using declarative macros though, which is fine, as it does not use an external process.

There are specially these native macros:

- `#[derive(Serialize, Deserialize)]`

Most method macros are native as well.

## Declarative macros

They are similiar to [Rust's Macros by Example](https://doc.rust-lang.org/reference/macros-by-example.html).

## Procedural macros

Procedural macros are written separately in another language, often Rust or JavaScript. A project template should be auto generated via the command line. For example, for Rust, `cargo run` is run at the project path.

_How it is run:_ The separate macro project is run as an _in-demand_ parallel process. That is, once a procedural macro is used, it invokes a command like `cargo run`, keeping it running until the compiler finishes and supplies tokens and receives tokens. Therefore, the macro project in Rust should look like:

```
use ds_proc_macro::{TokenStream, proc_macros};

proc_macros! {
    #[proc_macro]
    fn my_function_macro(input: TokenStream) -> TokenStream {
        output
    }

    #[proc_macro_attribute]
    fn my_attribute_macro(attr_tokens: TokenStream, item_tokens: TokenStream) -> TokenStream {
        output
    }

    #[proc_macro_derive]
    fn my_derive_macro(item_tokens: TokenStream) -> TokenStream {
        TokenStream::try_from("fn answer() -> u32 { 42 }").unwrap()
    }
}
```

Example defining procedural macros in DS:

```
proc_macros! {
    // language can be "rust" currently.
    lang = "rust";
    path = "./project_path";

    #[proc_macro]
    fn my_function_macro();
}
```

## Doc comments

Neither declarative or procedural macros support doc comment tokens, like `/// str`, therefore they are always converted to `#[doc = r"str"]` token streams.

## Declarative method macros

Macros are supported as methods of a type, in which case they are always restricted to instances.

They must use `__self` instead of `self`.

```
impl S {
    macro f => { __self.x() + 1 }
}

let _ = o.f!();
```