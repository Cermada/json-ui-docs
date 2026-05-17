# File Structure

<span class="badge beginner">Beginner</span>

Understanding how the UI files are organized helps you find the right file to modify — fast. Minecraft's UI is split across many files, each one responsible for a specific screen or feature.

## The `ui/` Folder

All JSON UI files live inside the `ui/` folder of a resource pack. Both vanilla and your custom pack use the same folder name.

```
resource_pack/
└── ui/
    ├── _ui_defs.json         ← master registry of UI files
    ├── _global_variables.json ← shared variables used across files
    ├── hud_screen.json       ← the HUD (health, hotbar, etc.)
    ├── inventory_screen.json ← player inventory
    ├── pause_screen.json     ← pause menu
    ├── start_screen.json     ← main menu / title screen
    ├── common/               ← shared elements used by many screens
    │   ├── common.json
    │   └── ...
    └── ...
```

## The Special Files

### `_ui_defs.json` — The Registry

This file tells Minecraft which JSON files to load and merge. If your UI file isn't listed here, it won't be loaded — period.

```json
{
    "ui_defs": [
        "ui/hud_screen.json",
        "ui/my_custom_screen.json"
    ]
}
```

Every UI file you create must be registered here. The vanilla `_ui_defs.json` lists all the vanilla files. Your pack's `_ui_defs.json` adds your files on top.

> **You don't need to list vanilla files** — they're already registered by vanilla's `_ui_defs.json`. Only list the files you're adding or overriding.

### `_global_variables.json` — Shared Variables

This file defines variables that can be used across all UI files. Think of it as a place to store values that many files need — like a common font size or a shared color.

```json
{
    "$default_panel_color|default": [0.1, 0.1, 0.1, 0.8],
    "$ui_scale|default": 2
}
```

Variables always start with `$`. We'll cover them in depth in the [Variables & Templates](core-concepts/variables.md) page.

## The Screen Files

Each major screen in Minecraft has its own JSON file. Here are the most commonly modified ones:

| File | What It Controls |
|---|---|
| `hud_screen.json` | Health bar, hunger, hotbar, XP bar, chat, crosshair, boss bar |
| `inventory_screen.json` | Player inventory screen |
| `pause_screen.json` | The pause/game menu |
| `start_screen.json` | Main menu (title screen) |
| `chat_screen.json` | Chat input box |
| `death_screen.json` | "You Died" screen |
| `book_screen.json` | Book & Quill writing interface |
| `container_screens.json` | Chests, furnaces, crafting tables |
| `settings_screen.json` | Game settings |
| `server_form.json` | Server forms (NPC dialogs) |

## The `common/` Folder

The `common/` folder contains elements shared by many screens — things like buttons, toggles, scrollbars, and popup dialogs. Instead of each screen defining its own button from scratch, they all extend the common button.

You'll see references like `@common.button` or `@common_buttons.light_button_3` throughout the vanilla files. These all come from the common folder.

You typically don't need to modify common files unless you want to change something globally — like making all buttons have rounded corners.

## How to Find the Right File

**Step 1:** Figure out which *screen* the element is on.
- Is it visible during gameplay? → `hud_screen.json`
- Is it a menu? → check `pause_screen.json`, `start_screen.json`, etc.
- Is it inside a container (chest, furnace)? → `container_screens.json`

**Step 2:** Open the vanilla file and use `Ctrl+F` to search for descriptive keywords.
- Looking for the health bar? Search for `health`
- Looking for the XP bar? Search for `xp` or `experience`
- Looking for a button label? Search for the button's text

**Step 3:** Follow the element references. The screen's root element usually contains other elements, which contain more elements. Trace the hierarchy until you find what you're looking for.

## Namespaces Match File Names (Usually)

Each file declares its namespace at the top. By convention, the namespace usually matches the file name:

- `hud_screen.json` → `"namespace": "hud"`
- `inventory_screen.json` → `"namespace": "inventory"`
- `pause_screen.json` → `"namespace": "pause"`

This makes it easy to guess which file an element came from just by looking at its namespace.

## Your Resource Pack's File Structure

When you're building a UI pack, your `ui/` folder mirrors the vanilla structure but only includes the files you're modifying:

```
my_pack/
├── manifest.json
├── pack_icon.png
└── ui/
    ├── _ui_defs.json       ← lists your files
    ├── hud_screen.json     ← your HUD changes
    └── pause_screen.json   ← your pause menu changes
```

You never need to copy the entire vanilla file. Just create a file with the same name, declare the same namespace, and write only the elements you're changing.

---

Next: [Element Types →](core-concepts/element-types.md)
