## bo-battery
Code and analysis for low-data Bayesian Optimisation of sodium-ion battery electrolyte formulation

Data-processing, statistical-analysis and Bayesian Optimisation (BO) pipeline for my MSc dissertation, evaluating whether BO is a useful experimental optimisation framework for sodium-ion battery (SIB) hard carbon anode formulation under low-data conditions. Everything runs from a single notebook, bo_pipeline_ICE.ipynb, in the same order as the dissertation's Methods (Section 3) and Results (Section 4).
ICE (Initial Coulombic Efficiency) is the only modelling target. Retention was considered early on but dropped, see the dissertation's Future Plans section for why.

## Environment setup
Everything ran in a dedicated conda environment (ml-gpu in my case, name doesn't matter).
  - conda create -n ml-gpu python=3.11
  - conda activate ml-gpu

Then install the packages below. Most come straight from PyPI, but a few need attention:
  - pip install pandas numpy matplotlib scikit-learn shap scipy statsmodels
  - pip install torch botorch gpytorch
  - pip install baybe==0.15.0
  - pip install NewareNDA
  - pip install jupyterlab

Notes:
torch/botorch/gpytorch: install torch first (CPU build is enough, the GPs here are small). botorch pulls in a compatible gpytorch automatically, but pinning both is safer if versions drift.
baybe: pinned to 0.15.0 because the kernel configuration API (baybe.surrogates.gaussian_process.components) this project doesn't use directly, but the campaign/searchspace API this project does use, changed across versions. Newer BayBE versions may need small adjustments to Section 3.8.
NewareNDA: reads the binary .nda/.ndax Neware exports (longterm protocol cells). Not on conda, only pip.
tkinter: used for the file-selection dialogs in Section 3.2 (select_files). Comes with most Python installs; on Linux it sometimes needs a system package (sudo apt install python3-tk).

## Data
Included in this repo:
  - master_log.csv --> one row per cell, tracker parameters + calculated ICE/n_cycles + warnings. This is what everything downstream (Sections 3.4 onward) is built from.
  - The cell tracker file --> design parameters per cell (binder, solvent, FEC, thickness, whole plot, etc.).
  - low_confidence_cells_log.csv --> cells flagged by the low-cycle-count filter in Section 3.6.
Not included: the raw Neware exports (flat .csv/.xlsx for the rate-test protocol, binary .nda/.ndax for the longterm protocol) and files_inspect.csv. These are only needed to rebuild master_log.csv from scratch (Section 3.2, STEP 1/2), which isn't necessary to run the rest of the notebook. Available on request.
