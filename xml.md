# XML

DS derives the E4X (ECMAScript for XML) standard.

## Literal

The XML literal is not compliant to XML and these are the main differences:

- Any text node has to be written as a string literal, like `<>"Some text"</>`.
- Interpolations are supported by using braces.

Example:

```
let node = xml!(<></>);
```

## Qualified names and properties

Syntax for qualified names:

```
let _ = xml::qname!(k);
let _ = xml::qname!([k]); // computed key
let _ = xml::qname!(q:k);
let _ = xml::qname!((qns):k); // computed qualifier
let _ = xml::qname!((qns):[k]);
```

Syntax for properties is taken from `xml::qname!`, with an optional attribute prefix (`@`). The operators take a chain of properties directly; for example, `tag1.tag2.@attr`.

```
let _ = node.get!(p);
node.set!(p, v);
let _ = node.delete!(p); // bool
let _ = node.has!(p);
```

## Query

```
// the `filter!` method macro defines a submacro
// `q!`, used for retrieving properties in few characters.
let _ = node.filter!(q!(p) == "v");

let _ = node.descendants!(p); // descendants
```