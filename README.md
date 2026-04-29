# Temel-preNsibe17-2204A-
# Atovaquone-GBM pFBA Simulation Platform

## Overview

This project implements a comprehensive **Parsimonious Flux Balance Analysis (pFBA)** platform to study the metabolic effects of Atovaquone on glioblastoma (GBM) and normal brain cell phenotypes. The platform utilizes the **Recon3D human genome-scale metabolic model** to simulate cellular energy metabolism under drug treatment at varying concentrations.

**Key Features:**
- Multi-phenotype metabolic modeling (6 distinct cell types)
- Multi-dose drug simulation (0–50 µM Atovaquone)
- Real-time progress monitoring and quality control
- Comprehensive visualization suite (13 publication-ready figures)
- Detailed result summaries with biological validation

---

## Scientific Background

### Atovaquone
Atovaquone is an antimalarial compound that acts as a **complex III (cytochrome bc1) inhibitor** in the electron transport chain (ETC). It disrupts oxidative phosphorylation (OXPHOS) and forces cells to rely on alternative energy pathways, primarily glycolysis.

### Glioblastoma (GBM)
Glioblastoma is the most aggressive primary brain tumor. Tumor cells exhibit metabolic heterogeneity, with different subpopulations ranging from:
- **OXPHOS-dependent** (oxidative tumors)
- **Warburg phenotype** (highly glycolytic)
- **Intermediate/Moderate** phenotypes

### Metabolic Flexibility & Compensation
Cells respond to ETC inhibition by upregulating glycolysis and lactate production. The degree of metabolic flexibility varies by cell type and phenotype.

### pFBA Methodology
pFBA (Lewis et al., 2010) extends FBA by adding a second optimization layer that minimizes total metabolic flux while maintaining the maximum growth rate. This produces more biologically realistic flux distributions than standard FBA.

---

## Project Structure

```
FBA_Atovaquone_one/
├── README.md                      # This file
├── Recon3D_301.mat               # Human genome-scale metabolic model
├── code/                          # MATLAB source code
│   ├── run_full_experiment.m     # Master execution script
│   ├── init_env.m                # Environment initialization
│   ├── load_model.m              # Load and configure Recon3D model
│   ├── apply_atovaquone.m        # Simulate CIII inhibition effects
│   ├── configure_phenotypes.m    # Define cell phenotype parameters
│   ├── get_dose_calibration.m    # Map doses to CIII inhibition levels
│   ├── run_pfba.m                # Execute pFBA simulations
│   ├── qc_validation.m           # Quality control checks
│   ├── generate_figures.m        # Create publication-ready visualizations
│   └── figures/                  # Legacy figure outputs
├── figures/                       # Final publication figures
│   ├── Fig01_ATP_vs_Dose.fig
│   ├── Fig02_O2_vs_Dose.fig
│   ├── Fig03_Lactate_Absolute.fig
│   ├── Fig03b_Lactate_FoldChange.fig
│   ├── Fig04_ATP_Reduction_Heatmap.fig
│   ├── Fig05a_Lactate_Absolute_Heatmap.fig
│   ├── Fig05b_Lactate_FoldChange_Heatmap.fig
│   ├── Fig06_Mechanism_MultiPanel.fig
│   ├── Fig07-Fig13_Bar_Comparisons.fig  # Dose-specific phenotype comparisons
├── results/                       # Simulation outputs
│   ├── simulation_results_v22.mat # Raw data (MATLAB format)
│   └── summary_v22.txt           # Human-readable summary report
├── github_code/                  # Code archive (text format)
└── LICENSE                        # Project license (if applicable)
```

---

## Installation & Requirements

### System Requirements
- **MATLAB R2019b or later** (tested up to R2024b)
- **COBRA Toolbox 3.0+** (for FBA/pFBA functionality)
- **Minimum 8 GB RAM** (recommended 16 GB for large models)
- **2–5 hours execution time** on standard hardware

### Setup Instructions

#### 1. Install COBRA Toolbox
```matlab
% Download and install from: https://opencobra.github.io/cobratoolbox
% Then initialize:
initCobraToolbox(false)  % false = don't update or check for updates
```

#### 2. Verify Model
Ensure `Recon3D_301.mat` is in the project root directory:
```matlab
load('Recon3D_301.mat');
disp(model);  % Should display model structure
```

#### 3. Verify MATLAB Paths
Ensure all code files are on the MATLAB path:
```matlab
addpath(genpath('code'));
```

---

## Quick Start

### Run the Full Experiment

Execute the master script to run all simulations:

```matlab
>> run_full_experiment
```

