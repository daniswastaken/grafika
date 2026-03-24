# Grafika Shader

**Grafika** is a high-performance, anime-style shader pack for Minecraft Bedrock (RenderDragon). It combines advanced environmental rendering with a standalone **Stencil Outline System** for a next-gen visual experience.

## 🎨 Why Anime-Style?
The "anime-style" branding comes from the pack's emphasis on clean, bold visual separation. Just like the hand-drawn outlines in classic animation, our **Stencil Outline System** provides every entity with a sharp border. This creates a distinct visual hierarchy where players and mobs are clearly defined against the world's backgrounds.

> [!IMPORTANT]
> This pack requires **BetterRenderDragon (BRD)** or **Minecraft Patched/MB Loader** to function correctly.

## ✨ Core Features

### 🌌 Shipped Shader Features
- **The Singularity (End Sky)**: A cinematic, procedurally rendered black hole with realistic gravitational lensing and accretion rings.
- **Glowing Ores**: High-visibility, shimmering emissive textures for all ores, making cave exploration both beautiful and efficient.
- **Enhanced Lava**: Dynamic noise-based lava "bump" patterns for more realistic volcanic environments.
- **Atmospheric Rendering**: Custom sun/moon paths, refined fogs, and vibrant, multi-layered End lighting.

### 🔥 Stencil Outline System (ESP)
Grafika provides an option for a permanent, white ESP outline for players and mobs. This is built entirely on Minecraft's client-side rendering engine (Stencil Buffer) and requires no Behavior Packs.

#### How it Works:
1. **The Mask (Base Mesh)**: The entity mesh is rendered with `outline_base`, writing a reference value (32) to the Stencil Buffer.
2. **The Outline (Overlay)**: A second, slightly larger geometry is rendered with `outline`. It only draws where the stencil value is **NotEqual** to 32, creating a perfect halo.

## 📁 Subpack Configuration
Grafika is modular. You can select your preferred configuration in the Resource Pack settings:
- **Shader Only**: Use only the environmental shaders without entity outlines.
- **Player Outline Only**: Apply the outline/ESP effect only to player entities.
- **All Outline §6[BETA]**: Enable outlines for all supported mobs and players.

---

## 🚀 How to Expand to Other Mobs

To add the outline effect to a new mob (e.g., a Zombie), follow these steps:

### Phase 1: Preparation

1. Create a larger "outline" version of the mob's geometry (usually by inflating the cubes slightly in Blockbench).
2. Add the mob's base texture and the outline geometry to the mob's `.entity.json`.

### Phase 2: Stencil Masking

Modify the mob's standard Render Controller (e.g., `controller.render.zombie`):

- Change the material from `default` to `outline_base`.
- This ensures the zombie writes to the stencil buffer.

### Phase 3: The Outline Controller

Create a new render controller `controller.render.zombie_outline`:

```json
{
    "format_version": "1.10.0",
    "render_controllers": {
        "controller.render.zombie_outline": {
            "geometry": "Geometry.zombie_outline",
            "materials": [{ "*": "Material.outline" }],
            "textures": ["Texture.default"],
            "overlay_color": { "r": 1.0, "g": 1.0, "b": 1.0, "a": 1.0 },
            "ignore_lighting": true
        }
    }
}
```

### Phase 4: Integration

In the mob's `.entity.json`:

1. Add the new render controller to the `render_controllers` array.
2. **CRITICAL:** Ensure it is placed **after** the base render controller.

---

## 📜 Credits
- **Developer**: daniswastaken
- **Foundation**: Built upon Newb Shaders by devendrn.

---
**Grafika is fully open source.** For the shader source code and development tools, see the [Grafika-Dev Repository](https://github.com/daniswastaken/grafika-dev).
