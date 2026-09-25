<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/voxolith/.github/main/profile/lockup-dark.svg">
    <img alt="Voxolith — WebGPU voxel engine" src="https://raw.githubusercontent.com/voxolith/.github/main/profile/lockup.svg" width="520">
  </picture>
</p>

**Voxolith** is a WebGPU voxel engine for the browser: a fullscreen raymarcher over dense voxel
grids with coarse occupancy skipping, soft shadows, ambient occlusion and materials, plus
MagicaVoxel `.vox` and Minecraft `.mca` I/O. It ships as raw TypeScript and compiles with your app.

| repo | what | try it |
|---|---|---|
| [renderer](https://github.com/voxolith/renderer) | the engine, `@voxolith/renderer` on npm | [README](https://github.com/voxolith/renderer#readme) |
| [engine](https://github.com/voxolith/engine) | `@voxolith/engine`: entities, generator contract, input, animation, atmosphere, streaming | [README](https://github.com/voxolith/engine#readme) |
| [generators](https://github.com/voxolith/generators) | procedural trees, bushes, grass, rocks, buildings, creatures and terrain | [README](https://github.com/voxolith/generators#readme) |
| [viewer](https://github.com/voxolith/viewer) | `.vox` / `.mca` viewer | [voxolith.github.io/viewer](https://voxolith.github.io/viewer/) |
| [editor](https://github.com/voxolith/editor) | voxel editor (early) | [voxolith.github.io/editor](https://voxolith.github.io/editor/) |
| [examples](https://github.com/voxolith/examples) | minimal example pages | [voxolith.github.io/examples](https://voxolith.github.io/examples/) |
| [demolition-shot](https://github.com/voxolith/demolition-shot) | mobile demolition game, the engine showcase (installable PWA) | [voxolith.github.io/demolition-shot](https://voxolith.github.io/demolition-shot/) |

WebGPU only: use a current Chrome, Edge, Safari 26+ or Firefox with WebGPU enabled. Everything
public is MIT.

Voxolith is built with AI assistance: most of its code is written with Claude and reviewed and
run by a human. Contributions are welcome, AI-assisted ones included; see
[CONTRIBUTING](https://github.com/voxolith/.github/blob/main/CONTRIBUTING.md).