This will:
1. ✓ Initialize environment and load dependencies
2. ✓ Load the Recon3D metabolic model
3. ✓ Configure 6 cell phenotypes
4. ✓ Define 6 Atovaquone dose levels
5. ✓ Run 36 pFBA simulations (6 phenotypes × 6 doses)
6. ✓ Perform quality control validation
7. ✓ Generate 13 publication-ready figures
8. ✓ Save results and summary reports

**Expected Output:**
```
████████████████████████████████████████████████████████████████████
██     ATOVAQUONE-GBM pFBA SIMULATION PLATFORM v22              ██
████████████████████████████████████████████████████████████████████

Start: 2025-12-18 14:15:13

═══════════════════════════════════════════════════════════════════
 STEP 1: ENVIRONMENT SETUP
═══════════════════════════════════════════════════════════════════
[Progress bar...]
Completed in 5.23 seconds

... [Additional steps] ...

═══════════════════════════════════════════════════════════════════
 QC VALIDATION SUMMARY
═══════════════════════════════════════════════════════════════════
Feasibility tests:  36/36 ✓ PASSED
Monotonicity tests: ✓ PASSED
Biological consistency: ✓ PASSED

Total execution time: 156.42 minutes
```

### Run Individual Components

If you need to run specific steps:

```matlab
% Initialize environment
init_env();

% Load metabolic model
model = load_model();

% Configure phenotypes
phenotypes = configure_phenotypes();

% Get dose-CIII calibration
dose_info = get_dose_calibration();

% Run single pFBA simulation
result = run_pfba(model, phenotype, dose_level);

% Run quality control
qc_validation(results);

% Generate visualizations
generate_figures(results);
```

---

## Cell Phenotypes

Six distinct metabolic phenotypes are simulated:

| Phenotype | OXPHOS | Glycolysis | Compensation | Description |
|-----------|--------|-----------|--------------|-------------|
| **Healthy Astrocyte** | 1.00 | 0.15 | 1.30 | Normal CNS glial cells; primarily oxidative |
| **Healthy Neuron** | 1.20 | 0.08 | 1.10 | Normal CNS neurons; heavily oxidative |
| **OXPHOS-GBM** | 1.30 | 0.25 | 1.40 | Oxidative tumor phenotype |
| **Moderate GBM** | 0.70 | 0.50 | 1.60 | Intermediate metabolic profile |
| **Warburg GBM** | 0.40 | 0.80 | 1.90 | Highly glycolytic tumor phenotype |
| **Aggressive GBM** | 0.30 | 1.00 | 2.20 | Highly glycolytic, maximally compensatory |

**Parameters:**
- **OXPHOS**: Relative oxidative phosphorylation capacity
- **Glycolysis**: Relative glycolytic capacity  
- **Compensation**: Metabolic flexibility factor (how much glycolysis increases when OXPHOS is inhibited)

---

## Atovaquone Dose Calibration

Atovaquone is modeled as a **Complex III (CIII) inhibitor**. Dose levels are calibrated to literature-reported IC50 values:

| Dose | CIII Activity | Basis | Biological Effect |
|------|---|---|---|
| 0 µM | 100% | Control | No drug; full ETC function |
| 5 µM | 82% | Fiorillo 2016 | OXPHOS suppression begins |
| 10 µM | 60% | Fiorillo 2016 | Significant glycolysis compensation |
| 20 µM | 35% | Takabe/Kapur 2022 | GBM IC50 range; OXPHOS collapse |
| 30 µM | 18% | Sbirkov 2021 | Near-complete ETC shutdown |
| 50 µM | 8% | Saturation | Maximum inhibition |

---

## Key Outputs

### Simulation Results (`results/simulation_results_v22.mat`)

A MATLAB structure containing:

```matlab
results.phenotypes     % Array of 6 phenotype structures
results.doses          % Array of 6 dose levels (µM)
results.atp_production % 6×6 ATP levels (mmol/gDW/hr)
results.lactate_flux   % 6×6 lactate secretion rates
results.o2_consumption % 6×6 oxygen uptake rates
results.atp_loss       % 6×6 ATP reduction (%)
results.lactate_fold   % 6×6 lactate fold-change vs. control
results.qc_metrics     % Quality control validation results
results.timestamp      % Simulation date and time
```

### Summary Report (`results/summary_v22.txt`)

Human-readable summary including:
- Dose calibration table
- Phenotype parameters
- Key findings (most sensitive/resistant phenotypes)
- QC validation results
- Literature references

### Figures

| Figure | Description |
|--------|-------------|
| **Fig01** | ATP production vs. Atovaquone dose (6 phenotypes) |
| **Fig02** | Oxygen consumption vs. dose (ETC dependency) |
| **Fig03** | Lactate production (absolute values) |
| **Fig03b** | Lactate production (fold-change) |
| **Fig04** | ATP reduction heatmap (phenotypes × doses) |
| **Fig05a** | Lactate production heatmap (absolute) |
| **Fig05b** | Lactate production heatmap (fold-change) |
| **Fig06** | Multi-panel mechanism visualization |
| **Fig07–Fig13** | Phenotype comparisons at each dose (0, 5, 10, 20, 30, 50 µM) |

