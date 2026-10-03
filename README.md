# Learning behaviour analysis: OULAD and Moodle

This project analyzes learner activity in two educational datasets: the Open University Learning Analytics Dataset (OULAD) and a Moodle event log. It combines descriptive and temporal analysis, learner profiling, hypothesis tests, sequential pattern mining, and early-risk modelling.

The notebooks are the source code for the analysis. Dataset files, derived row-level tables, and generated outputs are kept locally and are not included in this repository by default. In particular, the Moodle event log must not be published unless its data owner has explicitly approved public release.

## Research questions

### OULAD — `notebooks/oulad.ipynb`

- **RQ1: Behavioural profiles.** Identify early VLE activity profiles and compare their activity patterns and final outcomes.
- **RQ2: Early-risk prediction.** Use information available by week 8 to identify learners who eventually Fail or Withdraw. The notebook uses a train/test split and reports classification metrics and feature importance.

### Moodle — `notebooks/moodle.ipynb`

- **RQ1: Preparation and quiz workflow.** Compare preparation features with quiz response coverage, answer changes, timing, submission, and prior attempts. Moodle has no quiz marks, so these are workflow associations, not performance predictions.
- **RQ2: Preparation before quizzes.** Test three patterns in logged material access: sparse prior-week core access, interactive/media versus core access, and single-day versus multi-day core access.
- **RQ3: Learner profiles and sequences.** Form numeric activity profiles with K-Means and use sequential pattern mining (SPM) to compare ordered actions within short activity chains.

## Project structure

```text
.
├── README.md
├── requirements.txt
├── data/
│   └── README.md              # where to put the local datasets
├── notebooks/
│   ├── oulad.ipynb
│   └── moodle.ipynb
├── outputs/
│   └── README.md              # generated files and report guidance
└── reports/
    └── README.md              # location for the final report
```

The notebooks create `data/processed/` and `outputs/` subfolders when they run. The old `figures/` and `work/` folders contain legacy or temporary files and are excluded from Git.

## Dataset setup

1. Download the seven OULAD CSV files from the [OULAD Kaggle dataset](https://www.kaggle.com/datasets/anlgrbz/student-demographics-online-education-dataoulad) and place them in `data/raw/oulad/`. Expected names: `assessments.csv`, `courses.csv`, `studentAssessment.csv`, `studentInfo.csv`, `studentRegistration.csv`, `studentVle.csv`, and `vle.csv`.
2. Copy the provided Moodle file to `data/raw/moodle/MOODLE Data.csv`. This project does not distribute that file.
3. Do not commit `data/raw/` or `data/processed/`. The ignore rules protect both original data and derived learner-level tables from accidental upload.

For more detail, see [`data/README.md`](data/README.md).

## Environment and running the notebooks

From Anaconda Prompt or PowerShell, at the repository root:

```powershell
conda create -n learning-behaviour python=3.11
conda activate learning-behaviour
python -m pip install -r requirements.txt
python -m ipykernel install --user --name learning-behaviour --display-name "Python (learning-behaviour)"
jupyter notebook
```

In Jupyter, open `notebooks/oulad.ipynb` or `notebooks/moodle.ipynb`, select the **Python (learning-behaviour)** kernel, then use **Kernel → Restart Kernel and Run All Cells**. Run from the repository root so the notebooks can find `data/`. OULAD can be run independently; Moodle requires the separate Moodle CSV.

The notebooks do not require a GPU. Moodle sequence mining and OULAD's larger VLE processing can take longer than the initial data-loading cells. Allow the notebook to finish and check the final output cells for exported files and results.

## Outputs and report

Generated figures and result tables are saved under `outputs/`; derived datasets are saved under `data/processed/`. These are ignored by Git because they are reproducible, may contain learner-level records, and some current local exports may be from earlier notebook versions. Regenerate them by running the current notebooks before using any output in the report.

Notebook outputs are stored inside each `.ipynb` file and are **not** covered by the output-folder ignore rule. Before publishing, review the Moodle notebook's displayed outputs for learner-level traces or identifying details; clear those outputs from the copy being published if needed. Keep your local analysis copy if you need its current display state.

Place the final cohesive report in `reports/` (suggested filename: `learning_behaviour_report.pdf`). The assignment requires one PDF of no more than 10 pages, including figures, tables, and references, and a public GitHub repository link for the code. Add the repository URL to the report after publishing the repository. Keep the Moodle raw data and learner-level exports private.

## Publishing this project

The local Git repository is initialized on the `main` branch, but no GitHub remote is configured and nothing has been uploaded. After creating an empty GitHub repository and reviewing/clearing any private notebook outputs, check `git status` to confirm only intended files are included, then connect and push:

```powershell
git remote add origin https://github.com/<your-account>/<your-repository>.git
git add .gitignore README.md requirements.txt data/README.md outputs/README.md reports/README.md notebooks/oulad.ipynb notebooks/moodle.ipynb
git status
git commit -m "Prepare learning behaviour analysis project"
git push -u origin main
```

Replace the example remote with the URL of the repository you created. Do not use `git add .` until you have checked the ignore rules and reviewed notebook-embedded outputs.

## Reproducibility notes

- Each notebook documents its own preprocessing, feature construction, analysis, and limitations.
- Moodle material views mean logged access, not confirmed reading; Moodle quiz workflow fields are not marks.
- OULAD early-risk predictors are restricted to information available by the specified early-course cutoff; final outcome and post-cutoff information are not predictors.
- Re-run notebooks from a clean kernel after changing code so displayed notebook outputs match the source.
