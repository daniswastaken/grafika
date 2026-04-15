## How to Expand to Other Mobs

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

## Credits
- **Developer**: daniswastaken
- **Foundation**: Built upon Newb Shaders by devendrn.

---
**Grafika is fully open source.** For the shader source code and development tools, see the [Grafika-Dev Repository](https://github.com/daniswastaken/grafika-dev).
