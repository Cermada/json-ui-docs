# Common Properties Reference

<span class="badge beginner">Beginner</span>

A quick-reference table of the properties you'll use most often across all element types.

---

## Universal Properties (Work on All Types)

| Property | Type | Description | Example |
|---|---|---|---|
| `type` | string | Element type | `"panel"`, `"label"`, `"image"` |
| `size` | [w, h] | Element dimensions | `[100, 50]`, `["100%", 20]` |
| `anchor_from` | string | Anchor point on this element | `"center"`, `"top_left"` |
| `anchor_to` | string | Anchor point on parent | `"bottom_right"` |
| `offset` | [x, y] | Pixel offset from anchor | `[10, -5]` |
| `visible` | bool | Whether element renders | `true`, `false` |
| `enabled` | bool | Whether element takes layout space | `true`, `false` |
| `layer` | int | Z-order (higher = on top) | `1`, `10`, `100` |
| `clips_children` | bool | Clip children to element bounds | `false` |
| `bindings` | array | Live data connections | See [Bindings](core-concepts/bindings.md) |
| `controls` | array | Child elements | See [How JSON UI Works](core-concepts/how-json-ui-works.md) |
| `modifications` | array | Patch children of named element | See [Modifications](core-concepts/modifications.md) |

---

## Anchor Values

```
"top_left"    | "top_middle"    | "top_right"
"left_middle" | "center"        | "right_middle"
"bottom_left" | "bottom_middle" | "bottom_right"
```

---

## Size Syntax

| Syntax | Meaning |
|---|---|
| `[200, 100]` | Fixed 200×100 pixels |
| `["100%", 50]` | Full parent width, 50px tall |
| `["50%", "50%"]` | Half parent in both dimensions |
| `["100% - 16px", 20]` | Parent width minus 16px |
| `["default", "default"]` | Size to content |

---

## Label Properties

| Property | Type | Description | Example |
|---|---|---|---|
| `text` | string | Displayed text | `"Hello!"` |
| `color` | [r, g, b, a] | Text color (0.0–1.0) | `[1, 1, 0, 1]` |
| `font_type` | string | Font style | `"smooth"`, `"default"` |
| `shadow` | bool | Drop shadow | `true` |
| `font_scale_factor` | float | Text size multiplier | `1.5` |
| `text_alignment` | string | Horizontal alignment | `"left"`, `"center"`, `"right"` |
| `localize` | bool | Treat text as localization key | `true` |

---

## Image Properties

| Property | Type | Description | Example |
|---|---|---|---|
| `texture` | string | Path to PNG (no extension) | `"textures/ui/my_icon"` |
| `color` | [r, g, b, a] | Tint color | `[1, 0.5, 0, 1]` |
| `uv` | [x, y] | Sprite sheet source point | `[18, 0]` |
| `uv_size` | [w, h] | Sprite sheet region size | `[18, 18]` |
| `nineslice_size` | [t, r, b, l] | Nine-slice border | `[4, 4, 4, 4]` |
| `fill` | bool | Progress bar fill mode | `true` |
| `keep_ratio` | bool | Preserve aspect ratio | `true` |
| `grayscale` | bool | Render in grayscale | `true` |

---

## Stack Panel Properties

| Property | Type | Description | Example |
|---|---|---|---|
| `orientation` | string | Layout direction | `"vertical"`, `"horizontal"` |

---

## Grid Properties

| Property | Type | Description | Example |
|---|---|---|---|
| `grid_dimensions` | [cols, rows] | Grid size | `[9, 1]` |
| `grid_item_template` | string | Template element | `"ns.slot"` |
| `collection_name` | string | Data collection | `"hotbar_items"` |

---

## Color Reference

| Color | Value |
|---|---|
| White | `[1, 1, 1, 1]` |
| Black | `[0, 0, 0, 1]` |
| Red | `[1, 0, 0, 1]` |
| Green | `[0, 1, 0, 1]` |
| Blue | `[0, 0, 1, 1]` |
| Yellow | `[1, 1, 0, 1]` |
| Orange | `[1, 0.5, 0, 1]` |
| Purple | `[0.5, 0, 1, 1]` |
| Gray | `[0.5, 0.5, 0.5, 1]` |
| Transparent | `[0, 0, 0, 0]` |
| Semi-transparent black | `[0, 0, 0, 0.7]` |

**Formula:** Convert 0–255 to 0.0–1.0 by dividing by 255.
- `rgb(255, 128, 0)` → `[1.0, 0.502, 0, 1]`

---

## Font Types

| Value | Description |
|---|---|
| `"smooth"` | Anti-aliased modern font (recommended) |
| `"default"` | Classic Minecraft bitmap font |
| `"rune"` | Enchantment table glyphs |
| `"unicode"` | Full Unicode character support |
| `"MinecraftSeven"` | Pixel-perfect Minecraft style |
| `"MinecraftTen"` | Larger Minecraft style |

---

## Binding Condition Values

| Value | Description |
|---|---|
| `"always"` | Update every frame |
| `"always_when_visible"` | Update only when element is visible |
| `"once"` | Update once when element first renders |
| `"visibility_changed"` | Update when visibility changes |

---

## Modifications Operations

| Operation | Description |
|---|---|
| `"insert_front"` | Add to beginning of array |
| `"insert_back"` | Add to end of array |
| `"insert_before"` | Add before target element |
| `"insert_after"` | Add after target element |
| `"remove"` | Remove target element |

---

Next: [Important UI Files →](reference/ui-files.md)
