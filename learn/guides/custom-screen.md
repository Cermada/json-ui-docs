# Building a Custom Screen

<span class="badge advanced">Advanced</span>

Building a custom screen means creating a new UI panel — typically injected into an existing screen — with your own layout, elements, and logic. This is where everything you've learned comes together.

## What "Custom Screen" Means in JSON UI

JSON UI doesn't let you register entirely new game screens from scratch (that requires scripting or native code). What you *can* do is:

1. **Add a custom overlay panel** to an existing screen (like the pause menu or HUD)
2. **Completely replace the visual contents** of an existing screen
3. **Create a new-looking screen** by gutting the content of a simple screen and filling it with your own

For most creators, the goal is option 1 or 2 — which gives you enormous flexibility.

---

## Planning Your Screen

Before writing any JSON, sketch (even mentally) what you want:

1. What screen will it appear on? (pause menu, HUD, inventory?)
2. What should it contain? (text, buttons, images?)
3. How is it laid out? (centered, anchored to a corner, full-screen?)
4. Does it need live data? (health, player name, scores?)

---

## Example: A Custom Info Panel in the Pause Menu

We'll add a panel to the pause menu that shows a custom server message:

```
┌─────────────────────────────┐
│  Minecraft Pause Menu       │
│                             │
│  [Resume] [Achievements]    │
│  [Settings] [Quit]          │
│                             │
│  ┌─────────────────────┐   │
│  │  Welcome, Player!   │   │  ← our custom panel
│  │  Have a great game. │   │
│  └─────────────────────┘   │
└─────────────────────────────┘
```

### Step 1: Create the Panel File

Create `ui/pause_screen.json` in your pack:

```json
{
    "namespace": "pause",

    "custom_info_panel_bg": {
        "type": "image",
        "texture": "textures/ui/dialog_background",
        "nineslice_size": [4, 4, 4, 4],
        "size": ["100%", "100%"],
        "color": [0, 0, 0, 0.6]
    },

    "custom_info_title": {
        "type": "label",
        "text": "Welcome, Player!",
        "color": [1, 0.85, 0.2, 1],
        "font_type": "smooth",
        "shadow": true,
        "font_scale_factor": 1.3,
        "anchor_from": "top_middle",
        "anchor_to": "top_middle",
        "offset": [0, 12]
    },

    "custom_info_body": {
        "type": "label",
        "text": "Have a great game. Explore, survive, thrive.",
        "color": [0.9, 0.9, 0.9, 1],
        "font_type": "smooth",
        "anchor_from": "center",
        "anchor_to": "center",
        "offset": [0, 4]
    },

    "custom_info_panel": {
        "type": "panel",
        "size": [220, 70],
        "anchor_from": "bottom_middle",
        "anchor_to": "bottom_middle",
        "offset": [0, -16],
        "controls": [
            { "bg@pause.custom_info_panel_bg": {} },
            { "title@pause.custom_info_title": {} },
            { "body@pause.custom_info_body": {} }
        ]
    }
}
```

### Step 2: Inject Into the Pause Screen

Find the pause menu's root panel by searching the vanilla `pause_screen.json` for the main content area — usually something like `pause_menu_content` or `menu_main`.

Then use modifications to insert your panel:

```json
"pause_menu_content": {
    "modifications": [
        {
            "array_name": "controls",
            "operation": "insert_back",
            "value": [
                { "info_panel@pause.custom_info_panel": {} }
            ]
        }
    ]
}
```

### Step 3: Register the File

In `ui/_ui_defs.json`:

```json
{
    "ui_defs": [
        "ui/pause_screen.json"
    ]
}
```

---

## Example: Full-Screen Custom HUD Overlay

A full-screen overlay sits above all other HUD elements. Useful for custom minimaps, dashboards, or overlays:

```json
{
    "namespace": "myhud",

    "full_overlay": {
        "type": "panel",
        "size": ["100%", "100%"],
        "anchor_from": "center",
        "anchor_to": "center",
        "layer": 100,
        "controls": [
            { "top_bar@myhud.top_info_bar": {} },
            { "bottom_bar@myhud.bottom_info_bar": {} }
        ]
    },

    "top_info_bar": {
        "type": "panel",
        "size": ["100%", 20],
        "anchor_from": "top_middle",
        "anchor_to": "top_middle",
        "controls": [
            {
                "bg": {
                    "type": "image",
                    "texture": "textures/ui/black",
                    "size": ["100%", "100%"],
                    "color": [0, 0, 0, 0.5]
                }
            },
            {
                "title": {
                    "type": "label",
                    "text": "My Server",
                    "color": [1, 1, 1, 0.9],
                    "font_type": "smooth",
                    "anchor_from": "center",
                    "anchor_to": "center"
                }
            }
        ]
    }
}
```

Then in `hud_screen.json`:

```json
{
    "namespace": "hud",

    "hud_content": {
        "modifications": [
            {
                "array_name": "controls",
                "operation": "insert_back",
                "value": [
                    { "my_overlay@myhud.full_overlay": {} }
                ]
            }
        ]
    }
}
```

And register **both** `ui/hud_screen.json` and `ui/my_overlay_screen.json` in `_ui_defs.json`.

---

## Tips for Custom Screens

**Use high `layer` values for overlays.** The default layer is 0. Setting layer to 50 or 100 ensures your overlay appears above vanilla elements.

**Use `nineslice` for scalable backgrounds.** A plain color `image` with `color` tint and `nineslice_size` is the cleanest way to make resizable backgrounds.

**Keep namespace unique.** If your namespace is `hud`, your elements might conflict with vanilla ones. Use a custom namespace like `myhud` or your pack's name for new elements.

**Test on multiple screen sizes.** Use percentage sizes and anchors rather than fixed pixel values to ensure your screen looks right on phones, tablets, and PCs.

---

## What's Next?

You now have the skills to:
- Create any UI element
- Inject it anywhere in the interface
- Drive it with live game data
- Build complete custom overlays

The limit is your imagination and the bindings the engine exposes. Check the [Further Learning](reference/further-learning.md) page for community resources with more advanced techniques.

---

Next: [Common Properties Reference →](reference/properties.md)
