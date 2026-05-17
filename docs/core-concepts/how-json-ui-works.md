# How JSON UI Works

<span class="badge beginner">Beginner</span>

Before you can confidently modify the UI, you need to understand the system from the inside. This page explains the mental model — how Minecraft builds its interface from JSON files at runtime.

## The Tree of Elements

JSON UI is organized as a **tree**. Every visible thing on screen is an element. Elements can contain other elements. The result is a hierarchy, just like folders contain files, or like an outline has sections and sub-sections.

```
screen_root
├── header_bar
│   ├── title_label
│   └── close_button
├── content_panel
│   ├── item_list
│   │   ├── item_0
│   │   ├── item_1
│   │   └── item_2
│   └── scrollbar
└── footer
    └── confirm_button
```

Each of those nodes is an **element** defined in a JSON file. The parent element decides where its children are placed on screen.

## Namespaces

Every JSON UI file has a **namespace** — a short prefix that identifies which file an element belongs to. This prevents naming conflicts between different files.

```json
{
    "namespace": "hud",

    "root_panel": {
        "type": "panel"
    }
}
```

The `namespace` is declared once at the top of each file. After that, every element defined in the file is automatically in that namespace. So `root_panel` above is actually `hud.root_panel` to the rest of the system.

When you reference an element from another namespace, you use the full `namespace.element_name` syntax:

```json
"controls": [
    { "my_button@hud.root_panel": {} }
]
```

This is the `@` syntax — we'll cover it in depth soon.

## The Loading & Merging Process

When Minecraft starts up (or loads a world), it:

1. Reads the vanilla UI files
2. Reads every resource pack's UI files (from lowest priority to highest)
3. **Merges** them all together into one combined UI definition
4. Renders from that merged result

Your resource pack is always higher priority than vanilla. So if you define an element with the same name as a vanilla element in the same namespace, yours wins.

```
Vanilla: hud.chat_text_display → { color: [1,1,1,1] }
Your pack: hud.chat_text_display → { color: [1,1,0,1] }

Result: hud.chat_text_display → { color: [1,1,0,1] }  ✓ yours wins
```

Properties are merged at the **property level**. If the vanilla element has 20 properties and you only override 1, the other 19 stay as vanilla defaults.

## The `@` Extension Syntax

This is one of the most important concepts in JSON UI. Elements can **extend** other elements:

```json
"my_button@common.button": {
    "size": [100, 30]
}
```

The `@` means: "start with everything from `common.button`, then apply these extra properties on top."

This is how inheritance works in JSON UI. The vanilla files use it extensively — most complex elements are built by extending simpler base elements and adding specifics.

You can extend:
- Vanilla elements (to build on their behavior)
- Your own elements (to reuse your own definitions)
- Common base types like `common.button`, `common.toggle`, etc.

## Controls: The Children Array

When an element contains other elements, those children are listed in its `controls` array:

```json
"my_panel": {
    "type": "panel",
    "size": [200, 100],
    "controls": [
        {
            "title@hud.section_title": {}
        },
        {
            "body@hud.section_body": {}
        }
    ]
}
```

Each entry in `controls` is an object with a single key. That key follows the format `local_name@namespace.element_name`. The `local_name` is just a name for this instance — it needs to be unique within the parent's controls list.

## How Properties Override

When your pack's file is merged with vanilla, properties are combined at the element level. Merging follows these rules:

| Situation | Result |
|---|---|
| You set a property vanilla doesn't have | Property is added |
| You set a property vanilla already has | Your value wins |
| You omit a property vanilla has | Vanilla value is kept |
| You set `"modifications"` | Special patch system (see next page) |

The `modifications` key is a powerful alternative to full element overrides — it lets you surgically add, remove, or reorder child elements without touching anything else.

## Summary: The Four Key Ideas

1. **Tree** — the UI is a hierarchy of elements
2. **Namespaces** — every file has a prefix; use `namespace.name` to reference across files
3. **Merging** — your pack's JSON is merged with vanilla; you only need to write what you're changing
4. **Extension (`@`)** — elements can inherit from other elements and add/override properties

---

Next: [File Structure →](core-concepts/file-structure.md)
