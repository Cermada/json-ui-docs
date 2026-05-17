# Bindings — Live Data

<span class="badge intermediate">Intermediate</span>

Bindings are how JSON UI connects to **live game data**. Without bindings, every UI element shows static content. With bindings, a label can display the player's current health, an image can change based on armor type, or an element can appear and disappear based on game state.

## The Problem Bindings Solve

Static JSON can only show fixed values. But the HUD needs to show the player's current health — which changes constantly. The XP bar needs to fill up as you gain experience. The hotbar needs to highlight the selected slot.

Bindings are the bridge between the game engine's live data and the UI renderer.

## What Is a Binding?

A binding is a connection between a **source** (a value from the game engine) and a **target** (a property on a UI element).

```json
"my_health_label": {
    "type": "label",
    "text": "0",
    "bindings": [
        {
            "binding_name": "#player_health",
            "binding_name_override": "#text"
        }
    ]
}
```

This says: "take the value of `#player_health` from the engine, and use it as this label's `#text` (the displayed text)."

## Binding Names

Binding names start with `#`. They are string identifiers that the game engine publishes as observable data. The UI system subscribes to them.

Some commonly used bindings:

| Binding Name | What It Provides |
|---|---|
| `#player_health` | Current health (number) |
| `#player_max_health` | Maximum health (number) |
| `#player_hunger` | Current hunger/food level |
| `#player_level` | Current XP level |
| `#player_experience_progress` | XP bar progress (0.0 to 1.0) |
| `#selected_hotbar_slot` | Currently selected hotbar slot index |
| `#hotbar_slot_count` | Number of hotbar slots |
| `#is_holding_item` | Whether player is holding an item |

> Binding names are defined by the game engine in C++. You cannot create new binding sources — only use the ones the engine exposes.

## The Two Parts of a Binding

Every binding has two parts:

```json
{
    "binding_name": "#source_value",
    "binding_name_override": "#target_property"
}
```

- **`binding_name`** — the source: what data you're reading from the engine
- **`binding_name_override`** — the target: which property of this element to set

If you omit `binding_name_override`, the binding applies to the property with the same name as the source — which is often what you want.

## Binding Types

### Direct property binding

The most common form. Maps an engine value directly to an element property:

```json
"bindings": [
    {
        "binding_name": "#player_health",
        "binding_name_override": "#text"
    }
]
```

### Visibility binding

A common pattern is showing/hiding elements based on game state:

```json
"boss_bar_panel": {
    "type": "panel",
    "bindings": [
        {
            "binding_name": "#boss_bars_visible",
            "binding_name_override": "#visible"
        }
    ]
}
```

When `#boss_bars_visible` is `false`, the panel (and all its children) disappears.

### Conditional binding (`binding_condition`)

Sometimes you only want a binding to apply under certain conditions:

```json
"bindings": [
    {
        "binding_name": "#player_health",
        "binding_name_override": "#text",
        "binding_condition": "always_when_visible"
    }
]
```

Common `binding_condition` values:
- `"always"` — update on every frame
- `"always_when_visible"` — update only when the element is visible
- `"once"` — update once when first shown

## Example: Health Display

Here's a label that shows the player's current health as a number:

```json
"health_display": {
    "type": "label",
    "text": "20",
    "color": [1, 0.3, 0.3, 1],
    "bindings": [
        {
            "binding_name": "#player_health",
            "binding_name_override": "#text"
        }
    ]
}
```

When the player's health is 18, this label shows "18". When they heal to 20, it shows "20". The binding handles all updates automatically.

## Example: Conditional Visibility

Show a warning label only when health is low:

```json
"low_health_warning": {
    "type": "label",
    "text": "Low Health!",
    "color": [1, 0, 0, 1],
    "bindings": [
        {
            "binding_name": "#is_showing_hud_elements",
            "binding_name_override": "#visible"
        }
    ]
}
```

## Bindings on Images (Progress Bars)

Images can use bindings to control their UV region — this is how progress bars work. The `#texture_file_system` and `fill_image` type controls create bars that fill based on a value:

```json
"xp_progress_bar": {
    "type": "image",
    "texture": "textures/ui/experience_bar_progress",
    "bindings": [
        {
            "binding_name": "#player_experience_progress",
            "binding_name_override": "#fill_amount"
        }
    ]
}
```

## Where to Find Available Bindings

The best way to discover available bindings is to look at the vanilla UI files — search for `"binding_name"` in any screen file to see all the bindings that screen uses. The Bedrock Wiki also maintains a list of common bindings.

## Key Takeaways

- Bindings start with `#`
- They connect engine data (source) to element properties (target)
- They update automatically — you don't control when
- You can't create new binding sources, only use what the engine exposes
- Visibility, text, progress, and color can all be driven by bindings

---

Next: [Variables & Templates →](core-concepts/variables.md)
