# Introduction to JSON UI

<span class="badge beginner">Beginner</span>

## What Is JSON UI?

Every button, every health bar, every chat message, every menu screen you see in Minecraft Bedrock Edition is defined by **JSON UI** — a system that describes the entire game interface as a collection of text files written in JSON.

Instead of the interface being hardcoded into the game's engine, Mojang built a flexible **declarative UI system** where each element of the UI is described in a plain text file. This means you — a player, a creator, a resource pack maker — can **change any part of the interface** just by editing or adding files in a resource pack.

## Why Does Bedrock Use JSON UI?

Bedrock Edition needs to run on many different devices: Windows PCs, phones, tablets, consoles. A hardcoded interface would be a nightmare to maintain across all these screen sizes and input methods.

JSON UI solves this by letting the game's rendering engine read a set of instructions — the JSON files — and build the UI from those instructions at runtime. The engine does the actual drawing; JSON UI tells it *what* to draw, *where*, and *how*.

This architecture has a great side effect for us: **the files are readable and editable**. You don't need to recompile anything or modify the game's code. You just change the files.

## What Can You Do With JSON UI?

Almost anything. Here are some real examples of what the community has made:

- **Custom HUD layouts** — Move the health bar, hotbar, or minimap to different positions
- **Hidden elements** — Remove the paper doll, hide the XP bar, or strip the UI down to nothing
- **Reskinned menus** — Give the pause menu, settings screen, or inventory a completely different look
- **New UI elements** — Add custom text, images, or counters that display game data
- **Full UI overhauls** — Replace every single screen with a custom design

If you can see it in the game's UI, you can change it.

## How Does It Work — The Big Picture

Here is the flow from file to screen:

```
Your Resource Pack
    └── ui/
        ├── hud_screen.json      ← you edit this
        ├── inventory_screen.json
        └── ...

         ↓  Minecraft loads and merges these files

    The game's JSON UI renderer reads the merged result

         ↓

    Pixels appear on screen
```

Your resource pack sits on top of the vanilla UI files. You don't replace them entirely (unless you want to) — you can **patch** specific parts, add new elements, or override individual properties.

## The Key Insight

JSON UI is **declarative**. You don't write *instructions for drawing* — you write *descriptions of what should exist*. You say "there should be a label here, showing the player's health, with white text, positioned 10 pixels from the top-left". The engine figures out how to make that happen.

This is different from traditional programming. There are no `if` statements or `for` loops in JSON UI (well, there are bindings and variables that act similarly, but conceptually you are always *describing*, not *instructing*).

Once you internalize this, reading and writing JSON UI becomes much more intuitive.

## What JSON UI Is NOT

- It is **not a scripting system** — you cannot run arbitrary code in JSON UI
- It is **not HTML/CSS** — though some concepts are similar, html/css are meant to display data, JSON is just one of the way to store them

---

Next: [Setting Up Your Environment →](getting-started/environment-setup.md)
