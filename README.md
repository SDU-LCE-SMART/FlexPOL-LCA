# A python-based modular tool for Flexible, Prospective, and Policy-Oriented Life Cycle Assessment (FlexPOL-LCA)

## Status

| Item | Value |
|---|---|
| Project state | Finished and finalized |
| Collaborators | Mohammadhossein Narang, Hadis Marami, Benyamin Khoshnevisan |
| Contact person | mhna@igt.sdu.dk, bekh@igt.sdu.dk (If you are willing to use BETA version please contact us.)|
| Open-source plan | This repository will be made open source after the scientific papers are published. |

A modular Python-based Life Cycle Impact Assessment (LCIA) tool with a desktop GUI for matching, filtering, calculation, and result visualization.

The application is organized into independent modules and orchestrated through `main.py`, with an interactive interface in `GUI_modular.py`.

## Tool Overview

This project is a modular tool for performing Life Cycle Impact Assessment workflows in a structured, reproducible, and user-accessible way. It is designed to connect foreground process data with background environmental datasets, support iterative matching and filtering, run burden calculations, generate structured result workbooks, and produce visual outputs for interpretation. The system brings these steps together in a single workflow so that users do not need to manage separate scripts for preprocessing, matching, calculation, and reporting. Instead, the tool provides a guided desktop interface backed by modular Python components, allowing users to configure inputs, run different workflow stages, and inspect outputs with a much lower operational barrier than a code-only pipeline.

The project was designed to address a practical gap between flexible research code and usable decision-support software. In many LCIA workflows, analysts manually connect foreground entries to background databases, repeat filtering rounds, rebuild result sheets, and generate figures through disconnected tools, which increases effort and reduces reproducibility. The novelty of this project is not just in performing individual LCIA tasks, but in integrating matching, iterative refinement, row-level calculation selection, automated result generation, and visualization into one coherent, modular environment. Its architecture separates configuration handling, matching logic, calculation logic, and orchestration, which makes the tool easier to maintain, adapt to new datasets, and extend to other domains beyond agriculture while still remaining usable through a GUI for non-developer users.

## Modular Architecture

The model is organized into eight modules that separate configuration, scenario preparation, matching, impact calculation, visualization, and future analytical extensions. This structure makes the workflow easier to understand, maintain, and extend because each module has a clear responsibility in the overall pipeline. Together, these modules form a complete LCIA toolchain from data preparation to result interpretation.

### Module 0: Configuration Module

This module manages the runtime configuration of the tool, including file paths, column definitions, scenario settings, and general execution parameters. It ensures that the rest of the system receives consistent and validated inputs before any processing begins. In practice, this module acts as the control layer that connects user-defined settings from the GUI or configuration file to the analytical workflow.

### Module 1: Scenario Builder Module

This module reads the foreground dataset and performs all operations related to foreground process preparation. It structures the foreground information, extracts the relevant columns, and prepares the process descriptions, coefficients, keywords, and regional references needed for later stages. In this way, it transforms raw user-provided foreground data into an analysis-ready scenario representation.

### Module 2: Matching and Filtering Module

This module reads the background dataset and performs the matching of foreground entries against candidate background activities or records. It is responsible for search logic, keyword-based comparison, candidate ranking, and iterative filtering so that unsuitable matches can be removed in multiple rounds. Its purpose is to reduce the gap between user-defined foreground descriptions and the structured background data required for impact calculation.

### Module 3: Environmental Impact Calculation Module

This module performs the core environmental burden calculations after the matching stage has been completed. It applies the selected rows, combines foreground coefficients with background environmental values, and generates the structured results workbook. It therefore acts as the computational core of the tool and converts prepared data into interpretable LCIA outputs.

### Module 4: Visualization Module

This module converts the calculated results into charts and other visual outputs that support interpretation and communication. It is responsible for organizing result values into presentation-ready figures that can help users compare categories, scenarios, or process contributions. By separating visualization from the calculation logic, the tool keeps analytical processing and output presentation cleanly decoupled.

### Module 5: SSBD and Scoring Module

This module is reserved for future development and is intended to support SSBD-related logic and scoring functionality. It can be used to extend the current workflow with additional interpretation, ranking, or decision-support layers once the methodology is finalized. Keeping this module defined from the beginning helps preserve a stable architecture for future expansion.

### Module 6: Sensitivity Analysis Module

This module is intended to handle repeated runs, parameter manipulation, and controlled variation of assumptions in order to test the robustness of results. It will make it possible to rerun the workflow under alternative settings and compare how outputs change when key parameters are modified. In methodological terms, it serves as the experimentation layer of the tool.

### Module 7: GUI Module

