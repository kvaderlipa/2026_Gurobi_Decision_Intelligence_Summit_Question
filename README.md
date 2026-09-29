# Gurobi Formulation Behavior Investigation

This repository contains a small set of industrial car-sequencing MILP models that exhibit an interesting and repeatable difference in Gurobi's behavior.

The purpose of this repository is not to report a solver issue, but rather to better understand why one objective-function formulation consistently performs substantially better than several alternative formulations representing the same production objective.

Across multiple production instances, objective-function variant **1**:

- finds incumbents earlier,
- starts branching earlier,
- spends less time at the root node,
- explores significantly larger branch-and-bound trees,
- and consistently delivers better solution quality within the same time limit.

The effect is visible across all tested production instances and appears surprisingly robust.

From a modelling perspective, all formulations are intended to optimize the same production metric: the total number of color transitions in the final production sequence. While the formulations differ mathematically, they are designed to represent the same operational objective.

Because the observed behavior is consistent and significant, I would be very interested in any observations from the Gurobi team regarding:

- possible formulation properties that may explain the behavior,
- solver mechanisms that appear to favor formulation 1,
- characteristics that make a formulation easier for Gurobi to process,
- and general modelling recommendations for future objective-function design.

---

## Repository Content

### `lp_models`

LP models used in the experiments.

The repository contains:

- 5 industrial test instances,
- 5 objective-function variants for each instance,

for a total of 25 LP models.

Models belonging to the same test instance differ only in the objective-function formulation.

Model naming convention:

```text
YYYYMMDD_AAxBBBB_Y.lp
```

Where:

- `YYYYMMDD` = production day / test instance,
- `AA` = number of vehicle types,
- `BBBB` = sequence length,
- `Y` = objective-function variant (1–5).

Example:

```text
20260219_88x1257_1.lp
```

---

### `results`

Raw Gurobi logs and corresponding solution files.

These logs contain detailed information about:

- presolve reductions,
- root relaxation behavior,
- incumbent discovery,
- branching behavior,
- explored node counts,
- cutting planes,
- bounds,
- MIP gaps,
- and final solution quality.

No post-processing has been applied to the solver logs.

---

### `figure_convergence_obj_1_5.pdf`

Convergence plots comparing all five objective-function variants.

The plots show incumbent objective values over time and provide a direct visual comparison of solver performance.

This file is probably the best starting point for understanding the observed behavior, as it clearly shows how objective-function variant 1 consistently outperforms the alternative formulations across the included test instances.

---

### `MILP_MODEL.pdf`

Description of the industrial car-sequencing MILP model and the five objective-function formulations.

This document provides the mathematical background necessary for understanding the structural differences between the formulations.

---

### `TESTING_RESULTS_OVERVIEW.xlsx`

Summary of the experimental results.

The workbook includes:

- final objective values,
- final MIP gaps,
- incumbent discovery times,
- branching start times,
- root-node processing statistics,
- explored node counts,
- and additional solver-related statistics extracted from the logs.

The purpose of the workbook is to make cross-instance and cross-formulation comparisons easier without requiring manual inspection of all log files.

---

## Computational Environment

All experiments were executed using:

- Gurobi Optimizer 11.0.3
- Windows Server 2016
- Intel Xeon Gold 6240 @ 2.60 GHz
- 8 physical CPU cores
- 24 GB RAM
- Time limit: 10,800 seconds per run

Additional parameter settings can be found directly in the corresponding log files.

---

## Main Observation

The most interesting observation is not simply that formulation 1 achieves slightly better final solutions.

Rather, formulation 1 appears to induce fundamentally different solver behavior.

Compared to the alternative formulations, it repeatedly demonstrates:

- fewer root-node difficulties,
- earlier incumbent discovery,
- earlier branching,
- more effective branch-and-bound exploration,
- and better practical performance within the available solving time.

This behavior can be observed repeatedly across multiple test instances and therefore appears to be related to the formulation itself rather than instance-specific effects.

I would be very interested in understanding which formulation characteristics might be responsible for this effect and what modelling lessons can be learned from it for future MILP development.

---

Thank you for taking the time to review the models and experimental results.

---

## Contact

For any questions regarding the models, experimental setup, data preparation, or interpretation of the results, please feel free to contact:

- Luboš Takác – <ltakac@kia.sk>
- Alternative contact – <lubos.takac@gmail.com>

I would be happy to provide additional information, model details, or supplementary experimental data if needed.