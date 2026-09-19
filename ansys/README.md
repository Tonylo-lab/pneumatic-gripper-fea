# ANSYS Project Files

The analysis was developed in ANSYS Workbench / Mechanical 2026 R1.

The full Final A2 Workbench archive is approximately **900 MB**, so it is intentionally not stored directly in the Git repository. A `.wbpz` archive is the preferred reproducible package because a standalone `.wbpj` file depends on its associated project-data directory.

## Full Workbench archive

Expected release asset:

- `pneumatic_gripper_final_A2.wbpz`

Release tag:

- `v1.0`

Once published, the archive will be available from the repository's **Releases** page.

## Distribution strategy

- Git repository: README, methodology, figures, result tables, Excel workbook, and CAD STEP files.
- GitHub Release: full Final A2 Workbench archive (`.wbpz`).
- Baseline and A1/topology are documented in the repository and do not need separate large Workbench archives for this portfolio.

Large solver-generated files and unpacked Workbench cache directories are excluded from version control.