This module provides the desktop graphical user interface through which users interact with the full workflow. It exposes configuration, matching, calculation, and visualization actions in a guided form so that users do not need to work directly with code. Its role is to make the modular analytical backend accessible, practical, and usable for non-developer users.

## Modular Architecture Overview

The following diagram illustrates the logical flow and interaction between modules in the system. Each module represents a distinct stage in the LCIA workflow, from configuration and data preparation to analysis, visualization, and user interaction.

```
        +-----------------------------+
        | Module 0: Configuration     |
        | (Settings, Paths, Params)   |
        +-------------+---------------+
                      |
                      v
        +-----------------------------+
        | Module 1: Scenario Builder  |
        | (Foreground Preparation)    |
        +-------------+---------------+
                      |
                      v
        +-----------------------------+
        | Module 2: Matching &        |
        | Filtering                  |
        +-------------+---------------+
                      |
                      v
        +-----------------------------+
        | Module 3: Impact            |
        | Calculation                |
        +-------------+---------------+
                      |
                      v
        +-----------------------------+
        | Module 4: Visualization     |
        | (Charts & Outputs)          |
        +-------------+---------------+
                      |
                      v
        +-----------------------------+
        | Module 7: GUI               |
        | (User Interaction Layer)    |
        +-----------------------------+

   -----------------------------------------------
   Future Analytical Extensions
   -----------------------------------------------

        +-----------------------------+
        | Module 5: SSBD & Scoring    |
        +-----------------------------+

        +-----------------------------+
        | Module 6: Sensitivity       |
        | Analysis                    |
        +-----------------------------+
```

This architecture separates configuration, data preparation, matching, calculation, and visualization into clearly defined modules. The modular design ensures that each stage of the workflow can be independently maintained, extended, or replaced without affecting the overall system.

## What This Tool Does

The tool supports three end-user workflows:

1. Full pipeline: matching -> filtering -> row selection -> calculation -> results workbook generation -> chart generation.
2. Matching-only workflow: run matching and iterative filtering, then export filtered data.
3. Calculation-only workflow: load pre-filtered data from `results/`, run calculations, build the results workbook, and generate charts.

## Repository Structure

```text
Agro4Agri/
  GUI_modular.py        # Tk/ttkbootstrap GUI entry point
  main.py               # Orchestrator layer (LCAOrchestrator)
  lca_base.py           # Config loading, template parsing, region handling, utilities
  lca_matching.py       # Matching and iterative filtering logic
  lca_calculation.py    # Emission calculation, result filling, visualization
  config.yaml           # Runtime configuration
  regions.json          # Region mapping/validation input
  requirements.txt      # Python dependencies
  SRC/                  # Input Excel files (foreground/background)
  results/              # Generated outputs
  logs/                 # Runtime logs
```

## Requirements

- Linux, macOS, or Windows
- Python 3.10+
- `tkinter` available in your Python installation
- Excel input files in `.xlsx` format

Dependencies are listed in `requirements.txt`, including:
- `pandas`, `numpy`, `openpyxl`
- `PyYAML`
- `matplotlib`, `Pillow`, `imageio`, `imageio-ffmpeg`
- `ttkbootstrap`

All required third-party Python packages are included in `requirements.txt`.

Note about `tkinter`:
- `tkinter` is part of Python's standard library and is usually bundled with Python.
- It is not a normal third-party dependency managed by `requirements.txt`.
- If `tkinter` is missing, install the OS package (examples below) and then run the GUI again.

Common `tkinter` install commands:
- Ubuntu/Debian: `sudo apt install python3-tk`
- Fedora/RHEL: `sudo dnf install python3-tkinter`
- Arch Linux: `sudo pacman -S tk`
- macOS: install the official Python from `python.org` (it includes Tk support)
- Windows: reinstall/modify Python and ensure `tcl/tk and IDLE` is selected

## Installation and Run (GUI)

Use this tool through the GUI only.

### 1. Create and activate a virtual environment

Linux/macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Windows (PowerShell):

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 2. Run the GUI

From the project root:

```bash
python GUI_modular.py
```

### 3. First use in the GUI

1. Open the `Configuration` tab.
2. Set `Background file`, `Foreground file`, and `Output dir`.
3. Hover over the `(i)` helper marker for mandatory/optional guidance.
4. Click `Apply to config.yaml`.
5. Run one of the workflows: `Full Pipeline`, `Matching Workflow`, or `Calculation Workflow`.

No manual editing of `config.yaml` is required.

The GUI window includes tabs for:
- `Configuration`
- `Full Pipeline`
- `Matching Workflow`
- `Calculation Workflow`

## GUI Behavior and Responsiveness

