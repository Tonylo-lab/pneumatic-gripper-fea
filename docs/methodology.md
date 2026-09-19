# Methodology

## 1. Baseline nonlinear model

The original SolidWorks gripper assembly was transferred to ANSYS Workbench / Mechanical 2026 R1 and evaluated at a prescribed actuator retraction of **-28.8 mm**.

### Main modeling assumptions

- Printed structural parts: homogenized isotropic FDM PLA
- Young's modulus: **2.0 GPa**
- Poisson's ratio: **0.35**
- Density: **1240 kg/m^3**
- Cube: rigid
- Block-cube contact: frictionless
- Pivot connections: revolute joints
- Actuator rod: translational joint
- Large Deflection: On

The actuator is displacement-controlled because the real pneumatic-cylinder force was not available. Therefore, actuator reaction force is used as a comparative system-response metric rather than as a directly measured gripping-force rating.

## 2. Gripping-load study

The actuator displacement was swept from **-20 mm to -28.8 mm**. The mechanism first reached a four-contact gripping state at approximately **-22.5 mm**. Reaction force increased rapidly toward the end of travel, indicating a strong rise in mechanism stiffness after full contact.

## 3. Mesh convergence

Four mesh levels were compared at the final gripping position.

| Mesh | Nodes | Elements | Reaction Force (N) | Max Eq. Stress (MPa) | Max Principal Stress (MPa) | Max Contact Pressure (MPa) |
|---|---:|---:|---:|---:|---:|---:|
| M0 | 115,014 | 55,882 | 257.22 | 47.865 | 61.712 | 19.812 |
| M1 | 128,372 | 62,989 | 257.06 | 48.659 | 62.150 | 19.826 |
| **M2** | **168,478** | **84,428** | **256.67** | **48.007** | **57.060** | **19.779** |
| M3 | 219,611 | 113,578 | 256.28 | 49.724 | 58.151 | 19.767 |

M2 was selected as the production mesh. M2-to-M3 differences were approximately **0.15%** in reaction force and **3.45%** in maximum equivalent stress.

## 4. Component screening

The baseline model was used to identify redesign candidates.

| Component | Mass (g / part) | Max Eq. Stress (MPa) | Design interpretation |
|---|---:|---:|---|
| Jaw Plate | 40.1 | 8.9373 | Good lightweighting candidate |
| Actuation Link | 41.5 | 36.71 | Preserve Y-fork / pivot load path |
| Mounting Frame | 367.5 | 37.115 | Large mass opportunity, excluded from final scope |

The actuation-link Y-fork showed a localized hotspot and was therefore not selected for aggressive material removal.

## 5. Case A1 — contact-interface correction

The deformed baseline configuration showed approximately **5.3 deg** of contact-face misalignment near the final gripping position.

A full 5.3 deg correction reduced available closure too much because the actuator was already close to its physical stroke limit. A **3 deg wedge correction** was therefore adopted.

### Nonlinear solver settings used for the validated A1 model

- Block-cube contact: frictionless
- Contact stabilization damping factor: **0.1**
- Initial / minimum / maximum substeps: **100 / 20 / 2000**
- Maximum equilibrium iterations per substep: **50**
- Large Deflection: On

## 6. Case A2 — topology-informed jaw-plate redesign

Topology optimization was not run directly on the full nonlinear assembly. Instead, a separate linearized jaw-plate model was created from the A1 load state.

### Optimization setup

- Design region: Jaw Plate
- Objective: minimize compliance
- Mass constraint: **70% retained**
- Method: topology optimization — mixable density
- Large Deflection: Off in the simplified optimization model

The raw density result was treated as a **load-path reference**, not as final CAD. The jaw plate was manually redesigned into a clean manufacturable geometry and returned to the complete nonlinear gripper model.

## 7. Final full-assembly validation

The final A2 CAD was validated in the complete nonlinear mechanism with the original actuator displacement, contact assumptions, joints, material model, and large-deflection setting retained.

This final step is essential because the topology study itself does not capture the full nonlinear contact behavior of the assembled gripper.
