# Important UI Files

<span class="badge beginner">Beginner</span>

A reference guide to the vanilla UI files — what each one controls and which elements are most commonly modified.

---

## The `ui/` Folder Layout

```
ui/
├── _ui_defs.json               ← master file registry (always needed)
├── _global_variables.json      ← shared variables
├── hud_screen.json             ← in-game HUD
├── inventory_screen.json       ← player inventory
├── pause_screen.json           ← pause/game menu
├── start_screen.json           ← title/main menu
├── chat_screen.json            ← chat input
├── death_screen.json           ← "You Died"
├── book_screen.json            ← Book & Quill
├── container_screens.json      ← chest, furnace, crafting
├── settings_screen.json        ← game settings
├── server_form.json            ← NPC dialog forms
├── mob_effects.json            ← potion effect display
├── toast.json                  ← achievement/recipe toast popups
├── hud_with_effects.json       ← HUD layer with mob effects shown
└── common/
    ├── common.json             ← shared base elements
    └── ...
```

---

## File-by-File Guide

### `_ui_defs.json`

**Purpose:** Registers which UI files Minecraft should load and merge.

**You must edit this** whenever you add a new file to your pack.

```json
{
    "ui_defs": [
        "ui/hud_screen.json",
        "ui/pause_screen.json"
    ]
}
```

Only list files from your pack — vanilla files are already registered.

---

### `hud_screen.json` — Namespace: `hud`

The most commonly modified file. Controls everything visible during gameplay.

| Element Name | What It Is |
|---|---|
| `hud_content` | Root container of most HUD elements |
| `health_bar` | Heart icons row |
| `hunger_bar` | Food/hunger icons |
| `experience_bar` | XP progress bar |
| `armor_bar` | Armor icon row |
| `paper_doll` | Character model display |
| `hotbar` | The 9-slot item bar |
| `hotbar_and_slot_items` | Hotbar + slot container |
| `crosshair_container` | Crosshair element |
| `boss_bars` | Boss health bar area |
| `chat_text_display` | Chat message renderer |
| `actionbar_text` | Action bar (middle-bottom text) |
| `title_text` | `/title` command text |
| `subtitle_text` | `/title` subtitle text |

---

### `pause_screen.json` — Namespace: `pause`

Controls the in-game pause/game menu.

| Element Name | What It Is |
|---|---|
| `pause_menu_content` | Root content area |
| `resume_button` | "Back to Game" button |
| `achievements_button` | Achievements screen link |
| `settings_button` | Opens settings |
| `feedback_button` | Feedback/bug report button |
| `disconnect_button` | Leave world/server |

---

### `start_screen.json` — Namespace: `start`

Controls the main title/menu screen.

| Element Name | What It Is |
|---|---|
| `start_content` | Root content area |
| `play_button` | "Play" button |
| `settings_button` | Settings button |
| `marketplace_button` | Marketplace button |
| `minecraft_title` | The Minecraft logo/title |

---

### `inventory_screen.json` — Namespace: `inventory`

Controls the player's inventory screen.

| Element Name | What It Is |
|---|---|
| `inventory_panel` | Main inventory area |
| `crafting_grid` | 2×2 crafting area |
| `player_model` | 3D character display |
| `armor_slots` | Helmet/chest/legs/boots slots |

---

### `container_screens.json` — Namespace: `container`

Controls container UIs: chests, furnaces, crafting tables, etc.

---

### `chat_screen.json` — Namespace: `chat`

Controls the chat input interface that appears when you press T.

---

### `death_screen.json` — Namespace: `death`

Controls the "You Died" / respawn screen.

---

### `toast.json` — Namespace: `toast`

Controls achievement unlock and recipe discovery toast popups.

---

### `common/common.json` — Namespace: `common`

Shared base elements extended by many other files. Contains:
- Base button definitions (`common.button`)
- Toggle base
- Scrollbar components
- Common panel backgrounds

You'll often reference elements from here (`@common.button`) but rarely modify the file itself.

---

## The Most Common Pattern

For any modification:

1. Identify which file controls what you want → from the table above
2. Find the specific element name → search the vanilla file
3. Create `ui/that_screen.json` in your pack with the same namespace
4. Write only the elements you're changing
5. Register in `_ui_defs.json`

---

## Version Differences

Element names and file structure can change between Minecraft versions. Always verify against the vanilla files that match your game version. Download vanilla files from [github.com/Mojang/bedrock-samples](https://github.com/Mojang/bedrock-samples) — make sure to select the correct tag/release for your version.

---

Next: [Tips & Troubleshooting →](reference/tips.md)
