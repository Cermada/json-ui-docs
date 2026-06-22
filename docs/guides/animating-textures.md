# Animating UI Textures

<span class="badge intermediate">Intermediate</span>

JSON UI supports flipbook-style texture animation — the same technique Minecraft uses internally for animated block textures, applied directly to UI image elements. With it, you can create animated icons, looping effects, and frame-by-frame sprite animations inside any screen.

## How It Works

A UI texture animation uses two elements working together:

1. **An animation control element** — defines the animation parameters (frame count, speed, starting position)
2. **An image element** — displays the texture and points its `uv` property at the animation control

The image element delegates all UV movement to the animation control. Every frame, the animation control shifts the visible region of the texture to the right, creating the illusion of motion.

---

## The Sprite Sheet

Before writing any JSON, you need a texture where all animation frames are arranged **side by side horizontally** in a single PNG file.

```
┌────────────────────────────────────────┐
│ Frame 0 │ Frame 1 │ Frame 2 │ Frame 3  │
│  8×8px  │  8×8px  │  8×8px  │  8×8px  │
└────────────────────────────────────────┘
         Total width: 32px, Height: 8px
```

Three measurements you'll need before writing JSON:

| Measurement | What It Is | Used In |
|---|---|---|
| Frame width & height | The pixel size of one frame | `uv_size`, `frame_step` |
| Frame count | How many frames total | `frame_count` |
| First frame position | Top-left corner of frame 0 (usually `[0, 0]`) | `initial_uv` |

---

## The Animation Control Element

Create an element with `"type": "animation"` and `"anim_type": "flipbook"`. This element is invisible — it only holds animation data.

```json
"my_animation_control": {
    "type": "animation",
    "anim_type": "flipbook",
    "initial_uv": [0, 0],
    "frame_count": 10,
    "frame_step": 8,
    "fps": 10,
    "easing": "linear"
}
```

### Animation Control Properties

| Property | Type | Description |
|---|---|---|
| `type` | string | Must be `"animation"` |
| `anim_type` | string | Must be `"flipbook"` |
| `initial_uv` | [x, y] | Pixel position of the top-left corner of the first frame in the sprite sheet |
| `frame_count` | int | Total number of frames in the animation |
| `frame_step` | int | How many pixels to move right after each frame — usually equal to the frame width |
| `fps` | int | How many frames to display per second |
| `easing` | string | Animation easing — use `"linear"` for a consistent playback speed |
| `reversible` | bool | *(Optional)* If `true`, plays the animation in reverse after it finishes, then loops |

> **`frame_step` explained:** After displaying a frame, the game moves the visible region (`frame_step`) pixels to the right to show the next frame. Set this to the width of one frame. For 8×8 frames, use `"frame_step": 8`.

---

## The Animated Image Element

In your image element, set `uv` to reference your animation control element using the `@namespace.element_name` syntax, and set `uv_size` to the size of one frame:

```json
"my_animated_image": {
    "type": "image",
    "texture": "textures/ui/my_sprite_sheet",
    "uv": "@mynamespace.my_animation_control",
    "uv_size": [8, 8]
}
```

### Image Properties for Animation

| Property | Type | Description |
|---|---|---|
| `texture` | string | Path to your sprite sheet PNG (no extension) |
| `uv` | string | Reference to the animation control: `@namespace.element_name` |
| `uv_size` | [w, h] | Width and height of one frame in pixels |
| `disable_anim_fast_forward` | bool | *(Optional)* If `true`, prevents the game from speeding up the animation. If `false` or omitted, the game may fast-forward the animation to catch up |

---

## Complete Working Example

This example animates a 10-frame sprite sheet where each frame is 8×8 pixels, playing at 10 FPS.

**Sprite sheet layout:**
```
textures/ui/my_sprite_sheet.png
Width: 80px (10 frames × 8px), Height: 8px
```

**`ui/hud_screen.json`:**
```json
{
    "namespace": "myhud",

    "my_animation_control": {
        "type": "animation",
        "anim_type": "flipbook",
        "initial_uv": [0, 0],
        "frame_count": 10,
        "frame_step": 8,
        "fps": 10,
        "easing": "linear"
    },

    "my_animated_image": {
        "type": "image",
        "texture": "textures/ui/my_sprite_sheet",
        "uv": "@myhud.my_animation_control",
        "uv_size": [8, 8],
        "size": [8, 8],
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
                    { "animated_icon@myhud.my_animated_image": {} }
                ]
            }
        ]
    }
}
```

> **Don't forget:** Register `"ui/hud_screen.json"` in your `ui/_ui_defs.json`. See [Making Your Element Appear](core-concepts/making-elements-visible.md) if your animation isn't showing up.

---

## Adjusting Animation Speed

`fps` controls playback speed. Some reference values:

| `fps` | Effect |
|---|---|
| `1` | Very slow — one new frame per second |
| `5` | Slow, like a blinking icon |
| `10` | Medium — smooth for small sprites |
| `20` | Fast — good for action effects |
| `30` | Very fast — near-seamless loop |

---

## Making It Loop in Reverse

Set `"reversible": true` to make the animation play forward, then backward, then repeat:

```json
"my_animation_control": {
    "type": "animation",
    "anim_type": "flipbook",
    "initial_uv": [0, 0],
    "frame_count": 5,
    "frame_step": 16,
    "fps": 8,
    "easing": "linear",
    "reversible": true
}
```

This is useful for "pulse" or "breathe" effects where you want a smooth back-and-forth loop without a hard cut.

---

## Sprite Sheet Checklist

Before testing your animation, confirm:

- [ ] All frames are the **same size**
- [ ] Frames are arranged **horizontally** (left to right)
- [ ] `uv_size` matches the width and height of **one frame**
- [ ] `frame_step` equals the **width of one frame**
- [ ] `frame_count` matches the **actual number of frames** in your sheet
- [ ] The total width of your PNG is at least `frame_count × frame_step` pixels
- [ ] `initial_uv` is `[0, 0]` unless your animation starts partway through a shared sprite sheet

---

## Troubleshooting

**Animation isn't playing / image is frozen:**
- Check that `uv` uses the `@namespace.element_name` format and the namespace + name are correct
- Make sure `anim_type` is exactly `"flipbook"` (not `"flip_book"` or `"Flipbook"`)

**Only one frame is showing:**
- Verify `frame_step` equals the width of one frame
- Check that `frame_count` is greater than 1

**Animation is showing the wrong part of the texture:**
- Adjust `initial_uv` to the correct top-left pixel coordinate of your first frame
- Confirm `uv_size` matches the frame size, not the full sprite sheet size

**Image appears as a white box:**
- The texture path is wrong — double-check the path relative to your pack root, with no `.png` extension

---

Next: [Common Properties Reference →](reference/properties.md)
