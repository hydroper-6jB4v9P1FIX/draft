# AIR API

## Overview

- `#[derive(RefEq)]` for `DisplayObject`

## Geometry

- `pub struct Vector2d(pub f64, pub f64);`

## Display object paths

- Display object paths are based on `name` and take slashes as separators and special `..` portions for stepping backwards in the display list.
- `display_object.get_path()`
- `display_object.resolve_path()`
- `display_object.position()` (Vector2d)
- `display_object.set_position()` (Vector2d)