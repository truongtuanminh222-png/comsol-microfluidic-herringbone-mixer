# Flow-Focusing Herringbone Micromixer — COMSOL Multiphysics Simulation

A COMSOL simulation quantifying how herringbone groove structures improve passive mixing efficiency in a microfluidic device, from **12.4% to 89.2%**.

<img width="1004" height="433" alt="image" src="https://github.com/user-attachments/assets/9c548ab0-eae8-4431-99eb-f8c49387c181" />

## Background

At the microscale, flow is almost always laminar (low Reynolds number) — there's no turbulence to mix fluids, so two streams merging in a straight channel stay largely separated except for slow molecular diffusion. This is a real bottleneck in lab-on-chip and biosensor applications, where fast, complete mixing of a sample and reagent is often required before detection.

This project compares two mixer designs to see how much a passive geometric feature — herringbone grooves — can compensate for the lack of turbulence.

## Method

- **Software:** COMSOL Multiphysics
- **Physics:** Laminar Flow (`spf`) coupled with Transport of Diluted Species (`tds`)
- **Geometry:** Imported from `.dxf` CAD files, isolated using boolean Difference/Intersection operations
- **Model split:**
  - *2D model* — full flow-focusing geometry, used to establish baseline concentration and velocity fields without herringbone influence
  - *3D segment* — a focused model of just the herringbone mixer section, used to isolate the effect of the grooves on mixing
- **Boundary conditions:** inlet velocities of 40 mm/s (c = 0) and 10 mm/s (c = 1); fully-mixed target concentration c∞ = 0.2, from a flow-weighted average of the two inlets
- **Solver:** stationary solver for the velocity field, followed by species transport, for a steady-state analysis

The 2D and 3D models were solved separately rather than as one continuous domain, both to keep the simulation computationally manageable and because COMSOL's 2D serpentine solution tended to assume a fully-mixed state by the time it reached the 3D input. To keep the 3D result a genuine test of the mixer geometry, the peak concentration at the 2D junction was used as the 3D model's inlet condition instead.

## Results

<img width="1012" height="440" alt="image" src="https://github.com/user-attachments/assets/469a9ec6-115e-4f59-be3c-314e8e62e1e6" />

| Configuration | Mixing Ratio (M) | Observation |
|---|---|---|
| 2D channel (no grooves) | 12.4% | Mixing driven only by molecular diffusion. A sharp, needle-like concentration gradient persists at the outlet. |
| 3D herringbone segment | 89.2% | Grooves trigger chaotic advection, stretching and folding fluid layers, increasing the diffusion contact area. |

Mixing ratio is defined as:

```
M = (1 − ∫outlet(c − c∞)² / ∫inlet(c − c∞)²) × 100%
```

**Takeaway:** the herringbone grooves don't add turbulence — they physically fold the fluid layers over each other (chaotic advection), which shortens the diffusion distance and multiplies the contact area between the two species. That mechanism alone accounts for the jump from 12.4% to 89.2% mixing efficiency, with no change in flow rate or fluid properties.

## Repository contents

```
├── report/    → full write-up (PDF) with methodology and discussion
├── figures/   → exported geometry and streamline plots
└── model/     → COMSOL .mph file (solution data stripped to keep file size small)
```

> **Note:** the `.mph` file requires a COMSOL license to open. It's included for transparency/reproducibility — the report and figures are the best way to review the work without COMSOL installed.

## Tools

COMSOL Multiphysics — Laminar Flow, Transport of Diluted Species, CAD import (.dxf)
