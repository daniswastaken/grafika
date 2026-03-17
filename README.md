# Experimental Decoupled Outline RP (White ESP)

This Resource Pack (RP) provides a permanent, white ESP outline for players. It has been fully decoupled from Behavior Pack (BP) dependencies, meaning it functions standalone based on Minecraft's client-side rendering engine (Stencil Buffer).

---

## 🛠 How the System Works

The outline effect is achieved using **Stencil Masking**. This is a two-pass rendering process:

1.  **The Mask (Base Mesh):** The base entity model (player skin, armor, cape, etc.) is rendered using the `outline_base` material. This material writes a specific value (Ref 32) into the Stencil Buffer.
2.  **The Outline (Overlay):** A second, slightly larger geometry is rendered using the `outline` material. This material is configured to:
    - **Depth Test: Always** (This creates the ESP effect, allowing it to be seen through walls).
    - **Stencil Test: NotEqual** (It only renders pixels where the stencil value is *not* 32).
    - Since the base mesh already filled the stencil buffer at the player's position, the larger outline only renders in the "halo" around the player.

### Why was it breaking before?
In previous versions, the base mesh only used `outline_base` if certain Behavior Pack properties were met. If those properties failed, the base mesh wouldn't write to the stencil buffer, causing the "Always Visible" outline to fill the entire player model, resulting in a solid white silhouette.

---

## 📁 Component Breakdown

### 1. Materials (`materials/entity.material`)
- `outline_base`: Writes to the Stencil Buffer. Must be used by the **Underlying Mesh**.
- `outline`: The actual glowing effect. Uses `depthFunc: Always` for ESP work.

### 2. Render Controllers
- **Base Controllers (`player.render_controllers.json` etc.):** Forced to use `Material.outline_base` globally.
- **Overlay Controllers (`slim/outline_player.render_controllers.json` etc.):**
    - Material: `Material.outline`
    - Color: Fixed at `{ 1.0, 1.0, 1.0, 1.0 }` (White).
    - `ignore_lighting: true`: Ensures the white remains vibrant in the dark.

### 3. Entity Definition (`entity/player.entity.json`)
- **Render Stack:** The base mesh controllers are listed **before** the outline controllers. This order is mandatory for the stencil mask to exist when the outline tries to read it.
- **Property Free:** All `q.property` calls were removed. Skin type (Slim/Wide) is now detected via bone pivots.

---

## 🚀 How to Expand to Other Mobs

To add this effect to a new mob (e.g., a Zombie), follow these steps:

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
      "materials": [ { "*": "Material.outline" } ],
      "textures": [ "Texture.default" ],
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

## ⚠️ Notes
- Since the ESP uses `Depth: Always`, the outline will render "over" everything except other stencil-masked objects.
- This version is optimized for performance by removing unnecessary property evaluations during the render tick.
