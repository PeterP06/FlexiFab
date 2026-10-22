# FlexiFab

## Installation

1. Install **Rhino 8** with Grasshopper and the native **Python 3 Script** component. The definition was tested on macOS with Rhino **8.35.26251.13002**.
2. Install the Grasshopper plugins listed below. Use Rhino's `PackageManager` where available, or follow the plugin author's Rhino 8 installation instructions. Restart Rhino after installation.

   | Plugin | Assembly version used by the definition |
   | --- | --- |
   | DKUI | 26.9.9.18644 |
   | LunchBox | 2025.5.5.0 |
   | Wombat / WombatGH | 1.3.0.0 |
   | Pancake | 2.8.1.0 |
   | Fologram For Grasshopper Utilities | 2024.2.26.0 |
   | Sasquatch | 1.0.0.0 |

3. Download [FlexiFab.gh](FlexiFab.gh). All Python code is embedded in the definition; no separate Python files or pip packages are required.
4. Open Rhino and set the document units to **millimeters**.
5. Run `Grasshopper`, then use **File > Open inside Grasshopper** to open `FlexiFab.gh`.
6. Enable the Grasshopper solver. Set **Open FlexiFab UI** to `True` to display the interface.

## Load a Model

1. Open or create the model in the current Rhino document.
2. Use **Import Model** in the FlexiFab interface to select it. Select the model again when opening the definition in a different Rhino document.
3. Use a closed Brep, or an open roof surface with a single height at each XY position. Join adjacent roof surfaces before selecting them. Vertical sheets, folded roofs, and overlapping roof layers are not supported by the open-surface workflow.
4. For an open roof, adjust **OpenBaseDepth** in Grasshopper if a lower closing base is needed.

## Generate and Adjust the Parts

Use the interface to adjust the layout and inspect the preview:

| Control | Use |
| --- | --- |
| Grid Spacing | Set the spacing between the orthogonal panel slices. |
| Grid Phase X / Y | Shift the slice grid while keeping its count and spacing. Use values between -1 and 1. |
| Tunable | Enable the paired long and short threaded connectors. Disable it to retain only the short smooth ends. |
| Omit Edge Joints | Optionally omit paired edge slots and connectors where the outside strip is narrow. |
| Edge Width Ratio | Set the narrow-strip threshold as a fraction of the unit width. |

Additional controls are available on the Grasshopper canvas:

- **TopMargin** and **TopOnly** set the blank top region. **RootMargin** sets the full-width blank band above the connector roots.
- **UnitPeriodFactor (k)** and **UnitPeriodOffset (mm)**, in the **UNIT PERIOD: k x GridSpacing + b** group, set the horizontal cell period: `UnitPeriod = k * GridSpacing + b`. For example, `k = 0.8` and `b = 0` shorten the period by 20% without changing panel spacing.
- **Dy** controls half of the vertical cell period; the full vertical period is `2 * Dy`.
- **Thickness**, **Clearance**, and the connector length, pitch, and radius controls set the panel and joint dimensions.

Inspect **Assembly** for the assembled panels, **Cut2D** for the flat cutting outlines, and **Report** for parameter errors or geometry messages. Cells that would create disconnected fragments are automatically filled solid.

When using **Omit Edge Joints**, inspect Assembly for interference at the omitted joints: the crossing panels retain their full outlines.

## Export

1. Check the assembled preview and flat outlines after changing parameters.
2. Enter a valid DXF destination on your computer in the interface. The definition does not include a preset export path.
3. Run the interface's export action. Alternatively, bake **Cut2D** into Rhino and export those curves as DXF.

## If the Interface Does Not Appear

- Confirm that DKUI and the other listed plugins loaded successfully, then restart Rhino if necessary.
- Confirm that the Grasshopper solver is enabled.
- Toggle **Open FlexiFab UI** from `False` to `True`.
- If the component reports an error, read **Report**, correct the model or parameters, and recompute.
