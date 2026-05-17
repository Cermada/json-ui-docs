# The Modifications System

<span class="badge intermediate">Intermediate</span>

The `modifications` key is the surgical tool of JSON UI. Instead of replacing an entire element, it lets you **add, remove, or reorder** specific children without touching anything else. Once you understand modifications, you'll use them constantly.

## Why Modifications Exist

Imagine the vanilla `hud_screen.json` defines a panel with 15 children — health bar, hunger bar, XP bar, hotbar, chat, crosshair, boss bar, and more. You want to add a custom element to that panel.

Without modifications, you'd have to copy the entire `controls` array and add your element. That means your pack now hardcodes all 15 children — if Mojang adds a 16th in an update, your pack won't include it and something will break.

With modifications, you just say: "insert my element at the end of that panel's controls." Your pack only contains your addition. Everything else stays exactly as vanilla defines it.

## Basic Syntax

```json
"hud_root_element": {
    "modifications": [
        {
            "array_name": "controls",
            "operation": "insert_back",
            "value": [
                { "my_custom_widget@mynamespace.my_widget": {} }
            ]
        }
    ]
}
```

The `modifications` array contains one or more modification objects. Each modification specifies:
- `array_name` — which property array to target (usually `"controls"`)
- `operation` — what to do
- `value` — what to insert (for insert operations)

## Operations

### `insert_front`

Adds elements to the **beginning** of the array (before all existing children):

```json
{
    "array_name": "controls",
    "operation": "insert_front",
    "value": [
        { "my_element@ns.widget": {} }
    ]
}
```

### `insert_back`

Adds elements to the **end** of the array (after all existing children):

```json
{
    "array_name": "controls",
    "operation": "insert_back",
    "value": [
        { "my_element@ns.widget": {} }
    ]
}
```

### `insert_before`

Adds elements **before a specific named child**. Use this when order matters:

```json
{
    "array_name": "controls",
    "operation": "insert_before",
    "target": "hotbar_and_slot_items@hud.hotbar_and_slot_items",
    "value": [
        { "my_element@ns.widget": {} }
    ]
}
```

The `target` must exactly match the key of an existing child in that controls array.

### `insert_after`

Adds elements **after a specific named child**:

```json
{
    "array_name": "controls",
    "operation": "insert_after",
    "target": "hotbar_and_slot_items@hud.hotbar_and_slot_items",
    "value": [
        { "my_element@ns.widget": {} }
    ]
}
```

### `remove`

Removes a specific named child from the array:

```json
{
    "array_name": "controls",
    "operation": "remove",
    "target": "some_element@namespace.some_element"
}
```

This is how you hide elements without setting them invisible — you surgically remove them from the tree.

## Real-World Example: Adding a Custom Label to the HUD

Let's add a label that reads "Hello!" in the top-left corner of the HUD.

**Step 1:** Define your custom label element somewhere in your file:

```json
"my_hello_label": {
    "type": "label",
    "text": "Hello!",
    "color": [1, 1, 0, 1],
    "anchor_from": "top_left",
    "anchor_to": "top_left",
    "offset": [8, 8]
}
```

**Step 2:** Use modifications to inject it into the HUD's root panel:

```json
"hud_content": {
    "modifications": [
        {
            "array_name": "controls",
            "operation": "insert_front",
            "value": [
                { "hello@myhud.my_hello_label": {} }
            ]
        }
    ]
}
```

You need to figure out the name of the HUD's root panel — open the vanilla `hud_screen.json` and find the top-level element whose controls you want to add to. Common ones are `hud_content` and `root_panel`.

## Multiple Modifications at Once

A single element can have multiple modifications:

```json
"hud_content": {
    "modifications": [
        {
            "array_name": "controls",
            "operation": "remove",
            "target": "paper_doll_and_hotbar@hud.paper_doll_and_hotbar"
        },
        {
            "array_name": "controls",
            "operation": "insert_back",
            "value": [
                { "my_custom_hotbar@myhud.custom_hotbar": {} }
            ]
        }
    ]
}
```

This removes the vanilla hotbar element and inserts your custom one in its place — all in one shot.

## When to Use Modifications vs. Direct Override

| Use **modifications** when... | Use **direct override** when... |
|---|---|
| You're adding/removing children | You're changing element properties (color, size, position) |
| You want to preserve vanilla children | You're replacing the whole element |
| You want update-safety | You know the element's full desired state |

> **Best practice:** Prefer modifications for structural changes. They're safer across game updates and more readable.

---

Next: [Bindings — Live Data →](core-concepts/bindings.md)