The GUI supports manual adjustment for dense screens and special usage cases:

- Window is resizable.
- Main content area is vertically split from logs with a draggable divider.
- Each tab content area is scrollable with mouse wheel support.
- A compact status panel at the bottom shows concise execution logs (scrollable).
- Configuration section headers include a hover `(i)` helper marker with mandatory/optional guidance.

## Configuration (`config.yaml`)

The GUI writes runtime settings into `config.yaml` using the Configuration tab.
The file is resolved dynamically next to `main.py`, so moving the project folder to any location still works.

Current key fields:

| Key | Purpose |
|---|---|
| `background_file` | Background Excel file path |
| `foreground_file` | Foreground Excel file path |
| `output_dir` | Output folder for generated artifacts |
| `region_file` | Region lookup JSON file |
| `foreground_columns` | Column names used in the foreground workbook |
| `background_value_mode` | `PEF` or `SSP_RCP` |
| `background_columns` | Column names used in background workbook |
| `ssp_rcp` | Scenario columns used in SSP/RCP mode |

Notes:
- Paths can be absolute or relative to project root.
- `output_dir` is auto-created if missing.
- Region validation is skipped if `region_file` is missing or invalid.

## Input Data Expectations

### Background file (`background_file`)

Expected columns include:
- The first worksheet of the background workbook is used automatically.
- `Name`
- `Informations`
- `Location`
- `Year`
- `Environmental Categories`
- Value column(s):
  - `PEF` when `background_value_mode: PEF`
  - scenario columns listed in `ssp_rcp` when `background_value_mode: SSP_RCP`

### Foreground file (`foreground_file`)

Expected columns are configurable via `foreground_columns`, typically:
- `Item/Unit process`
- `KEYWORDS`
- `Region/Country`
- `Coefficient`

## Workflow Guide

### A. Full Pipeline

1. Open `Configuration` tab and verify paths/columns.
2. Apply settings to `config.yaml`.
3. Open `Full Pipeline` tab.
4. Run steps in order:
   - Initialize
   - Run Matching
   - Filtering Rounds (iterative)
   - Select Rows for Calculation
  - Run Calculation & Build Results
   - Draw Charts from Results

### B. Matching Workflow

Use this when you only need candidate matching and filtering exports.

1. Initialize.
2. Run Matching.
3. Apply one or more filtering rounds.
4. View summary.
5. Finish and export filtered data.

### C. Calculation Workflow

Use this when `results/` already contains filtered or matched files.

1. Initialize.
2. Select rows.
3. Run calculation.
4. Build results workbook.
5. Draw charts.

## Generated Outputs

Primary generated files are placed in `results/`.

Common outputs:
- `matched_items_list.xlsx`
- `filtered_round_<n>.xlsx`
- `Results_Filled.xlsx`
- `results/visualizations/...` (charts)

Logs:
- `logs/LCA_run_<timestamp>.log`

## Troubleshooting

### GUI does not appear or appears blank

- Run from project root so relative paths resolve correctly.
- Ensure your Python includes `tkinter`.
- Confirm no stale environment mismatch (`which python`, `pip show ttkbootstrap`).
- Use the project virtual environment when launching (`.venv`).

### Missing Excel/region files

- Verify `background_file`, `foreground_file`, and `region_file` in `config.yaml`.
- Use the GUI Browse buttons in `Configuration` tab.

### No matches found

- Verify foreground process names and keyword quality.
- Ensure background `Informations` and `Name` fields contain expected terms.
- Inspect `logs/` output for parsing/matching details.

### Calculation cannot run

- Ensure selected rows exist from matching/filtering.
- For calculation-only mode, ensure `results/` has `filtered_round_<n>.xlsx` or `matched_items_list.xlsx`.

### Chart generation fails

- Ensure `Results_Filled.xlsx` exists in `output_dir`.
- Check `logs/` for missing columns or mismatched headers.

## Development Notes

- Core logic is split by responsibility:
  - Configuration and parsing: `lca_base.py`
  - Matching/filtering: `lca_matching.py`
  - Calculation/output/visualization: `lca_calculation.py`
  - Orchestration: `main.py`
  - GUI: `GUI_modular.py`

- Logging is centralized through `setup_logging()` in `lca_base.py`.

## Acknowledgements

This project has received funding from the European Union’s Horizon Europe Research and Innovation programme under AGRO4AGRI (Grant Agreement No. 101130890). Funded by the European Union. Methodes and logics expressed are however those of the author(s) only and do not necessarily reflect those of the European Union or the European Health and Digital Executive Agency (HADEA). Neither the European Union nor the granting authority can be held responsible for them.

🔗 Learn more about the project: [https://agro4agri.eu/]

