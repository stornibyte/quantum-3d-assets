# quantum-3d-assets

Hero 3D models (GLB) for the **Quantum Hardstore PC Builder** visor 3D.

Served publicly via **jsDelivr** with cache + CDN edge nodes:

```
https://cdn.jsdelivr.net/gh/stornibyte/quantum-3d-assets@v1/registry.json
https://cdn.jsdelivr.net/gh/stornibyte/quantum-3d-assets@v1/cases/<file>.glb
```

The Quantum armador V6 visor 3D reads `registry.json` on every case pick. If a slug match is found, it loads the GLB and swaps the procedural mesh for the hero model with cinematic fade-in. If no match, the procedural baseline stays — graceful fallback, no errors.

## Structure

```
quantum-3d-assets/
├── cases/                  ← GLB models (Meshopt + KTX2 compressed, <500 KB each)
│   ├── nzxt-h7-flow.glb
│   ├── corsair-4000d.glb
│   └── ...
├── registry.json           ← slug → URL mapping
└── README.md
```

## Operational pipeline

Full step-by-step guide:
[`armador/3D-ASSETS-PIPELINE.md`](https://github.com/stornibyte/q3/blob/main/armador/3D-ASSETS-PIPELINE.md)

Curated Sketchfab models with CC-BY licenses:
[`_workspace/audits/buildcores/3D-HERO-MODELS-CURATED.md`](https://github.com/stornibyte/q3/blob/main/_workspace/audits/buildcores/3D-HERO-MODELS-CURATED.md)

## How to add a hero model

```bash
# 1. Download a CC-BY GLB from Sketchfab
# 2. Optimize
gltf-transform optimize input.glb cases/my-case.glb \
  --simplify 0.6 --meshopt --texture-compress ktx2 --weld

# 3. Update registry.json with entry
# 4. Commit + move tag v1 + push
git add . && git commit -m "feat: add my-case hero model"
git tag -f v1 && git push -f origin v1

# 5. Invalidate jsDelivr cache (optional, 12h default TTL)
curl https://purge.jsdelivr.net/gh/stornibyte/quantum-3d-assets@v1/registry.json
curl https://purge.jsdelivr.net/gh/stornibyte/quantum-3d-assets@v1/cases/my-case.glb
```

## Licenses

All models hosted here must be CC0, CC-BY 4.0, or fully owned by Quantum (photogrammetry, freelancer with IP transfer). NEVER CC-BY-NC, CC-BY-SA, or proprietary.

Attribution (CC-BY) is shown to end users in the armador-pc page footer via `<details><summary>Créditos 3D</summary>...` markup.

## Status

| Slot | Hero model | Source | License | Status |
|------|------------|--------|---------|--------|
| `gabinete-gamer-aureox-skoll-arx-200g-16wcb` | BoomBox (test only) | Khronos sample | CC0 | placeholder · reemplazar |

This is a fresh repo. The single entry is a PoC pointer to a Khronos Public Domain sample model (BoomBox) to validate the end-to-end load flow. Replace with real PC case GLBs as they are downloaded + optimized.
