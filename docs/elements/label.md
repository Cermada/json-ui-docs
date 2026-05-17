# Label

<span class="badge beginner">Beginner</span>

A **label** displays text on screen. It is one of the most-used elements — every piece of visible text in the game's UI is a label.

## Basic Usage

```json
"my_label": {
    "type": "label",
    "text": "Hello, World!",
    "color": [1, 1, 1, 1]
}
```

## Text

The `text` property sets the displayed string. It can be:

**Static text:**
```json
"text": "Score: 100"
```

**Localization key** (fetches from `.lang` files):
```json
"text": "menu.start"
```

**A variable:**
```json
"text": "$label_text"
```

**Dynamic via binding:**
```json
"text": "0",
"bindings": [
    {
        "binding_name": "#player_health",
        "binding_name_override": "#text"
    }
]
```

## Color

Colors use RGBA format, each channel from `0.0` to `1.0`:

```json
"color": [1, 1, 1, 1]       // white
"color": [1, 0, 0, 1]       // red
"color": [0, 1, 0, 1]       // green
"color": [1, 1, 0, 1]       // yellow
"color": [0.5, 0.5, 0.5, 1] // gray
"color": [1, 1, 1, 0.5]     // white, 50% transparent
```

## Font Type

Controls the font rendering style:

```json
"font_type": "smooth"
```

| Value | Description |
|---|---|
| `"smooth"` | Modern anti-aliased font (most UI text) |
| `"default"` | Minecraft's classic bitmap font |
| `"rune"` | Enchantment table font (unreadable glyphs) |
| `"unicode"` | Unicode/international characters |
| `"MinecraftSeven"` | Pixel-perfect Minecraft font |

## Shadow

Adds a drop shadow for readability:

```json
"shadow": true
```

## Text Alignment

```json
"text_alignment": "center"
```

Values: `"left"` (default), `"center"`, `"right"`

Note: alignment only has a visible effect when the label has a fixed width larger than its text content.

## Font Scale Factor

Scale the text size relative to its parent:

```json
"font_scale_factor": 1.5
```

`1.0` is normal size. `2.0` is double. This is more reliable than trying to change font size directly.

## Line Breaking

Labels wrap text automatically. Control the max width before wrapping:

```json
"size": [150, "default"],
"localize": true
```

Setting a fixed width on the label causes it to wrap at that width. Set height to `"default"` so it grows with the text.

## Binding to Game Data

Labels are the primary way to display live game data as text:

```json
"health_label": {
    "type": "label",
    "text": "♥ 20",
    "color": [1, 0.3, 0.3, 1],
    "shadow": true,
    "font_type": "smooth",
    "bindings": [
        {
            "binding_name": "#player_health",
            "binding_name_override": "#text"
        }
    ]
}
```

## Full Example

```json
"level_display": {
    "type": "label",
    "text": "Level 1",
    "color": [0.5, 1, 0.2, 1],
    "font_type": "smooth",
    "font_scale_factor": 1.2,
    "shadow": true,
    "text_alignment": "center",
    "anchor_from": "bottom_middle",
    "anchor_to": "bottom_middle",
    "offset": [0, -20],
    "bindings": [
        {
            "binding_name": "#player_level",
            "binding_name_override": "#text"
        }
    ]
}
```

A green label showing the player's current level, centered, with shadow, slightly scaled up, 20px from the bottom of its parent.

---

Next: [Image →](elements/image.md)
