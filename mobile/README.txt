Mobile Optimized Models for Grafika
===================================

These models are simplified versions of the high-poly player and armor outlines.
They use standard inflated boxes instead of thousands of tiny cubes, which prevents
the rendering corruption (broken blocks) seen on Android devices.

To use these models on mobile:
1. Replace the corresponding files in your subpacks:
   - player_outline.geo.json -> subpacks/player_outline/models/entity/
   - wide_player_outline.geo.json -> subpacks/player_outline/models/entity/
   - player_armor_1.json -> subpacks/all_outline/models/entity/
   - player_armor_2.json -> subpacks/all_outline/models/entity/

Hardware Note:
Mobile RenderDragon often has a 65,536 vertex limit per draw call. High-poly voxel 
models (like the 20MB slim model) exceed this limit by 4x, causing the GPU driver 
to scramble the geometry. These box models stay well under that limit while 
maintaining a clean outline effect.
