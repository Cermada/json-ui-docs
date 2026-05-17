# Variables & Templates

<span class="badge intermediate">Intermediate</span>

Variables let you define a value once and reuse it in many places. Templates let you create reusable element blueprints with customizable slots. Together, they make JSON UI far less repetitive and much easier to maintain.

## Variables

A variable is a named value that starts with `$`. Variables can hold colors, sizes, strings, booleans, or any other JSON value.

### Defining a Variable

Variables are defined inside an element definition and are available to that element and its children:

```json
"my_panel": {
    "type": "panel",
    "$text_color": [1, 1, 0, 1],
    "$panel_size": [200, 100],

    "size": "$panel_size",
    "controls": [
        { "my_label@ns.title_label": {} }
    ]
}
```

You can then reference `$text_color` and `$panel_size` anywhere inside this element or its children.

### Using a Variable

Reference a variable by its name anywhere you'd put a value:

```json
"my_label": {
    "type": "label",
    "color": "$text_color",
    "text": "$label_text"
}
```

### Default Values

Variables can have default values using the `|default` suffix:

```json
"$text_color|default": [1, 1, 1, 1]
```

This means: "use this value if `$text_color` hasn't been set by the caller." This is how templates expose customizable slots with sensible fallbacks.

### Global Variables

In `ui/_global_variables.json`, you can define variables available everywhere:

```json
{
    "$hud_opacity|default": 1.0,
    "$primary_color|default": [0.2, 0.8, 0.2, 1]
}
```

Any UI file can then reference `$hud_opacity` without defining it locally.

## Templates (Element Inheritance + Variables)

Templates are what makes JSON UI's reuse system truly powerful. A template is just an element that defines variables with defaults — it's a blueprint for creating similar elements with different configurations.

### Creating a Template

```json
"my_button_template": {
    "type": "panel",
    "size": [120, 30],
    "$button_text|default": "Click Me",
    "$button_color|default": [0.2, 0.6, 0.2, 1],

    "controls": [
        {
            "bg@ns.button_background": {
                "color": "$button_color"
            }
        },
        {
            "label@ns.button_label": {
                "text": "$button_text"
            }
        }
    ]
}
```

### Using a Template

When you extend the template with `@`, you can override any variable:

```json
"confirm_button@ns.my_button_template": {
    "$button_text": "Confirm",
    "$button_color": [0.2, 0.6, 0.2, 1]
},

"cancel_button@ns.my_button_template": {
    "$button_text": "Cancel",
    "$button_color": [0.6, 0.2, 0.2, 1]
},

"close_button@ns.my_button_template": {
    "$button_text": "X",
    "$button_color": [0.4, 0.4, 0.4, 1]
}
```

Three buttons with different text and colors, all sharing the same layout and structure. If you want to change how all buttons look, you change the template once.

## Passing Variables Through the Control Hierarchy

Variables defined in a parent element flow down to children. This is how you customize a deeply nested element without touching its internal structure:

```json
"outer_panel": {
    "type": "panel",
    "$accent_color": [1, 0.5, 0, 1],

    "controls": [
        {
            "inner_widget@ns.colored_widget": {}
        }
    ]
}
```

Inside `colored_widget`, the variable `$accent_color` will resolve to `[1, 0.5, 0, 1]` because it was set in the parent.

## Variable Scope Rules

1. A variable is available in the element it's defined in and all its descendants
2. A child can shadow a parent's variable by redefining it
3. Variables set when calling `@element_name` take precedence over the template's defaults
4. `_global_variables.json` variables are available everywhere but have lowest precedence

## Practical Example: A Reusable Info Card

```json
"info_card_template": {
    "type": "panel",
    "$card_title|default": "Title",
    "$card_body|default": "Body text goes here.",
    "$card_width|default": 200,

    "size": ["$card_width", "default"],
    "controls": [
        {
            "title@ns.card_title_label": {
                "text": "$card_title"
            }
        },
        {
            "body@ns.card_body_label": {
                "text": "$card_body"
            }
        }
    ]
},

"health_card@ns.info_card_template": {
    "$card_title": "Health",
    "$card_body": "Your current health is shown here.",
    "$card_width": 180
},

"hunger_card@ns.info_card_template": {
    "$card_title": "Hunger",
    "$card_body": "Keep your hunger above 6 to regenerate health.",
    "$card_width": 180
}
```

Two cards with the same structure, different content. The template does the heavy lifting.

## Summary

| Concept | Syntax | Purpose |
|---|---|---|
| Variable definition | `"$name": value` | Store a value for reuse |
| Variable with default | `"$name\|default": value` | Store a value with fallback |
| Variable reference | `"property": "$name"` | Use a stored value |
| Template | Element with `$var\|default` properties | Reusable blueprint with slots |
| Template use | `"child@ns.template": { "$var": value }` | Instantiate with custom values |

---

Next: [Panel Element →](elements/panel.md)
