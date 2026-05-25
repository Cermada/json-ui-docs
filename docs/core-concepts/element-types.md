# Element Types

<span class="badge beginner">Beginner</span>

Every element in JSON UI has a `type` property that determines what it is and how it behaves. Think of types as the building blocks — just like HTML has `<div>`, `<p>`, `<img>`, and `<button>`, JSON UI has its own set of fundamental types.

## The Core Types

### `panel`

A **panel** is an invisible container. It holds other elements and controls their layout. It has no visual appearance by itself — it's purely structural.

```json
"my_panel": {
    "type": "panel",
    "size": [200, 100],
    "controls": [
        { "child_element@namespace.element": {} }
    ]
}
```

Use a panel whenever you need to group elements together, position them as a unit, or create a layout region.

---

### `label`

A **label** displays text on screen.

```json
"my_label": {
    "type": "label",
    "text": "Hello, World!",
    "color": [1, 1, 1, 1],
    "font_type": "smooth"
}
```

Labels can show static text, or dynamic text fed through bindings (like displaying the player's health as a number). They support color, font type, shadow, and text alignment.

---

### `image`

An **image** renders a texture file (`.png`) on screen.

```json
"my_image": {
    "type": "image",
    "texture": "textures/ui/my_icon",
    "size": [32, 32]
}
```

The `texture` path is relative to your resource pack root (no `.png` extension). Images support UV mapping (`uv`, `uv_size`) for sprite sheets, and can be tinted with `color`.

---

### `button`

A **button** is an interactive element that responds to clicks and taps.

```json
"my_button": {
    "type": "button",
    "size": [120, 40],
    "$pressed_button_name": "my_button_pressed"
}
```

Buttons have different visual states (default, hover, pressed) and trigger actions when clicked. In practice, you'll usually extend a vanilla button base rather than building from scratch.

---

### `toggle`

A **toggle** is a two-state button — on or off. Checkboxes and radio buttons are toggles.

```json
"my_toggle": {
    "type": "toggle",
    "toggle_name": "my_setting_toggle",
    "toggle_default_state": false
}
```

---

### `input_panel`

An **input panel** is a special panel that captures keyboard and gamepad input. Used for text fields and interactive areas that need to receive focus.

---

### `edit_box`

A text **input field** where the player can type.

```json
"my_text_field": {
    "type": "edit_box",
    "size": [200, 24],
    "placeholder_text": "Enter text here..."
}
```

---

### `scroll_view`

A **scroll view** makes its content scrollable. Used for long lists, chat history, and any content that might overflow its container.

```json
"my_scroll_view": {
    "type": "scroll_view",
    "size": [200, 300],
    "scrollbar_track_button": "scrollbar_track",
    "scrollbar_thumb_button": "scrollbar_thumb"
}
```

---

### `grid`

A **grid** repeats a child element in a 2D grid pattern. Used for hotbars, inventory slots, crafting grids, etc.

```json
"my_grid": {
    "type": "grid",
    "grid_dimensions": [9, 1],
    "grid_item_template": "namespace.slot_item",
    "size": [162, 18]
}
```

---

### `stack_panel`

A **stack panel** arranges its children in a row or column automatically.

```json
"my_stack": {
    "type": "stack_panel",
    "orientation": "vertical",
    "controls": [
        { "item_0@ns.row": {} },
        { "item_1@ns.row": {} }
    ]
}
```

Children are stacked one after another with no gaps (unless you add spacers or padding).

---

### `custom`

The **custom** type hooks into a C++ rendering system built into the game engine. These are used for things like the inventory item renderer, the 3D player model display, and the map view. You cannot create new custom renderers; you can only reference existing ones by name.

```json
"player_model": {
    "type": "custom",
    "renderer": "live_player_renderer"
}
```

---

## Choosing the Right Type

| You want to... | Use |
|---|---|
| Group elements together | `panel` |
| Display text | `label` |
| Display a texture | `image` |
| Make something clickable | `button` |
| Two-state on/off control | `toggle` |
| Let the player type text | `edit_box` |
| Make content scrollable | `scroll_view` |
| Repeat items in a grid | `grid` |
| Stack items in a row/column | `stack_panel` |

## Type Is Inherited

When an element extends another element using `@`, it inherits the parent's type (among other things). So if `common.button` has `"type": "button"`, then `"my_button@common.button"` is also a button — you don't need to re-declare the type.

---

Next: [Making Your Element Appear →](core-concepts/making-elements-visible.md)