---

## Code Modules

### `run_full_experiment.m` - Master Execution Script
Main entry point. Orchestrates all analysis steps with progress monitoring and timing.

**Key Functions:**
```matlab
run_full_experiment()      % Execute all steps
```

### `init_env.m` - Environment Setup
Initializes MATLAB paths, checks dependencies, and displays system information.

**Outputs:**
- COBRA toolbox availability check
- System specifications
- Available memory

### `load_model.m` - Load Recon3D Model
Loads the human genome-scale metabolic model from `Recon3D_301.mat` and performs basic validation.

**Outputs:**
- `model`: COBRA model structure
- Number of metabolites, reactions, genes

### `apply_atovaquone.m` - Drug Effect Simulation
Modifies model constraints to simulate Complex III inhibition at specified dose levels.

**Function Signature:**
```matlab
model_drug = apply_atovaquone(model, phenotype, dose_level)
```

**Parameters:**
- `model`: Original metabolic model
- `phenotype`: Cell type parameters
- `dose_level`: Atovaquone concentration (µM)

### `configure_phenotypes.m` - Define Cell Types
Sets up 6 distinct metabolic phenotypes with different OXPHOS/glycolysis balances.

**Outputs:**
```matlab
phenotypes(1)  % Healthy Astrocyte
phenotypes(2)  % Healthy Neuron
phenotypes(3)  % OXPHOS-GBM
phenotypes(4)  % Moderate GBM
phenotypes(5)  % Warburg GBM
phenotypes(6)  % Aggressive GBM
```

### `get_dose_calibration.m` - Map Doses to CIII Inhibition
Converts Atovaquone concentrations (µM) to Complex III activity levels based on literature IC50 values.

**Outputs:**
```matlab
dose_info.doses           % [0, 5, 10, 20, 30, 50] µM
dose_info.ciii_activity   % [1.0, 0.82, 0.60, 0.35, 0.18, 0.08]
dose_info.references      % Literature sources
```

### `run_pfba.m` - Execute pFBA Algorithm
Runs parsimonious FBA on the drug-modified model with phenotype-specific constraints.

**Function Signature:**
```matlab
result = run_pfba(model, phenotype, dose_level, verbose)
```

**Outputs:**
```matlab
result.obj_value       % Growth rate (hr^-1)
result.flux            % Flux vector (reactions × 1)
result.atp_production  % ATP synthesis rate (mmol/gDW/hr)
result.lactate_flux    % Lactate secretion rate (mmol/gDW/hr)
result.o2_uptake       % Oxygen consumption rate (mmol/gDW/hr)
result.status          % 'OPTIMAL', 'INFEASIBLE', etc.
```

### `qc_validation.m` - Quality Control
Validates simulation results for biological plausibility and data integrity.

**Checks:**
- Feasibility: All 36 simulations produced valid solutions
- Monotonicity: Metabolic fluxes decrease monotonically with increasing drug dose
- Biological consistency: ATP/O2 ratios within expected ranges
- No negative production (no spontaneous metabolite generation)

### `generate_figures.m` - Visualization
Generates 13 publication-ready figures in MATLAB `.fig` format (editable vector graphics).

**Figures:**
- Line plots: ATP, O2, lactate vs. dose
- Heatmaps: 6×6 phenotype-dose matrices
- Multi-panel comparisons: Phenotype responses at each dose
- Error bars: Uncertainty quantification

---

## Example: Analyzing a Single Simulation

```matlab
% Load results
load('results/simulation_results_v22.mat');

% Extract ATP production at 20 µM for Aggressive GBM
atp_20um_aggressive = results.atp_production(6, 4);  % phenotype 6, dose 20µM

% Calculate ATP loss relative to control
atp_control_aggressive = results.atp_production(6, 1);  % phenotype 6, dose 0µM
atp_loss_pct = (1 - atp_20um_aggressive/atp_control_aggressive) * 100;

fprintf('ATP loss in Aggressive GBM at 20 µM: %.1f%%\n', atp_loss_pct);

% Get lactate fold-change
lactate_fold = results.lactate_fold(6, 4);
fprintf('Lactate fold-change: %.2f×\n', lactate_fold);
```

---

## Interpretation Guide

### What Do These Results Mean?

#### ATP Production
- **High sensitivity to drug**: Cells lose >60% ATP production (e.g., Healthy Astrocytes at 20 µM)
- **Low sensitivity**: Cells lose <50% ATP (e.g., Aggressive GBM at 20 µM)
- **Implication**: Highly glycolytic cells (Aggressive GBM) better tolerate ETC disruption due to metabolic flexibility

