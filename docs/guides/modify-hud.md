# Modifying the HUD

<span class="badge intermediate">Intermediate</span>

The HUD (Heads-Up Display) is everything you see while playing — health bar, hunger, hotbar, XP bar, crosshair, and more. This guide walks through the most common HUD modifications with working code examples.

## Before You Start

Make sure you have:
- A resource pack set up ([Creating a Resource Pack](getting-started/first-resource-pack.md))
- `ui/_ui_defs.json` registering `"ui/hud_screen.json"`
- A copy of the vanilla `hud_screen.json` open as reference

All modifications in this guide go into your pack's `ui/hud_screen.json` with `"namespace": "hud"` at the top.

---

## Moving the Hotbar

The hotbar panel is called `hotbar_and_slot_items` in the vanilla HUD. To move it, override its anchor and offset:

```json
{
    "namespace": "hud",

    "hotbar_and_slot_items": {
        "anchor_from": "top_middle",
        "anchor_to": "top_middle",
        "offset": [0, 8]
    }
}
```

This moves the hotbar to the top-center of the screen, 8px from the top edge.

> **Tip:** The default position is `"anchor_to": "bottom_middle"`. Change only what you need — all other properties stay vanilla.

---

## Resizing the Hotbar

To make the hotbar larger, change the `size` property. But the hotbar uses a grid, so you'll need to coordinate the grid item sizes too. An easier approach is scaling with `font_scale_factor` (for text) or wrapping in a scaled panel.

For a simple visual scale, try overriding the `size` of the hotbar container:

```json
"hotbar": {
    "size": [200, 24]
}
```

---

## Changing Health Bar Color

The health bar uses heart images. To tint all heart icons red-to-orange:

```json
"hearts": {
    "color": [1, 0.5, 0, 1]
}
```

Find the exact element name by searching `hud_screen.json` for the heart renderer.

---

## Adding a Custom Text Overlay

Let's add a persistent label to the HUD showing a custom message:

**Step 1:** Define the label element:

```json
"my_custom_label": {
    "type": "label",
    "text": "♦ Custom HUD",
    "color": [0.4, 0.9, 0.4, 0.8],
    "font_type": "smooth",
    "shadow": true,
    "anchor_from": "top_right",
    "anchor_to": "top_right",
    "offset": [-6, 6]
}
```

**Step 2:** Inject it into the HUD's root using modifications:

```json
"hud_content": {
    "modifications": [
        {
            "array_name": "controls",
            "operation": "insert_front",
            "value": [
                { "custom_label@hud.my_custom_label": {} }
            ]
        }
    ]
}
```

> **Finding the right parent:** Search the vanilla `hud_screen.json` for `"hud_content"` or `"root_panel"`. The name of the HUD's root container varies slightly by game version.

---

## Displaying Player Health as a Number

```json
"health_number_display": {
    "type": "label",
    "text": "20",
    "color": [1, 0.3, 0.3, 1],
    "font_type": "smooth",
    "shadow": true,
    "anchor_from": "bottom_left",
    "anchor_to": "bottom_left",
    "offset": [4, -28],
    "bindings": [
        {
            "binding_name": "#player_health",
            "binding_name_override": "#text"
        }
    ]
}
```

Then inject this with modifications just like the custom label above.

---

## Moving the XP Bar

```json
"experience_bar": {
    "anchor_from": "top_middle",
    "anchor_to": "top_middle",
    "offset": [0, 32]
}
```

---

## Hiding the Paper Doll

The paper doll is the small character model shown in the HUD. To remove it:

```json
"paper_doll_and_hotbar": {
    "modifications": [
        {
            "array_name": "controls",
            "operation": "remove",
            "target": "paper_doll@hud.paper_doll"
        }
    ]
}
```

> The exact element name may vary. Search `hud_screen.json` for `"paper_doll"` to confirm.

---

## Hiding the Crosshair

```json
"crosshair_container": {
    "visible": false
}
```

Or more surgically, use modifications to remove it from its parent panel.

---

## Testing Your Changes

1. Save your `hud_screen.json`
2. Load Minecraft and enter a world with your pack active
3. Look for your changes

If nothing changed:
- Check `_ui_defs.json` lists your file
- Verify the namespace is `"hud"` (matching `hud_screen.json`)
- Look for JSON syntax errors in VS Code

If the HUD disappeared entirely:
- You have a syntax error or referenced a non-existent element
- Check VS Code for red underlines

---

## Full Working Example: Minimal HUD File

```json
{
    "namespace": "hud",

    "my_health_label": {
        "type": "label",
        "text": "20",
        "color": [1, 0.3, 0.3, 1],
        "font_type": "smooth",
        "shadow": true,
        "anchor_from": "bottom_left",
        "anchor_to": "bottom_left",
        "offset": [4, -28],
        "bindings": [
            {
                "binding_name": "#player_health",
                "binding_name_override": "#text"
            }
        ]
    },

    "hud_content": {
        "modifications": [
            {
                "array_name": "controls",
                "operation": "insert_front",
                "value": [
                    { "health_label@hud.my_health_label": {} }
                ]
            }
        ]
    }
}
```

This adds a live health number to the HUD without touching anything else.

---

Next: [Hiding UI Elements →](guides/hiding-elements.md)
