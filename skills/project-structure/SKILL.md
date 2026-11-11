---
name: project-structure
description: >-
  Use when naming, placing, or organizing any asset or file in this Unity project: deciding an asset's prefix, suffix, numbering, or which top-level folder it belongs in. Establishes the asset naming pattern (Prefix_AssetName_FamilyNumber+VariationLetter_Suffix), the full prefix and suffix tables (textures, panoramic textures, sound, sprites, materials, animation), and the top-level folder layout (Art, Audio, Code, Data, Plugins, Temp, Settings). Triggers on "name this asset", "where should this file go", "folder structure", "what prefix/suffix", "asset naming", "organize the project".
---

# Project Structure

Naming and folder conventions for every asset and file in this Unity
project. This skill is the source of truth.

## File & folder names

- All names use English (preferably ASCII) in PascalCase. Underscores split
  the prefix, asset name, numbering, and suffix.
- Asset type determines the prefix; purpose determines the suffix.
- Use a suffix only if the asset is part of a series or a variation of
  another asset.
- If numbering is used, use 2 digits for the family number and 1 letter for
  the variation.
- One folder per asset and its dependencies. A folder may hold multiple
  assets of the same type if those assets are used in multiple places.

### Pattern

```
[Prefix]_[AssetName]_[FamilyNumber][VariationLetter]_[Suffix].[Extension]
```

### Example asset folder (Visual FX)

```
QuantumExplosion/
|-- A_QuantumExplosion_Pieces.anim
|-- M_QuantumExplosion_Core.mat
|-- T_QuantumExplosion_Particle_Spark.png
`-- SHG_QuantumExplosion_Pattern.shadergraph
```

### Example asset folder (static mesh)

```
WillowTree_01/
|-- A_WillowTree_01a@idle.fbx
|-- M_WillowTree_01a.mat          (material)
|-- M_WillowTree_01b.mat          (variation using a color map variation)
|-- SM_WillowTree_01a.fbx         (mesh)
|-- T_WillowTree_01a_Color.png    (color / albedo / diffuse)
|-- T_WillowTree_01b_Color.png    (color map variation)
|-- T_WillowTree_01a_MetallicSmoothness.png  (metallic)
|-- T_WillowTree_01a_Normal.png   (normal)
`-- T_WillowTree_01a_AO.png       (AO)
```

## Numbering

- Two digits distinguish different assets (e.g. two different willow trees:
  `WillowTree_01`, `WillowTree_02`).
- A trailing letter marks a variant of the same asset (`01a`, `01b`).
- Digits can be omitted if the asset is likely to be unique.

## Prefixes

| Prefix | Asset |
|--------|-------|
| `A_` | Animation (fbx or clip) |
| `AC_` | Animation Controller |
| `M_` | Material |
| `MP_` | Particle material |
| `P_` | Particle system |
| `PST_` | Preset |
| `S_` | Sound |
| `SHG_` | Shader graph |
| `SH_` | Shader |
| `SK_` | 3D files containing humanoid or generic skeleton joint rigs |
| `SM_` | 3D static meshes |
| `SP_` | Sprite |
| `T_` | Texture |
| `TP_` | Panoramic texture |

## Suffixes

### Textures

| Purpose | Suffix |
|---------|--------|
| Albedo / Color | `_Color` |
| Normal | `_Normal` |
| Emissive | `_Emission` |
| Mask (R=Metallic, G=AO, B=Detail Map, A=Smoothness) | `_MADS` |
| Roughness / Smoothness | `_Roughness` / `_Smoothness` |
| Metallic | `_Metallic` |
| Metallic + Smoothness | `_MetallicSmoothness` |
| Displacement | `_Displacement` |
| Ambient Occlusion | `_AO` |
| Height | `_Height` |
| Light Map (custom) | `_LightMap` |

### Panoramic textures

| Purpose | Suffix |
|---------|--------|
| Equirectangular | `_Equirect` |
| Cubemap standard | `_Cubemap` |

### Sound

| Purpose | Suffix |
|---------|--------|
| Ambisonic | `_Ambisonic` |
| Binaural | `_Binaural` |

### Sprites

| Purpose | Suffix |
|---------|--------|
| Icon | `_Icon` |

### Materials

| Purpose | Suffix |
|---------|--------|
| One of multiple for a mesh | `_subtitle` (e.g. `M_WillowTree_01b_Leaves`) |

### Animation

Animation can be split into multiple fbx files named `model@anim_name.fbx`.
Those files must not contain the mesh itself.

## Folder structure

The top level has the folders below plus a few files.

| Folder | Purpose |
|--------|---------|
| `Art` | All models, shaders, and textures, plus some materials. |
| `Audio` | All sounds. Separated from `Art` to let artists work independently; add folders for other media types if they are independent. |
| `Code` | Project runtime code and Editor tools, except third-party. Corresponds to the root namespace; subfolders follow nested namespaces. Contains `Runtime` (included in builds) and `Editor` (development tools). |
| `Tests` | All project-authored tests, test-only helpers and test asmdefs. Contains `EditMode/<Module>` and `PlayMode/<Module>` (Unity Test Framework assemblies excluded from normal builds). This is the explicit exception to the `Code` root rule. |
| `Data` | Everything that drives runtime but is not art, sound, or code: prefabs, scenes, scriptable objects, volumes, etc. Subfolders: `Prefabs`, `Scenes`. |
| `Plugins` | Third-party code. Named `Plugins` so its scripts compile into a separate assembly. |
| `Temp` | (Optional) Experiments, variants, etc. The project must not change if this folder is removed (nothing links into it). Not committed to git. |
| `Settings` | Global project settings and defaults. |

### Files in the root folder

`README`, license, changelog, and other informational files.
