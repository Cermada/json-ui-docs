# Hiding UI Elements

<span class="badge beginner">Beginner</span>

Hiding elements is one of the most common things people want to do with JSON UI — remove the paper doll, strip the XP bar, hide the boss bar when not in a boss fight, or create a clean "minimal HUD" look.

There are two techniques: **setting `visible: false`** and **using modifications to remove**. Each has its place.

---

## Method 1: `visible: false`

The simplest approach. Override the element and set it invisible:

```json
{
    "namespace": "hud",

    "paper_doll": {
        "visible": false
    }
}
```

**Pros:**
- Simple, one-line change
- Reversible — just set `visible: true` to bring it back
- Works for elements that have visibility bindings

**Cons:**
- The element still exists in the UI tree (takes up memory, receives events)
- Children are hidden too, which is usually fine
- Some elements ignore `visible: false` and control their own visibility internally

---

## Method 2: `modifications` Remove

Surgically removes the element from its parent's `controls` array:

```json
"parent_panel": {
    "modifications": [
        {
            "array_name": "controls",
            "operation": "remove",
            "target": "paper_doll@hud.paper_doll"
        }
    ]
}
```

**Pros:**
- Truly removes the element — it doesn't exist in the tree at all
- Slightly more efficient than `visible: false`

**Cons:**
- You need to know the parent element's name
- The `target` must exactly match the child's key in the controls array
- Harder to undo

---

## Finding the Right Element Name

This is the tricky part. You need to find:
1. The element you want to hide
2. Its parent element (for the `modifications` approach)

**How to find it:**
1. Open the vanilla file for the screen in question (e.g., `hud_screen.json`)
2. Use `Ctrl+F` to search for keywords related to what you want to hide
3. Follow the element references up the tree to find the parent

### Common HUD Elements to Hide

| What to Hide | Element Name | File |
|---|---|---|
| Paper doll (character model) | `paper_doll` | `hud_screen.json` |
| XP bar | `experience_bar` | `hud_screen.json` |
| Boss bar | `boss_bars` | `hud_screen.json` |
| Crosshair | `crosshair_container` | `hud_screen.json` |
| Chat messages | `chat_text_display` | `hud_screen.json` |
| Action bar text | `actionbar_text` | `hud_screen.json` |
| Title/subtitle text | `title_text` / `subtitle_text` | `hud_screen.json` |
| Hunger bar | `hunger_bar` | `hud_screen.json` |
| Armor bar | `armor_bar` | `hud_screen.json` |

> **Note:** Element names change between Minecraft versions. Always verify against the vanilla files for your version.

---

## Hiding Conditionally with Bindings

Instead of permanently hiding, you can hide based on game state. This is what vanilla does — the boss bar is hidden unless a boss is nearby:

```json
"boss_bars": {
    "bindings": [
        {
            "binding_name": "#boss_bars_visible",
            "binding_name_override": "#visible"
        }
    ]
}
```

You can override an element's visibility binding to always be `false` — effectively always hiding it:

```json
"boss_bars": {
    "visible": false,
    "bindings": []
}
```

Setting `"bindings": []` clears the vanilla bindings so they can't override your `"visible": false`.

---

## Example: A Minimal HUD

Here's a complete example that strips down the HUD to just the hotbar:

```json
{
    "namespace": "hud",

    "paper_doll": {
        "visible": false
    },

    "experience_bar": {
        "visible": false
    },

    "armor_bar": {
        "visible": false
    },

    "hunger_bar": {
        "visible": false
    },

    "boss_bars": {
        "visible": false
    },

    "crosshair_container": {
        "visible": false
    }
}
```

Save this as `ui/hud_screen.json` in your pack (with `_ui_defs.json` registered). Load the game and you'll have a clean, minimal HUD with only the hotbar visible.

---

## Hiding Pause Menu Buttons

The same technique works for any screen. To remove the "Feedback" button from the pause menu:

```json
{
    "namespace": "pause",

    "feedback_button": {
        "visible": false
    }
}
```

Register `ui/pause_screen.json` in your `_ui_defs.json`.

---

## Troubleshooting

**Element is still visible:**
- The element name might be different — search the vanilla file
- The element might override its own visibility internally
- Try the `modifications` remove approach instead

**Game UI breaks after hiding:**
- You hid a parent element that other things depend on
- Try hiding only the specific visual child, not the containing panel

**Changes don't appear:**
- Check `_ui_defs.json` has your file registered
- Verify namespace matches the file's namespace

---

Next: [Building a Custom Screen →](guides/custom-screen.md)
