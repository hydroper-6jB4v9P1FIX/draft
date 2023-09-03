# Tree builder

`tree!` constructs typed structures using a syntax loosely similiar to XML with interpolations using braces.

```
let node = tree!(<S/>);
```

- Any `{interpolated}` element between children takes an iterable.

This requires `S` to:

- implement `ds::treebuilder::AddChild`,
- implement either
  - `ds::treebuilder::Assumes`, which assumes that
    - the structure contains a `fn new() -> Self` and
    - the structure contains `set_` methods for all attributes.
  - `ds::treebuilder::Builder<B>`
    - which takes `B` with assumptions from `Assumes`,
    - which assumes `B` has a `result` method (returning `S`) and
    - which requires `fn builder(self) -> B;`