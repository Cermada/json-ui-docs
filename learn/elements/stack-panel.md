# Stack Panel & Grid

<span class="badge beginner">Beginner</span>

**Stack Panel** and **Grid** are layout elements that automatically arrange their children. Instead of manually positioning every child with anchors and offsets, you let the layout engine do the work.

---

## Stack Panel

A stack panel lines up its children one after another — either vertically or horizontally.

### Basic Usage

```json
"my_stack": {
    "type": "stack_panel",
    "orientation": "vertical",
    "controls": [
        { "row_1@ns.menu_row": {} },
        { "row_2@ns.menu_row": {} },
        { "row_3@ns.menu_row": {} }
    ]
}
```

Children are placed in order, touching each other, from top to bottom (vertical) or left to right (horizontal).

### Orientation

```json
"orientation": "vertical"    // stack top to bottom (default)
"orientation": "horizontal"  // stack left to right
```

### Spacing Between Children

There's no built-in gap property in JSON UI. Instead, add spacer elements between children:

```json
"controls": [
    { "item_a@ns.row": {} },
    { "spacer": { "type": "panel", "size": [0, 8] } },
    { "item_b@ns.row": {} }
]
```

The spacer is an invisible 8px-tall panel that creates visual breathing room.

### Sizing the Stack Panel

The stack panel should usually be sized to fit its content:

```json
"size": ["default", "default"]
```

Or give it a fixed width but auto height:
```json
"size": [200, "default"]
```

### Centering a Stack Panel

To center a stack of items on screen:

```json
"centered_stack": {
    "type": "stack_panel",
    "orientation": "vertical",
    "anchor_from": "center",
    "anchor_to": "center",
    "controls": [
        { "item_1@ns.item": {} },
        { "item_2@ns.item": {} }
    ]
}
```

### Practical Example: A Vertical Menu

```json
"main_menu_buttons": {
    "type": "stack_panel",
    "orientation": "vertical",
    "anchor_from": "center",
    "anchor_to": "center",
    "controls": [
        {
            "play_btn@common_buttons.light_button_3": {
                "size": [200, 36],
                "$button_text": "Play"
            }
        },
        { "gap_1": { "type": "panel", "size": [0, 8] } },
        {
            "settings_btn@common_buttons.light_button_3": {
                "size": [200, 36],
                "$button_text": "Settings"
            }
        },
        { "gap_2": { "type": "panel", "size": [0, 8] } },
        {
            "quit_btn@common_buttons.light_button_3": {
                "size": [200, 36],
                "$button_text": "Quit"
            }
        }
    ]
}
```

Three buttons stacked vertically with 8px gaps between them, centered on screen.

---

## Grid

A grid repeats a single element template in a two-dimensional pattern. It's how the hotbar, inventory slots, and crafting grids work.

### Basic Usage

```json
"my_grid": {
    "type": "grid",
    "grid_dimensions": [9, 1],
    "grid_item_template": "ns.slot_template",
    "size": [162, 18]
}
```

- `grid_dimensions` — `[columns, rows]`
- `grid_item_template` — the element to repeat (referenced as `namespace.element_name`)
- `size` — total dimensions of the grid

### The Grid Item Template

The template element is cloned for each cell. It receives special bindings:
- `#collection_name` — name of the data collection driving the grid
- `#collection_index` — this cell's index in the collection

```json
"slot_template": {
    "type": "panel",
    "size": [18, 18],
    "controls": [
        { "slot_image@ns.slot_bg": {} },
        { "item_renderer@ns.item_in_slot": {} }
    ]
}
```

### Grid with Collection Binding

Grids are usually driven by a data collection — the engine provides one value per cell:

```json
"hotbar_grid": {
    "type": "grid",
    "grid_dimensions": [9, 1],
    "grid_item_template": "ns.hotbar_slot",
    "collection_name": "hotbar_items",
    "size": [162, 18]
}
```

The `collection_name` tells the engine which set of data to use. The engine automatically creates one instance of the template per data item.

### When to Use Grid vs. Stack Panel

| Use `grid` when... | Use `stack_panel` when... |
|---|---|
| Repeating the same element many times | Listing different elements in sequence |
| Driven by a data collection (inventory slots, hotbar) | Building a menu or list with varied items |
| You need rows AND columns | You only need a single row or column |
| The number of items is dynamic | The number of items is fixed |

---

## Combining Stack Panel and Grid

A common pattern: use a stack panel for the overall layout, and a grid for a repeated section:

```json
"inventory_layout": {
    "type": "stack_panel",
    "orientation": "vertical",
    "controls": [
        {
            "title_label@ns.title": {
                "text": "Inventory"
            }
        },
        { "gap": { "type": "panel", "size": [0, 8] } },
        {
            "item_grid": {
                "type": "grid",
                "grid_dimensions": [9, 3],
                "grid_item_template": "ns.inv_slot",
                "collection_name": "inventory_items",
                "size": [162, 54]
            }
        }
    ]
}
```

---

Next: [Modifying the HUD →](guides/modify-hud.md)
