# Pneumatic Gripper FEA Design Study

A nonlinear finite-element design study of a pneumatically actuated gripper, developed from an existing SolidWorks mechanism and evaluated in ANSYS Mechanical.

The project focuses on three stages:

1. **Baseline** — characterize the original mechanism, gripping response, stress field, contact behavior, and mesh sensitivity.
2. **Case A1 — Contact Interface Correction** — correct the jaw-contact-block face orientation to improve contact load transfer.
3. **Case A2 — Topology-Informed Jaw Plate Redesign** — use topology optimization to guide a lighter jaw plate, then validate the redesigned plate in the complete nonlinear gripper assembly.

> Final design path: **Baseline → Case A1 → Case A2 (Final Design)**

---

## Project Overview

The gripper was originally designed in SolidWorks for a robotics application. It clamps a hollow **150 × 150 × 150 mm** cube using a pneumatic actuator and a multi-link mechanism.

The complete assembly was modeled in ANSYS Mechanical with:

- prescribed actuator displacement of **−28.8 mm**
- **Large Deflection = On**
- rigid target cube
- frictionless jaw-block-to-cube contact
- revolute joints at mechanism pivots
- a translational joint for the actuator rod
- homogenized isotropic FDM PLA for printed structural parts

### PLA material model

| Property | Value |
|---|---:|
| Young's modulus | 2.0 GPa |
| Poisson's ratio | 0.35 |
| Density | 1240 kg/m³ |

---

## Baseline Model

The baseline study established the mechanism response over the gripping stroke and identified the dominant stress and contact behavior.

The mechanism reaches a four-contact gripping state at approximately **−22.5 mm** actuator displacement and becomes substantially stiffer toward the end of travel.

### Baseline production results

| Metric | Baseline |
|---|---:|
| Actuator reaction force | **256.67 N** |
| Max equivalent stress | **48.007 MPa** |
| Max principal stress | **57.06 MPa** |
| Max contact pressure | **19.779 MPa** |
| Jaw plate mass / part | **40.10 g** |
| Jaw plate max equivalent stress | **8.9373 MPa** |

### Component screening

The baseline model was also used to decide which parts were suitable for redesign.

- **Jaw Plate:** low stress relative to its mass, making it a good lightweighting candidate.
- **Actuation Link:** high local stress around the Y-fork / pivot region, so aggressive material removal was avoided.
- **Mounting Frame:** high absolute mass, but it was excluded from the final project scope to keep the design study focused.

---

## Mesh Convergence

A four-level mesh convergence study was performed at the final **−28.8 mm** gripping displacement.

| Mesh | Nodes | Elements | Reaction Force (N) | Max Eq. Stress (MPa) | Max Principal Stress (MPa) | Max Contact Pressure (MPa) |
|---|---:|---:|---:|---:|---:|---:|
| M0 | 115,014 | 55,882 | 257.22 | 47.865 | 61.712 | 19.812 |
| M1 | 128,372 | 62,989 | 257.06 | 48.659 | 62.150 | 19.826 |
| **M2** | **168,478** | **84,428** | **256.67** | **48.007** | **57.060** | **19.779** |
| M3 | 219,611 | 113,578 | 256.28 | 49.724 | 58.151 | 19.767 |

**M2** was selected as the production mesh. Relative to M3, the reaction-force difference is about **0.15%**, while the equivalent-stress difference is about **3.45%**.

---

## Case A1 — Contact Interface Correction

Inspection of the deformed baseline configuration showed approximately **5.3°** contact-face misalignment at the final gripping position.

A full 5.3° geometric correction reduced available closure too much because the actuator was already near its physical stroke limit. A reduced **3° wedge correction** was therefore used.

### A1 solver setup

- Block–cube contact: **Frictionless**
- Contact stabilization damping factor: **0.1**
- Automatic time stepping: **100 / 20 / 2000** initial / minimum / maximum substeps
- Maximum equilibrium iterations per substep: **50**
- Large Deflection: **On**

### A1 results

| Metric | Baseline | Case A1 | Change |
|---|---:|---:|---:|
| Actuator reaction force | 256.67 N | **48.561 N** | −81.1% |
| Max contact pressure | 19.779 MPa | **1.1391 MPa** | −94.2% |
| Total contact area | 32.378 mm² | **122.72 mm²** | +279.0% |
| Global max equivalent stress | 48.007 MPa | **16.782 MPa** | −65.0% |
| Global max principal stress | 57.06 MPa | **27.354 MPa** | −52.1% |

Because the model is displacement-controlled, reaction force is interpreted primarily as a measure of end-of-travel system stiffness rather than as a direct gripping-force rating.

The main purpose of A1 was to reduce edge-dominated contact loading and improve the contact interface before lightweighting the jaw plate.

---

## Case A2 — Topology-Informed Jaw Plate Redesign

A separate linearized jaw-plate study was created from the A1 load state to support topology optimization.

### Topology setup

- Design region: **Jaw Plate**
- Objective: **Minimize compliance**
- Mass constraint: **70% retained**
- Optimization method: **Mixable Density**
- Final CAD method: **Topology-informed manual redesign**

The raw topology result was used as a load-path reference rather than as final manufacturable geometry. A clean CAD redesign was then created and returned to the complete nonlinear gripper model for validation.

---

## Final A2 Full-Assembly Validation

| Metric | Case A1 | Final A2 | Change |
|---|---:|---:|---:|
| Jaw plate mass / part | 40.10 g | **31.198 g** | **−22.2%** |
| Jaw plate max equivalent stress | 7.7262 MPa | **8.9109 MPa** | +15.3% |
| Actuator reaction force | 48.561 N | **49.848 N** | +2.65% |
| Max contact pressure | 1.1391 MPa | **0.57908 MPa** | **−49.2%** |
| Total contact area | 122.72 mm² | **287.179 mm²** | **+134.0%** |
| Global max equivalent stress | 16.782 MPa | **11.485 MPa** | **−31.6%** |
| Global max principal stress | 27.354 MPa | **14.537 MPa** | **−46.9%** |

The final jaw plate is approximately **22% lighter** than the A1 plate while the actuator reaction force remains nearly unchanged. The redesigned assembly also shows lower global stress and a larger, lower-pressure contact footprint.

Relative to the original baseline, the final jaw-plate stress is essentially unchanged (**8.9373 MPa → 8.9109 MPa**) despite the mass reduction.

---

## Engineering Takeaways

This project was structured as a design-validation workflow rather than a single FEA run:

- use the baseline model to identify the actual load path and critical components
- verify mesh sensitivity before redesign
- correct contact-interface geometry before lightweighting
- use topology optimization as a design guide, not as final CAD
- return the redesigned part to the complete nonlinear assembly for final validation

The actuation-link hotspot also demonstrates why component selection matters: not every low-volume region is a good candidate for material removal.

---

## Repository Structure

```text
pneumatic-gripper-fea/
├── README.md
├── assets/
│   ├── baseline/
│   ├── case-a1/
│   ├── topology/
│   └── final-a2/
├── results/
│   └── gripper_fea_results.xlsx
├── cad/
└── ansys/
```

Additional CAD, ANSYS project files, plots, and result images will be added as the repository is completed.

---

## Software

- SolidWorks
- ANSYS Workbench / Mechanical 2026 R1
- ANSYS Discovery

---

## Scope and Limitations

This study uses a simplified isotropic material model for FDM PLA and prescribed actuator displacement because the actual pneumatic cylinder force was not available. The rigid cube and frictionless jaw-block contact are modeling assumptions for design comparison rather than a complete experimental characterization of real gripping performance.
