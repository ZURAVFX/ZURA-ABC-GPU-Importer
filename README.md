# ZURA ABC GPU Importer

**A faster way to turn Alembic poly-mesh animation into native Blender shape-key animation.**

ZURA ABC GPU Importer reads `.abc` mesh samples directly through a compiled Alembic backend instead of asking Blender to evaluate the Alembic cache frame-by-frame through the usual modifier/depsgraph path. The result is a streamlined bake workflow for VFX artists working with large, stable-topology caches.

## Why it exists

Alembic is excellent for moving geometry between DCCs, but heavy caches can become frustrating when you need the animation converted into editable Blender data. This add-on is designed around that specific bottleneck.

## Key features

- Direct `.abc` sample reads through the Alembic C++ library
- Builds the result mesh once, then bakes animation into shape keys
- Applies Alembic parent transforms during sampling for more faithful orientation
- Optional CUDA max-delta comparison for **Skip Identical** on supported NVIDIA GPUs
- Scan-first workflow showing mesh, sample, vertex and face information
- Configurable start/end timeline range
- Cancelable bake with progress/status feedback
- Leaves already baked shape keys intact if a bake is cancelled

## Current scope

This first public build is intentionally focused:

- Windows 64-bit
- Blender 5.1 / CPython 3.13 build
- Poly-mesh Alembic caches
- Stable / non-heterogeneous topology required for shape-key baking
- Defaults to the first poly mesh found, with optional explicit mesh path

The largest performance gain comes from bypassing Blender's per-frame Alembic evaluation. The CUDA comparison path is secondary and is most useful on very large caches when skipping identical samples.

## Install

1. Download `ZURA_ABC_GPU_Importer_Blender51_Win64.zip` from the `dist` folder or the project website.
2. In Blender, open **Edit → Preferences → Add-ons**.
3. Choose **Install from Disk…** and select the ZIP file.
4. Enable **Fast Baker Direct ABC GPU**.
5. Open the add-on panel, choose an Alembic file, run **Scan Alembic**, set the frame range, then use **Bake Direct Alembic**.

> The internal add-on name is currently **Fast Baker Direct ABC GPU**. ZURA ABC GPU Importer is the public project/release name.

## Notes

- Shape keys require matching vertex topology across samples. Heterogeneous-topology caches are rejected rather than producing unsafe geometry.
- GPU comparison is optional. The direct Alembic reader is the main acceleration path.
- This is an early public release. Please use GitHub Issues for reproducible bugs and compatibility reports.

## Credits

Created by **ZURA VFX** for production VFX and motion-design workflows.

Alembic, Imath, pybind11, Blender and NVIDIA/CUDA are third-party projects/trademarks belonging to their respective owners.
