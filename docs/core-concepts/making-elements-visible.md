# Making Your Element Appear On Screen

<span class="badge beginner">Beginner</span>

You wrote your custom element. You tested in-game. Nothing appeared. Sound familiar?

This is the **most common stumbling block** for JSON UI beginners, and the fix is straightforward once you understand what's actually happening.

---

## Why Your Element Doesn't Show Up

Writing an element definition **does not put it on screen**. It just registers a blueprint.

Think of it like this: you drew a blueprint for a chair. That blueprint exists — but there's no chair in the room yet. You have to actually place the chair somewhere.

In JSON UI, "placing the chair" means **attaching your element to the screen's element tree**. Until your element is connected to the tree of a live screen, it will never render — no matter how correctly it's written.

Here's what happens with an element that's defined but never attached:

```json
{
    "namespace": "hud",

    "my_cool_label": {
        "type": "label",
        "text": "Hello!",
        "color": [1, 1, 0, 1]
    }
}
```

✅ The element exists in memory  
❌ Nothing appears on screen — because nothing is showing it

---

## The Four-Step Checklist

Every custom element that needs to appear on screen requires **all four** of these:

| Step | What You Need | Common Mistake |
|---|---|---|
| 1 | Define the element | Syntax errors |
| 2 | Register your file in `_ui_defs.json` | Forgetting this entirely |
| 3 | Attach it to the screen's tree | Forgetting this entirely |
| 4 | Make sure the parent element is visible | Parent is hidden or wrong screen |

Most people nail step 1 and miss steps 2 and 3.

---

## Step 1: Define Your Element

Write your element in a UI file. Use the correct namespace for the screen you're targeting.

```json
{
    "namespace": "hud",

    "my_hello_label": {
        "type": "label",
        "text": "Hello from my pack!",
        "color": [1, 1, 0, 1],
        "font_type": "smooth",
        "shadow": true,
        "anchor_from": "top_left",
        "anchor_to": "top_left",
        "offset": [8, 8]
    }
}
```

> **Namespace matters.** If this file is `hud_screen.json`, use `"namespace": "hud"`. If it's a different screen, use the matching namespace. Wrong namespace = element is unreachable.

---

## Step 2: Register Your File in `_ui_defs.json`

This is the step most beginners miss. Minecraft will not load your UI file unless it is listed here.

In your pack's `ui/` folder, create (or edit) `_ui_defs.json`:

```json
{
    "ui_defs": [
        "ui/hud_screen.json"
    ]
}
```

If you're modifying multiple screens, list them all:

```json
{
    "ui_defs": [
        "ui/hud_screen.json",
        "ui/pause_screen.json"
    ]
}
```

> **You only list your own files.** The vanilla files are already registered by the vanilla pack. Never try to unregister vanilla files here.

---

## Step 3: Attach Your Element to the Screen Tree

This is the step that actually puts your element on screen. You need to **inject** your element into an existing screen's control hierarchy using the `modifications` system.

Find the parent element in the vanilla screen that you want to add to, then use `modifications` to insert your element as a child:

```json
{
    "namespace": "hud",

    "my_hello_label": {
        "type": "label",
        "text": "Hello from my pack!",
        "color": [1, 1, 0, 1],
        "font_type": "smooth",
        "shadow": true,
        "anchor_from": "top_left",
        "anchor_to": "top_left",
        "offset": [8, 8]
    },

    "hud_content": {
        "modifications": [
            {
                "array_name": "controls",
                "operation": "insert_front",
                "value": [
                    { "hello@hud.my_hello_label": {} }
                ]
            }
        ]
    }
}
```

The key parts:
- `"hud_content"` — the **name of an existing vanilla element** whose children we're adding to
- `"insert_front"` — adds your element at the beginning of that element's `controls` array
- `"hello@hud.my_hello_label"` — gives this instance the local name `hello` and points it to your element definition

> **How to find the right parent:** Open the vanilla `hud_screen.json` and look for a top-level panel that contains other HUD elements — `hud_content` or `root_panel` are common names. The parent must be a container element (`type: panel` or `stack_panel`).

