# Design Evolution and Results

## Baseline

| Metric | Value |
|---|---:|
| Actuator reaction force | 256.67 N |
| Max contact pressure | 19.779 MPa |
| Total contact area | 32.3784 mm^2 |
| Global max equivalent stress | 48.007 MPa |
| Global max principal stress | 57.06 MPa |
| Jaw plate max equivalent stress | 8.9373 MPa |
| Jaw plate mass / part | 40.1 g |

## Case A1 — Contact Interface Correction

| Metric | Baseline | Case A1 | Change |
|---|---:|---:|---:|
| Actuator reaction force | 256.67 N | 48.561 N | -81.1% |
| Max contact pressure | 19.779 MPa | 1.1391 MPa | -94.2% |
| Total contact area | 32.3784 mm^2 | 122.72 mm^2 | +279.0% |
| Global max equivalent stress | 48.007 MPa | 16.782 MPa | -65.0% |
| Global max principal stress | 57.06 MPa | 27.354 MPa | -52.1% |
| Jaw plate max equivalent stress | 8.9373 MPa | 7.7262 MPa | -13.6% |

The large drop in actuator reaction force should not be interpreted as a direct loss of measured gripping force. The model is displacement-controlled, so the reaction primarily reflects end-of-travel stiffness and contact over-closure under the prescribed displacement.

## Final A2 — Topology-Informed Jaw Plate

| Metric | Case A1 | Final A2 | Change |
|---|---:|---:|---:|
| Jaw plate mass / part | 40.1 g | 31.198 g | **-22.2%** |
| Jaw plate max equivalent stress | 7.7262 MPa | 8.9109 MPa | +15.3% |
| Actuator reaction force | 48.561 N | 49.848 N | +2.65% |
| Max contact pressure | 1.1391 MPa | 0.57908 MPa | **-49.2%** |
| Total contact area | 122.72 mm^2 | 287.179 mm^2 | **+134.0%** |
| Global max equivalent stress | 16.782 MPa | 11.485 MPa | **-31.6%** |
| Global max principal stress | 27.354 MPa | 14.537 MPa | **-46.9%** |

## Baseline-to-final interpretation

The final jaw plate is approximately **22.2% lighter** than the original plate.

Despite this reduction in mass, the final jaw-plate stress is essentially unchanged relative to the original baseline:

**8.9373 MPa -> 8.9109 MPa**

At the full-assembly level, the final A2 model also shows:

- much lower peak contact pressure
- substantially larger total contact area
- lower global equivalent and principal stress
- nearly unchanged actuator reaction force relative to A1

These results support the design decision to use topology optimization as a load-path guide and then validate a cleaned-up CAD redesign in the full nonlinear assembly.
