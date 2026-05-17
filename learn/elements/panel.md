# Panel

<span class="badge beginner">Beginner</span>

The **panel** is the most fundamental element in JSON UI. It is an invisible container — it has no visual appearance on its own, but it groups, positions, and clips its children.

## Basic Usage

```json
"my_panel": {
    "type": "panel",
    "size": [200, 100],
    "controls": [
        { "child_one@ns.element_a": {} },
        { "child_two@ns.element_b": {} }
    ]
}
```

A panel with no `controls` does nothing visible. A panel's job is to give structure to the elements inside it.

## Sizing

### Fixed size
```json
"size": [200, 100]
```
Width 200px, height 100px.

### Percentage of parent
```json
"size": ["100%", "50%"]
```
Full parent width, half parent height.

### Fill parent (100% minus offset)
```json
"size": ["100% + 0px", "100% + 0px"]
```
The `+ 0px` syntax lets you do math: `"size": ["100% - 20px", 40]` means "parent width minus 20 pixels".

### Size to content
```json
"size": ["default", "default"]
```
The element sizes itself to wrap its children. Only works when children have fixed sizes.

## Positioning (Anchors)

Positioning in JSON UI uses an anchor system. Every element has:
- `anchor_from` — the point on the **element** to anchor from
- `anchor_to` — the point on the **parent** to anchor to
- `offset` — a pixel offset from that anchor point

```json
"my_panel": {
    "type": "panel",
    "anchor_from": "top_left",
    "anchor_to": "top_left",
    "offset": [10, 10]
}
```

This positions the panel's top-left corner 10px from the parent's top-left corner.

### Anchor Values

```
"top_left"    | "top_middle"    | "top_right"
"left_middle" | "center"        | "right_middle"
"bottom_left" | "bottom_middle" | "bottom_right"
```

### Common Positioning Patterns

**Center on screen:**
```json
"anchor_from": "center",
"anchor_to": "center",
"offset": [0, 0]
```

**Bottom-right corner:**
```json
"anchor_from": "bottom_right",
"anchor_to": "bottom_right",
"offset": [-10, -10]
```

**Top-center, 20px down:**
```json
"anchor_from": "top_middle",
"anchor_to": "top_middle",
"offset": [0, 20]
```

## Clipping

A panel clips (cuts off) its children by default when they extend beyond its bounds. You can disable this:

```json
"clips_children": false
```

This is useful when children need to overflow — like a tooltip that extends beyond its parent container.

## Layer (Z-index)

The `layer` property controls stacking order. Higher values appear on top:

```json
"my_panel": {
    "type": "panel",
    "layer": 10
}
```

## Visibility

```json
"visible": false
```

Setting `visible` to `false` hides the panel and all its children. The element still takes up space in the layout unless you also set `enabled: false`. Typically you'll drive visibility through a binding.

## Full Example

```json
"info_overlay_panel": {
    "type": "panel",
    "size": [160, 80],
    "anchor_from": "top_right",
    "anchor_to": "top_right",
    "offset": [-8, 8],
    "layer": 5,
    "controls": [
        {
            "background@ns.dark_bg": {}
        },
        {
            "content@ns.info_content": {}
        }
    ]
}
```

A 160×80 panel, anchored to the top-right of its parent with an 8px margin, layered above most other elements, containing a background and some content.

## When to Use a Panel vs. Stack Panel

- Use **`panel`** when you need precise control over child positions, or when children overlap
- Use **`stack_panel`** when children should flow in a row or column automatically

---

Next: [Label →](elements/label.md)
