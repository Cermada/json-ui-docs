# Your First UI Change

<span class="badge beginner">Beginner</span>

Let's make a real change to Minecraft's interface. We'll modify the **chat message color** — a small, visible, satisfying change that teaches you the core workflow you'll use for every JSON UI edit.

## The Workflow

Every JSON UI modification follows the same four steps:

1. **Find** the vanilla file that controls what you want to change
2. **Understand** what the existing JSON is doing
3. **Copy** the relevant piece into your resource pack
4. **Modify** it

## Step 1: Find the Right File

Chat messages are part of the HUD (Heads-Up Display) — the interface you see while playing, not while in a menu.

In the vanilla UI files, the HUD lives in `ui/hud_screen.json`. Open the vanilla copy you downloaded earlier and search for `chat_text_display` — that's the element that renders chat messages.

You'll find something like this (simplified):

```json
"chat_label": {
    "type": "label",
    "color": $chat_text_color
}
```

This tells us: there's a **label** element called `chat_label`, and its text color denoted to global variables using variable `$chat_text_color` as the placeholder.

## Step 2: Understand the JSON

Let's break down what we're looking at:

```json
"chat_label": {    // The element's name (key)
    "type": "label",      // What kind of element it is
    "color": $chat_text_color // a placeholder denoted to _global_variables.json, you can replace them using hardcoded value of RGB 0-1 e.g, [ 1, 1, 1]
}
```

Colors in JSON UI use RGB 0-1 / RGBA 0-1, whereas its values between `0.0` (none) and `1.0` (full). So:
- `[1, 0, 0]` = red
- `[0, 1, 0]` = green
- `[1, 1, 0]` = yellow
- `[0.5, 0.5, 0.5]` = gray

## Step 3: Create Your Modification File

In your resource pack's `ui/` folder, create a file called `hud_screen.json`.

> **Important:** You are NOT copying the entire vanilla `hud_screen.json`. You only write the parts you want to change. Minecraft will **merge** your file with the vanilla one.

Add this content:

```json
{
    "namespace": "hud",

    "chat_label": {
        "color": [1, 1, 0, 1]
    }
}
```

That's it. You haven't defined a new element — you've **modified** an existing one by overriding just its `color` property.

## Step 4: Register Your UI File

There's one more thing needed. Minecraft needs to know your pack has a `ui/hud_screen.json` file it should include in the merge. This is done via `ui/_ui_defs.json`.

Create `ui/_ui_defs.json` in your pack:

```json
{
    "ui_defs": [
        "ui/hud_screen.json"
    ]
}
```

This file tells Minecraft: "hey, look at these files when building the UI."

> **Why is this necessary?** Minecraft doesn't automatically scan your pack for all JSON files. You have to explicitly register UI files here. Forgetting this is one of the most common beginner mistakes.

## Step 5: Test It

1. Save both files
2. Open Minecraft and load a world with your resource pack active
3. Open chat (`T` key) and type something
4. Your chat messages should now appear in **yellow**

If it works — congratulations! You've made your first JSON UI modification.

## What Just Happened?

When Minecraft loaded, it:
1. Read the vanilla `hud_screen.json` (defines thousands of elements)
2. Read your `hud_screen.json` (defines only `chat_label` with a new color)
3. **Merged** them together — your property overwrote the vanilla one
4. Rendered the UI using the merged result

This merge system is the fundamental mechanism of all JSON UI modding. You never have to replace the entire vanilla file — you just override the specific properties you want to change.

## Troubleshooting

**Chat is still white:**
- Did you save both files?
- Is `_ui_defs.json` in the `ui/` folder (not the pack root)?
- Is the pack activated in Minecraft?
- Try quitting and reloading Minecraft

**Game crashed or UI disappeared:**
- There's probably a JSON syntax error. Open the file in VS Code and look for red underlines.
- Check for missing commas, unclosed braces `{}`, or mismatched quotes `""`

---

Next: [How JSON UI Works →](core-concepts/how-json-ui-works.md)
