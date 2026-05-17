# Image

<span class="badge beginner">Beginner</span>

An **image** element renders a texture (PNG file) on screen. Images are used for icons, backgrounds, borders, HUD elements, and progress bars.

## Basic Usage

```json
"my_image": {
    "type": "image",
    "texture": "textures/ui/my_icon",
    "size": [32, 32]
}
```

The `texture` path is relative to your resource pack root and has **no file extension**.

## Referencing Textures

### From your resource pack
```json
"texture": "textures/ui/my_custom_icon"
```
This loads `textures/ui/my_custom_icon.png` from your pack.

### Vanilla textures (already in the game)
```json
"texture": "textures/ui/heart_half"
```
You can reference any texture from the vanilla pack — no need to copy it.

## Size and Scaling

```json
"size": [32, 32]       // fixed 32x32 pixels
"size": ["100%", 4]    // full parent width, 4px tall (e.g. a divider line)
"size": ["default", "default"]  // native texture size
```

## Tinting with Color

You can tint an image with a color overlay:

```json
"my_image": {
    "type": "image",
    "texture": "textures/ui/white_square",
    "color": [0.2, 0.6, 1, 0.8],
    "size": [100, 4]
}
```

This is how solid-colored backgrounds are made — use a white texture and tint it to any color. The alpha channel controls transparency.

## UV Mapping (Sprite Sheets)

Many vanilla textures are **sprite sheets** — one PNG containing many smaller images arranged in a grid. Use `uv` and `uv_size` to select a region:

```json
"my_sprite": {
    "type": "image",
    "texture": "textures/ui/icons",
    "uv": [0, 0],
    "uv_size": [18, 18],
    "size": [18, 18]
}
```

- `uv` — the top-left corner of the region in the sprite sheet, in pixels `[x, y]`
- `uv_size` — the width and height of the region in pixels `[w, h]`

### Example: Selecting the second icon in a row

If your sprite sheet has 18×18px icons in a row:
```json
"uv": [18, 0]   // skip the first icon (18px wide)
"uv_size": [18, 18]
```

## Nine-Slice Scaling

For backgrounds and borders that need to scale without distorting corners, use nine-slice:

```json
"my_bg": {
    "type": "image",
    "texture": "textures/ui/dialog_background",
    "nineslice_size": [4, 4, 4, 4],
    "size": [200, 150]
}
```

`nineslice_size` specifies the border thickness in pixels: `[top, right, bottom, left]`. The corners are kept at their original size; the middle stretches to fill.

## Fill (Progress Bars)

Images can act as fill bars by clipping from one side:

```json
"xp_bar_fill": {
    "type": "image",
    "texture": "textures/ui/xp_bar_fill",
    "fill": true,
    "bindings": [
        {
            "binding_name": "#player_experience_progress",
            "binding_name_override": "#fill_amount"
        }
    ]
}
```

When `fill` is `true`, the `#fill_amount` binding (a value from 0.0 to 1.0) controls how much of the image is visible, from left to right.

## Keeping Aspect Ratio

```json
"keep_ratio": true
```

Prevents the image from stretching when the element size doesn't match the texture's aspect ratio.

## Full Example: Custom Health Icon

```json
"heart_icon": {
    "type": "image",
    "texture": "textures/ui/custom_heart",
    "size": [9, 9],
    "anchor_from": "top_left",
    "anchor_to": "top_left",
    "offset": [0, 0],
    "color": [1, 0.3, 0.3, 1]
}
```

## Summary of Key Properties

| Property | Type | Description |
|---|---|---|
| `texture` | string | Path to PNG file (no extension) |
| `size` | [w, h] | Element dimensions |
| `color` | [r, g, b, a] | Tint color overlay |
| `uv` | [x, y] | Sprite sheet source position |
| `uv_size` | [w, h] | Sprite sheet region size |
| `nineslice_size` | [t, r, b, l] | Nine-slice border sizes |
| `fill` | bool | Enable progress bar fill mode |
| `keep_ratio` | bool | Preserve texture aspect ratio |

---

Next: [Button →](elements/button.md)
