# Char type

The `char` type, an Unicode Scalar Value, always maps to an ActionScript `uint`. This means that the subtype operations `cast!`, `try_cast!` and `is_type!` will never detect `char`.

This implies that `[char]` translates to ActionScript `Vector.<uint>`.