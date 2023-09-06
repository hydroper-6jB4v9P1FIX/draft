# Embedding files

_String and bytes:_ The `include_str!` and `include_bytes!` macros should contribute binary data to the compiled SWF. For, `include_str!`, the string is loaded from the binary data and parsed via `bytes.read_utf8(bytes.len())`.

## Fonts

_Fonts:_ A macro for including fonts statically should be improved in the future (based on [airsdk.dev](https://airsdk.dev/docs/development/text/embedding-fonts)). According to the SWF 19 specification, the `DefineFont` tag requires parsing of the font file and adapting it to what SWF takes, like shape outlines for each glyph.

A font can be embedded as binary data and loaded dynamically via ActionScript `Font.registerFont(baClass)`. It's not possible to specify things such as custom font name and custom CSS font family.

```
include_font! {
    path = "./font.ttf",
}
```

It's also possible to include multiple fonts from a SWF:

```
include_font! {
    path = "./fonts.swf",
    load_classes = ["FontClass1", "FontClassN"],
}
```