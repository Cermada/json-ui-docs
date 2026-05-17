# Tips & Troubleshooting

<span class="badge beginner">Beginner</span>

Common problems and how to fix them — plus tips to make your JSON UI work faster and more reliably.

---

## The Golden Rules

**1. Never edit vanilla files directly.**
Always copy what you need into your own pack. Keep vanilla files as read-only references.

**2. Always register your files in `_ui_defs.json`.**
Forgetting this is the #1 reason changes don't appear.

**3. Check for JSON syntax errors first.**
90% of broken UIs are caused by a missing comma, unclosed brace, or extra bracket. Open the file in VS Code and look for red underlines before anything else.

**4. Reload after every change.**
Minecraft doesn't hot-reload UI files. You must quit to the main menu and re-enter the world (or restart the game entirely for some changes).

**5. Match the namespace exactly.**
If the file is `hud_screen.json`, use `"namespace": "hud"`. Wrong namespaces mean your overrides never apply.

---

## Troubleshooting Guide

### "My changes aren't showing up"

Check in this order:
1. Is the file saved? (`Ctrl+S`)
2. Is the filename exactly right? (`hud_screen.json`, not `hud_Screen.json`)
3. Is the file listed in `_ui_defs.json`?
4. Does the namespace match the vanilla file's namespace?
5. Did you fully reload Minecraft (not just pause/resume)?
6. Is your resource pack activated for this world?

### "The UI is completely broken / invisible"

You have a JSON syntax error. Signs:
- Game shows a blank screen where the UI should be
- Entire screen is missing

Fix:
1. Open the file in VS Code
2. Look for red underlines (syntax errors)
3. Check: missing commas between elements, unclosed `{` or `[`, mismatched `"` quotes

### "My element appears but in the wrong position"

- Check `anchor_from` and `anchor_to` — they control which corner is the reference point
- Check `offset` direction — positive Y goes down on screen
- Remember: anchors are relative to the **parent element**, not the screen

### "I'm trying to modify an element but it has no effect"

The element might be in a different namespace than you think. Try:
1. Search the vanilla file for the element name
2. Check the `namespace` at the top of that file
3. Make sure your modification is in the correct namespace

### "The modification operation isn't finding the target"

When using `"operation": "remove"` or `"insert_before"/"insert_after"`, the `target` must exactly match how the child is named in the controls array — including the `@namespace.element` part.

Example: if the vanilla controls array has:
```json
{ "paper_doll@hud.paper_doll": {} }
```

Your target must be `"paper_doll@hud.paper_doll"` (the full key, including the `@` reference).

### "The game crashes when I load the pack"

A crash usually means a serious structural error — not just a missing comma, but something like:
- Circular element references (element A extends element B, B extends A)
- Missing required properties on certain element types
- Referencing a texture file that doesn't exist (in some cases)

Disable your pack, check for issues, re-enable.

### "My binding isn't updating the element"

- Check the `binding_name` spelling — it must exactly match the engine's binding name
- Try adding `"binding_condition": "always"` to force updates every frame
- Some bindings only work on specific screen types — a health binding won't work on the pause screen

---

## Performance Tips

**Avoid `"binding_condition": "always"` unless necessary.**
It forces the binding to evaluate every frame. `"always_when_visible"` or `"once"` is more efficient.

**Don't use excessively deep nesting.**
Very deep element trees (10+ levels) can slow rendering. Keep structure reasonably flat.

**Use `"visible": false` instead of making elements transparent.**
Invisible elements don't render. Transparent (`"color": [1,1,1,0]`) elements still render — just invisible.

---

## Development Tips

**Keep the vanilla files open in a second VS Code window.**
You'll be referencing them constantly. Split-screen or alt-tab makes this much faster.

**Use Git to track your changes.**
Even simple `git init` + `git commit` lets you roll back when something breaks.

**Test one change at a time.**
Don't make five changes then test. Make one, test, make another. This makes bugs easy to isolate.

**Use descriptive names for your elements.**
`"my_custom_health_display"` is much easier to work with than `"panel_1"` six months later.

**Comment your JSON... sort of.**
JSON doesn't support comments. Instead, add a `"_comment"` key to important elements:
```json
"complex_element": {
    "_comment": "This handles the boss bar visibility logic",
    "type": "panel"
}
```
The game ignores unknown keys, so this is safe.

---

## Common JSON Syntax Mistakes

```json
// WRONG: missing comma between elements
{
    "element_a": { "type": "panel" }
    "element_b": { "type": "label" }
}

// RIGHT:
{
    "element_a": { "type": "panel" },
    "element_b": { "type": "label" }
}
```

```json
// WRONG: trailing comma after last item
{
    "controls": [
        { "child@ns.child": {} },   ← trailing comma
    ]
}

// RIGHT:
{
    "controls": [
        { "child@ns.child": {} }
    ]
}
```

```json
// WRONG: single quotes instead of double
{
    'type': 'label'
}

// RIGHT:
{
    "type": "label"
}
```

---

Next: [Further Learning →](reference/further-learning.md)
