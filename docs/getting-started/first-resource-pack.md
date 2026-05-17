# Creating a Resource Pack

<span class="badge beginner">Beginner</span>

A resource pack is just a folder with a specific structure that Minecraft recognizes. Let's build one from scratch.

## The Minimum Required Structure

Every resource pack needs exactly two things to be recognized by Minecraft:

```
my_pack/
├── manifest.json       ← tells Minecraft what this pack is
└── pack_icon.png       ← the icon shown in the pack list (can be any PNG)
```

That's it. Everything else — textures, sounds, UI files — is optional and added on top.

## Step 1: Create the Pack Folder

Navigate to your `resource_packs` folder (see [Environment Setup](getting-started/environment-setup.md) for the path).

Create a new folder. Name it something meaningful like `my_ui_pack`. Avoid spaces — use underscores instead.

## Step 2: Create the manifest.json

Inside your pack folder, create a file called `manifest.json`. This file tells Minecraft:
- The pack's name and description
- Its unique ID (so it doesn't conflict with other packs)
- Which version of Bedrock it targets

```json
{
    "format_version": 2,
    "header": {
        "name": "My UI Pack",
        "description": "My first JSON UI resource pack",
        "uuid": "REPLACE_WITH_UUID_1",
        "version": [1, 0, 0],
        "min_engine_version": [1, 20, 0]
    },
    "modules": [
        {
            "type": "resources",
            "uuid": "REPLACE_WITH_UUID_2",
            "version": [1, 0, 0]
        }
    ]
}
```

### Generating UUIDs

You need two **unique** UUIDs (Universally Unique Identifiers) — one for the header, one for the module. These must be different from each other and from every other pack.

Generate them here: [uuidgenerator.net](https://www.uuidgenerator.net/)

Click "Generate UUID" twice and paste each one into the correct spot. A UUID looks like this:
```
a1b2c3d4-e5f6-7890-abcd-ef1234567890
```

> **Why two UUIDs?** The header UUID identifies the pack itself. The module UUID identifies this specific module (type of content). A pack can have multiple modules, each with its own UUID.

## Step 3: Add a Pack Icon

Add any PNG image named `pack_icon.png` to your pack folder. This is the image that appears in Minecraft's resource pack selection screen. It should ideally be 64×64 or 128×128 pixels.

Don't have one? Create a plain colored square in Paint or any image editor. It just needs to exist.

## Step 4: Add the UI Folder

For JSON UI work, you'll also need a `ui/` folder inside your pack:

```
my_pack/
├── manifest.json
├── pack_icon.png
└── ui/
    └── (your JSON UI files go here)
```

Create the `ui/` folder now. It can be empty for now.

## Step 5: Activate the Pack in Minecraft

1. Open Minecraft Bedrock Edition
2. From the main menu, go to **Settings → Storage → Resource Packs** (or on some versions: **Global Resources**)
3. Your pack should appear in the list — click it and tap **Activate**
4. The pack is now active for all new worlds

For testing in a specific world:
1. Create or edit a world
2. Go to **Resource Packs** in the world settings
3. Activate your pack

## Verifying It Works

If Minecraft shows your pack name and icon in the list, everything is set up correctly. If the pack doesn't appear:

- Double-check `manifest.json` for typos (missing commas, mismatched braces)
- Make sure the file is named exactly `manifest.json`, not `manifest.json.txt`
- Confirm the UUIDs are valid format (8-4-4-4-12 character groups)

> **Pro tip:** Use VS Code's built-in JSON validator. If any line shows a red underline, there's a syntax error. Fix it before testing in-game.

## Your Pack Structure Going Forward

As you add more UI modifications, your pack will grow. Here's what a typical UI pack looks like:

```
my_pack/
├── manifest.json
├── pack_icon.png
└── ui/
    ├── _ui_defs.json         ← registers your UI files (explained later)
    ├── hud_screen.json       ← HUD modifications
    ├── pause_screen.json     ← Pause menu modifications
    └── (other screen files)
```

We'll fill all of this in as we go.

---

Next: [Your First UI Change →](getting-started/first-modification.md)
