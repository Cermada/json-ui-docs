# Further Learning

<span class="badge beginner">Beginner</span>

You've covered all the fundamentals. Here's where to go next to deepen your knowledge and connect with the community.

---

## Official & Community Resources

### Bedrock Wiki — JSON UI Section
The most comprehensive community-maintained reference for Bedrock modding.

🔗 [wiki.bedrock.dev/json-ui/json-ui-intro.html](https://wiki.bedrock.dev/json-ui/json-ui-intro.html)

- Full property reference
- Advanced binding documentation
- Factory system (dynamic/repeated elements)
- Operator expressions (math inside JSON UI)
- Animation system

### JSON UI Examples Repository
Real, working examples of JSON UI modifications by the community.

🔗 [github.com/LeGend077/json-ui-examples](https://github.com/LeGend077/json-ui-examples)

Covers:
- Hotbar customizations
- Custom health displays
- Screen overlays
- HUD rearrangements
- Advanced binding uses

### Mojang Bedrock Samples (Vanilla Files)
The official vanilla resource pack and behavior pack samples.

🔗 [github.com/Mojang/bedrock-samples](https://github.com/Mojang/bedrock-samples)

This is your #1 reference when trying to understand what vanilla is doing. Always download the version matching your game version.

---

## Topics Beyond This Guide

Once you're comfortable with the basics covered here, these are the natural next steps:

### Factory System
Factories allow you to create elements dynamically — one element definition that spawns multiple instances based on data. Used in things like the chat history, effect list, and server list.

Start here: [wiki.bedrock.dev/json-ui/json-ui-intro.html#factory](https://wiki.bedrock.dev/json-ui/json-ui-intro.html)

### Operator Expressions
JSON UI supports limited expressions inside property values — things like `"(#value - 2) * 10"` to do simple math with binding values before applying them.

### Animation System
Elements can be animated with keyframe-style animations defined entirely in JSON — fade in, slide, bounce, scale. The vanilla screens use these for popup transitions.

### Custom Fonts
You can add custom bitmap fonts to your resource pack and reference them as a `font_type` in label elements.

### Scripting + JSON UI
The GameTest scripting API can interact with some UI systems, and server-side form APIs can display custom menus. These go beyond JSON UI into Bedrock's scripting layer.

---

## YouTube Tutorials

Search YouTube for:
- **"Minecraft Bedrock JSON UI tutorial"**
- **"Bedrock resource pack HUD customization"**
- **"Bedrock Edition UI modding"**

Video format is great for seeing the workflow in real-time — watching someone navigate the vanilla files and make changes can accelerate learning significantly.

---

## Community

### Bedrock OSS Discord
The community behind the Bedrock Wiki. Active channels for resource pack and JSON UI questions.

🔗 [discord.gg/XjV87YN](https://discord.gg/XjV87YN)

### Minecraft Creator Discord
Mojang's official Discord for creators (includes Bedrock modding channels).

🔗 Available through [discord.gg/minecraft](https://discord.gg/minecraft)

---

## Recommended Learning Path

If you want to go from beginner to expert, follow this order:

1. ✅ **This guide** — core concepts, elements, basic modifications
2. 📖 **Bedrock Wiki JSON UI** — fill in gaps, advanced properties
3. 🔍 **Read vanilla files** — understand real-world patterns Mojang uses
4. 🧪 **JSON UI Examples repo** — study real community modifications
5. 🔨 **Build your own pack** — start simple (hide one element, move one thing)
6. 🚀 **Tackle a full UI overhaul** — create a complete resource pack

The most important step is #5. Theory only gets you so far — actually making things and breaking them is how you learn fastest.

---

## Staying Up to Date

Minecraft Bedrock updates frequently. UI files can change between versions — elements may be renamed, moved, or restructured. When a new version drops:

1. Download the new vanilla samples from the Mojang repo
2. Check the Bedrock Wiki for changelogs
3. Test your pack in the new version and fix anything that broke

Following the Bedrock OSS Discord is the fastest way to hear about breaking changes.

---

**Good luck, and happy modding!** 🎮
