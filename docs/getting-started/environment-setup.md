# Setting Up Your Environment

<span class="badge beginner">Beginner</span>

Before you write a single line of JSON, you need the right tools. The good news: everything you need is free.

## What You Need

| Tool | Purpose | Download |
|---|---|---|
| **VS Code** | Text editor with JSON support | [code.visualstudio.com](https://code.visualstudio.com) |
| **Minecraft Bedrock Edition** | Testing your pack | Microsoft Store / App Store |
| **Bridge v2** *(optional)* | Dedicated Bedrock addon IDE | [bridge-core.app](https://bridge-core.app) |

> **Why VS Code?** It understands JSON syntax, shows you errors as you type, and has extensions that make editing much easier. Notepad works, but you'll thank yourself later for using a proper editor.

## Installing VS Code

1. Download VS Code from [code.visualstudio.com](https://code.visualstudio.com)
2. Install it with the default settings
3. Open VS Code

### Recommended Extensions

Install these from VS Code's Extensions panel (Ctrl+Shift+X):

- **Blockception's Minecraft Bedrock Development** — Autocomplete and validation for Bedrock files, including JSON UI
- **Prettier - Code formatter** — Keeps your JSON tidy
- **Error Lens** — Shows errors inline as you type

To install an extension: press `Ctrl+Shift+X`, search for the extension name, click Install.

## Finding Your Resource Packs Folder

This is where Minecraft looks for your packs. The location depends on your platform:

**Windows (from Microsoft Store):**
```
%LOCALAPPDATA%\Packages\Microsoft.MinecraftUWP_8wekyb3d8bbwe\LocalState\games\com.mojang\resource_packs
```
Press `Win+R`, paste the path above, and hit Enter.

**Android:**
```
/Android/data/com.mojang.minecraftpe/files/games/com.mojang/resource_packs
```

**iOS/iPadOS:**
```
Use the Files app → On My iPhone → Minecraft → games → com.mojang → resource_packs
```
> **Tip for Windows users:** Create a shortcut to your `resource_packs` folder on your desktop. You'll open it constantly.

## Getting Resource Pack samples (Optional but Highly Recommended)

Minecraft's built-in UI files are your best reference. You can download them from Mojang's official samples repository:

👉 [github.com/Mojang/bedrock-samples](https://github.com/Mojang/bedrock-samples)

Click **Code → Download ZIP**, extract it, and look in the `resource_pack/ui/` folder. These are the files you'll be modifying. Keep them open as a reference.

> **Important:** Never edit the vanilla files directly. Always copy what you need into your own resource pack. The original files should stay untouched so you can always reference them.

## Setting Up VS Code for JSON UI Work

Open VS Code, then go to **File → Preferences → Settings** (or press `Ctrl+,`).

Search for `files.associations` and add this:

```json
"*.json": "json"
```

This ensures all `.json` files get proper syntax highlighting.

Also enable **Format on Save**:

```json
"editor.formatOnSave": true
```

---

Next: [Creating a Resource Pack →](getting-started/first-resource-pack.md)