#### Lactate Production
- **High lactate fold-change (>2.0×)**: Strong Warburg shift; glycolysis compensates for lost OXPHOS
- **Low lactate fold-change (<1.5×)**: Limited metabolic compensation
- **Implication**: Tumors with high glycolytic capacity show metabolic resilience to OXPHOS inhibitors

#### O2 Consumption
- **Decreases dramatically with CIII inhibition**: Validates Complex III inhibition model
- **Residual O2 uptake at 50 µM**: Other respiratory complexes (I, II, IV) may remain functional

---

## Troubleshooting

### Issue: "Model not found" Error
```
Error: Cannot load Recon3D_301.mat
```
**Solution:** Verify that `Recon3D_301.mat` is in the project root directory.

### Issue: COBRA Toolbox Not Installed
```
Error: Unrecognized function or variable 'optimizeCbModel'
```
**Solution:** Install COBRA Toolbox from https://opencobra.github.io/cobratoolbox

### Issue: Infeasible Simulations
```
Warning: Some simulations returned status 'INFEASIBLE'
```
**Solution:** This may indicate incompatible phenotype parameters or model constraints. Check `qc_validation.m` output for details.

### Issue: Out of Memory
```
Error: Out of memory. Type HELP MEMORY.
```
**Solution:** The Recon3D model is large (~8000 reactions). Close other applications or use a machine with 16+ GB RAM.

---

## Performance & Scalability

### Typical Execution Times
- Model loading: ~5 seconds
- Environment setup: ~10 seconds
- 36 pFBA simulations: ~120–180 minutes (3–5 hours total)
- Figure generation: ~5 minutes
- **Total time: ~3–5 hours** on standard hardware

### Parallelization
The current implementation runs simulations sequentially. To speed up, consider:
```matlab
parpool('local', 4);  % Use 4 CPU cores
parfor i = 1:36
    results(i) = run_pfba(...);
end
```

---

## References

### Key Publications

1. **Lewis, N. E., et al.** (2010). "Omic data from evolved E. coli are consistent with computed optimal growth phenotypes." *Mol Syst Biol*, 6, 390.
   - Introduces pFBA methodology

2. **Brunk, E., et al.** (2018). "Recon3D enables a three-dimensional view of the genomic annotation and metabolic network of *Homo sapiens*." *Nat Biotechnol*, 36(3), 272–281.
   - Describes Recon3D model

3. **Fiorillo, M., et al.** (2016). "Atovaquone reduces O2 consumption, inhibits glycolysis, and prevents lactate acidification of tumors." *Oncotarget*, 7(1), 1157–1170.
   - Atovaquone pharmacodynamics

4. **Takabe, K., et al.** (2018). "GBM cell growth inhibition by atovaquone." *Cancer Biol Ther*, 19(5), 356–365.
   - GBM response to Atovaquone

5. **Sbirkov, Y., et al.** (2021). "Mitochondrial complex III inhibitors suppress glioblastoma." *Cancers*, 13(10), 2423.
   - Dose-response relationships

### Model References

- **Recon3D v3.0**: https://github.com/opencobra/cobratoolbox/tree/develop/data/models
- **COBRA Toolbox Documentation**: https://opencobra.github.io/cobratoolbox
- **Human Metabolic Reaction Database (HMR)**: https://www.metabolicatlas.org

---

## License & Usage

This project implements published methodologies (pFBA, Recon3D) with original phenotype parameterization for GBM-specific applications.

**Please cite this work as:**
```
Author et al. (2025). "Metabolic heterogeneity in glioblastoma: 
An in silico systems pharmacology study of Atovaquone effects." 
[Journal], Volume(Issue), Pages.
```

**Requires:**
- COBRA Toolbox (released under OPENCOBRATOOLS license)
- Recon3D model (publicly available)

---

## Contact & Support

For questions or issues:

1. **Check the troubleshooting section** (above)
2. **Review simulation logs** in `results/summary_v22.txt`
3. **Examine QC output** from `qc_validation.m`
4. **Verify all input files** exist and are readable

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v22 | 2024–2025 | Integrated pFBA, GBM phenotyping, figure generation, QC validation |
| v21 | 2024 | Initial FBA implementation |
| v1 | 2024 | Proof-of-concept |

---

## Acknowledgments

- **COBRA Toolbox Team**: For pFBA algorithm implementation and model curation
- **Recon3D Consortium**: For comprehensive human metabolic model
- **Literature Data**: Parameterization based on peer-reviewed studies

---

**Last Updated:** 2025
**Platform:** MATLAB R2019b–R2024b
**Status:** Fully Functional ✓
