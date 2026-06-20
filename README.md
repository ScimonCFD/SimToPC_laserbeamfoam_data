# SimToPC laserbeamFoam data

This repository contains reduced `laserbeamFoam` simulation cases used in the
SimToPC `measure_laserbeamfoam` tutorial.

The cases are provided in their native `laserbeamFoam` coordinate convention
so that users can exercise the full workflow, including the coordinate
adaptation step performed by `adapt_case_to_simtopc.py`.

---

## Contents

`laserbeamfoam_native.zip` contains two single-track SS316L simulations:

| Case | Power (W) | Effective scan speed (m/s) | Spot diameter (µm) |
|------|-----------|----------------------------|--------------------|
| `test_case_1` | 200 | 1.25 | 75 |
| `test_case_2` | 100 | 0.90 | 75 |

Both use a pulsed laser on a `200 × 200 × 800 µm³` domain with 5 µm cubic
cells. Only the files required by `adapt_case_to_simtopc.py` are included:

```text
test_case_i/
    constant/
        polyMesh/          ← mesh in native laserbeamFoam orientation
        g                  ← gravity vector (0 -9.81 0)
    system/
        blockMeshDict
    9e-05/
        alpha.metal        ← metal VOF field at final timestep
        solidificationTime ← solidification time field at final timestep
```

---

## Usage

These cases are consumed by the `examples/measure_laserbeamfoam/` tutorial
in the [SimToPC repository](https://github.com/laserbeamfoam/SimToPC).
Follow the instructions in that tutorial's `README.md`.

In brief:

```bash
# 1. Extract
unzip laserbeamfoam_native.zip

# 2. Adapt each case to SimToPC coordinates
python tools/adapt_case_to_simtopc.py \
    --src test_case_1 --dst laserbeamfoam_adapted/test_case_1
python tools/adapt_case_to_simtopc.py \
    --src test_case_2 --dst laserbeamfoam_adapted/test_case_2

# 3. Run SimToPC
simtopc measure config.yml
```

---

## Coordinate convention

In the native cases, `laserbeamFoam` uses:

- `x` → track width
- `y` → build direction (gravity in `-y`)
- `z` → scan direction

`adapt_case_to_simtopc.py` remaps these to the SimToPC convention
(`x` width, `y` scan, `z` build) before post-processing.

---

## Notes

- The full OpenFOAM simulation outputs are not distributed here due to size.
- These datasets are intended for tutorial and demonstration purposes.
- A stable archived version will be made available upon publication.