---

## Complete Working Example

Here is a full, copy-pasteable example that adds a yellow "Hello!" label to the top-left corner of the HUD.

**`ui/_ui_defs.json`:**
```json
{
    "ui_defs": [
        "ui/hud_screen.json"
    ]
}
```

**`ui/hud_screen.json`:**
```json
{
    "namespace": "hud",

    "my_hello_label": {
        "type": "label",
        "text": "Hello!",
        "color": [1, 1, 0, 1],
        "font_type": "smooth",
        "shadow": true,
        "anchor_from": "top_left",
        "anchor_to": "top_left",
        "offset": [8, 8]
    },

    "hud_content": {
        "modifications": [
            {
                "array_name": "controls",
                "operation": "insert_front",
                "value": [
                    { "hello@hud.my_hello_label": {} }
                ]
            }
        ]
    }
}
```

Load the game with this pack active, enter a world, and you'll see "Hello!" in the top-left corner of your HUD.

---

## What Happens Under the Hood

When Minecraft builds the UI, it merges your file with vanilla:

```
vanilla hud_content: { controls: [ ... 15 vanilla children ... ] }

your hud_content: { modifications: [ insert_front: my_hello_label ] }

merged result: { controls: [ my_hello_label, ... 15 vanilla children ... ] }
```

Your element is now part of the rendered tree. The screen includes it when drawing.

---

## Finding the Right Parent Element

The parent you inject into must:
1. Be a container type (`panel`, `stack_panel`, `scroll_view`)
2. Exist in the vanilla file for the screen you're targeting
3. Be the right screen — don't inject a HUD element into the pause screen

### How to discover parent names

Open the vanilla file (e.g. `hud_screen.json`) and look at the top-level elements. The one whose `controls` array contains all the visible HUD elements is your target:

```json
"hud_content": {
    "type": "panel",
    "controls": [
        { "health_bar@hud.health_bar_container": {} },
        { "hunger_bar@hud.hunger_bar_container": {} },
        { "hotbar@hud.hotbar": {} },
        ...
    ]
}
```

That `hud_content` is your injection target for HUD additions.

For the **pause menu**, look for `pause_menu_content` or `menu_main` in `pause_screen.json`.  
For the **inventory**, look for the root panel in `inventory_screen.json`.

---

## "I Did All Three Steps — Still Nothing"

Work through this checklist:

- [ ] Did you **save** all files?
- [ ] Is `_ui_defs.json` in the `ui/` folder (not the pack root)?
- [ ] Is the file path in `_ui_defs.json` exactly `"ui/hud_screen.json"` (with the `ui/` prefix)?
- [ ] Does the namespace in your file match the vanilla file's namespace? (`"hud"` for hud_screen.json)
- [ ] Is the parent element name (`hud_content`) spelled exactly right?
- [ ] Did you fully **reload** the game (quit to title, re-enter the world)?
- [ ] Is your resource pack **activated** for this world?
- [ ] Open the file in VS Code — are there any **red underline syntax errors**?

If you're still stuck, try this diagnostic: temporarily replace your element with the simplest possible thing:

```json
"my_test": {
    "type": "image",
    "texture": "textures/ui/white_square",
    "color": [1, 0, 0, 1],
    "size": [50, 50],
    "anchor_from": "center",
    "anchor_to": "center"
}
```

A bright red 50×50 square, centered on screen, is impossible to miss. If that doesn't appear, the problem is in steps 2 or 3 — the element isn't being loaded or isn't being injected. If it does appear, the problem is in your original element definition.

---

## Quick Recap

> **Define → Register → Attach**
>
> 1. Write your element in a `.json` file with the correct namespace
> 2. Add that file to `ui/_ui_defs.json`
> 3. Use `modifications` to inject it into a parent element in the screen tree

All three are required. Skip any one and nothing shows up.

---

Next: [Bindings — Live Data →](core-concepts/bindings.md)
