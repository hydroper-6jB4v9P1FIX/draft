# Name semantics

- Name groups are divided into three:
  - Module group
  - Macro group
  - Variable, variant or function group
  - Field group
- As you can see, there are many groups. Instead of storing a vector for each, it's less memory expensive to use kinded name-item pairs.
- Any expansion of a module with `use q::*;` contributes to different name groups.
- Any alias of an item `use q::x;` contributes different aliases for each name group.
- A name without a postfix `!` (exclamation mark) resolves to a non-macro.
- A name with a postfix `!` resolves to a macro.

## Delegate

When resolving a field or method, if the name does not exist in the current base and it implements `Delegate`, it delegates resolution to that base.