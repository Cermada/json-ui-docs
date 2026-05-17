# Button

<span class="badge intermediate">Intermediate</span>

A **button** is an interactive element that responds to clicks, taps, and controller input. Buttons have multiple visual states and trigger actions when activated.

## Why Buttons Are Different

Unlike panels and labels, buttons are interactive. The game engine handles:
- Detecting when the button is hovered, pressed, or released
- Switching between visual states automatically
- Triggering the bound action on press

Your job in JSON is to define what the button looks like in each state and what it does when pressed.

## The Button States

Every button has four visual states. You define a child element for each:

| State | When | Variable |
|---|---|---|
| Default | Not hovered, not pressed | (base element) |
| Hover | Mouse is over the button | Usually a lighter version |
| Pressed | Currently being clicked | Slightly darker/shifted |
| Locked | Button is disabled | Grayed out |

## Extending a Vanilla Button

In practice, you almost never build a button from scratch. You extend a vanilla button base:

```json
"my_confirm_button@common_buttons.light_button_3": {
    "size": [120, 30],
    "$button_text": "Confirm",
    "$pressed_button_name": "button.confirm_action"
}
```

Common vanilla button bases:
- `common_buttons.light_button_3` — standard menu button
- `common_buttons.dark_button` — darker button style
- `common.button` — the most basic button base

## Building a Custom Button

If you need a fully custom look, here's the pattern:

```json
"my_custom_button": {
    "type": "button",
    "size": [100, 30],
    "$pressed_button_name": "button.menu_exit",
    "$button_text|default": "Click",

    "controls": [
        {
            "default@ns.button_bg_default": {
                "layer": 0
            }
        },
        {
            "hover@ns.button_bg_hover": {
                "layer": 0
            }
        },
        {
            "pressed@ns.button_bg_pressed": {
                "layer": 0
            }
        },
        {
            "label@ns.button_label": {
                "text": "$button_text",
                "layer": 1
            }
        }
    ],

    "button_mappings": [
        {
            "from_button_id": "button.menu_select",
            "to_button_id": "$pressed_button_name",
            "mapping_type": "pressed"
        }
    ]
}
```

## Button Mappings

`button_mappings` defines what happens when the button is interacted with. The most common mapping triggers an action when the button is pressed:

```json
"button_mappings": [
    {
        "from_button_id": "button.menu_select",
        "to_button_id": "button.menu_exit",
        "mapping_type": "pressed"
    }
]
```

- `from_button_id` — the input event to listen for (`"button.menu_select"` = click or A button)
- `to_button_id` — the action to fire (must be a registered game action)
- `mapping_type` — `"pressed"` (on release) or `"held"` (while held)

## Common Button Action IDs

| Action ID | What It Does |
|---|---|
| `"button.menu_exit"` | Close the current screen |
| `"button.menu_select"` | Confirm / Select |
| `"button.menu_ok"` | OK / Accept |
| `"button.menu_cancel"` | Cancel |

## Making a Button Look Like Something Else

The simplest approach: extend a vanilla button base, override its visual child elements, and set the text:

```json
"icon_button@common.button": {
    "size": [24, 24],
    "$pressed_button_name": "button.menu_select",

    "controls": [
        {
            "icon@ns.icon_image": {}
        }
    ]
}
```

## Toggling Button Visibility

To show/hide a button based on game state:

```json
"my_button@common_buttons.light_button_3": {
    "$button_text": "Craft",
    "bindings": [
        {
            "binding_name": "#craftable",
            "binding_name_override": "#visible"
        }
    ]
}
```

## Full Example: Custom Exit Button

```json
"exit_button@common_buttons.light_button_3": {
    "size": [160, 40],
    "$button_text": "Back to Menu",
    "$pressed_button_name": "button.menu_exit",
    "anchor_from": "bottom_middle",
    "anchor_to": "bottom_middle",
    "offset": [0, -20]
}
```

A standard-styled exit button, centered at the bottom of its parent, 20px from the bottom edge.

---

Next: [Stack Panel & Grid →](elements/stack-panel.md)
