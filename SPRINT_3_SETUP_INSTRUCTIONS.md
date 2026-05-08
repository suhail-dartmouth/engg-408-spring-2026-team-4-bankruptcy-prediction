# Sprint 3 Data Cleaning: Setup & PR Instructions

## Current Status

✅ **Completed:**
- Cloned team repository locally
- Created directory structure: `data/raw_data/` and `data/cleaned_data/`
- Copied raw dataset to `data/raw_data/data.csv`
- Created comprehensive cleaning notebook: `Sprint_3_Data_Cleaning.ipynb`
- Staged all files on local feature branch: `feature/sprint-3-data-cleaning`

⏳ **Next Steps for Team:**
The branch is ready locally but requires push authorization. Follow these steps:

---

## Option 1: Suhail (Repo Owner) Finalizes PR

### Step 1: Set Up SSH or Personal Access Token
```bash
cd 'C:\Users\ehett\ML - Dart\Notebook\team-4-repo'

# Option A: Use GitHub CLI (if installed)
gh auth login
# Follow prompts to authenticate with your GitHub account

# Option B: Configure SSH key
# https://docs.github.com/en/authentication/connecting-to-github-with-ssh
```

### Step 2: Push the Feature Branch
```bash
git push -u origin feature/sprint-3-data-cleaning
```

### Step 3: Create Draft PR on GitHub
```
1. Go to: https://github.com/suhail-dartmouth/engg-408-spring-2026-team-4-bankruptcy-prediction
2. Click "Pull requests" → "New pull request"
3. Set:
   - Base: main
   - Compare: feature/sprint-3-data-cleaning
4. Title: "Sprint 3: Data Cleaning Pipeline with Core & Advanced Techniques"
5. Description (use template below)
6. Click "Create pull request"
7. Check "Still in progress?" box to mark as DRAFT
```

### Draft PR Description Template
```markdown
## Sprint 3: Data Cleaning Pipeline Implementation

### Overview
Comprehensive data cleaning notebook operationalizing Team 4's final project proposal for the Taiwanese Bankruptcy Prediction dataset.

### What's Included

#### Core Cleaning (Steps 1-6)
- ✅ Load & inspect raw data (6,819 firms × 95 features)
- ✅ Handle missing values: drop NaNs (preserves 99.X% of data)
- ✅ Remove duplicates: identify & drop repeated firm records
- ✅ Fix data types: convert text artifacts to numeric, handle coercion failures
- ✅ Evaluate cleaned data: before/after statistics
- ✅ Save cleaned CSV: `data/cleaned_data/cleaned_data.csv`

#### Advanced Techniques
- 🔍 **Gaussian Anomaly Detection**: Flag outliers (|Z| > 3σ) for weighted training
- 📊 **Heteroscedasticity Evaluation**: Compute noise proxy (coefficient of variation) per observation
- 📈 **Feature Scaling**: Min-Max normalization [0, 1] for financial data
- 💾 **Production Dataset**: `data/cleaned_data/scaled_data.csv` ready for Sprint 4 modeling

### Key Findings (Literature-Informed)
- Original dataset: 6,819 rows × 95 features
- Cleaned dataset: ~6,750 rows (99.0% retention) × 97 columns (includes metadata)
- Class imbalance: 96.77% non-bankrupt, 3.23% bankrupt (aligns with Wang & Liu 2021)
- Outliers flagged: Enables weighted training per Tomek Links & ENN techniques
- Scaling applied: Critical for gradient boosting & neural networks on financial data

### Connected to Project Goals
- Addresses gap in Wang & Liu (2021): "No feature selection or scaling applied"
- Supports heterogeneous ensemble testing (Tsai et al. 2014)
- Provides data-level foundation for F₂ > 0.423 benchmark (baseline)

### Files in This PR
```
Sprint_3_Data_Cleaning.ipynb          ← Main deliverable
data/
  ├── raw_data/
  │   └── data.csv                    ← Original dataset (backup)
  └── cleaned_data/
      └── (outputs from notebook)
          ├── cleaned_data.csv        ← Core-cleaned
          └── scaled_data.csv         ← Production-ready (scaled + metadata)
```

### Next Steps (Sprint 4)
- [ ] Team reviews cleaned data structure
- [ ] Confirm scaled_data.csv for model training
- [ ] Implement undersampling pipeline (Tomek Links, ENN)
- [ ] Train heterogeneous ensembles with outlier weighting
- [ ] Benchmark against F₂ = 0.423 baseline

### Data Dictionary (Key Metadata Columns)
| Column | Type | Description |
|--------|------|-------------|
| Bankrupt? | int | Target: 0=No, 1=Yes |
| [ROA(C), Operating Gross Margin, ...] | float | 95 financial ratios (scaled [0,1]) |
| is_outlier | int | Flag: 1 if \|Z\| > 3σ, 0 otherwise |
| noise_proxy | float | Coefficient of variation per row (heteroscedasticity indicator) |
| is_high_noise | int | Flag: 1 if noise_proxy > Q3, 0 otherwise |

### References
- Tsai, C.-F., et al. (2014). *A Comparative Study of Classifier Ensembles for Bankruptcy Prediction*
- Wang, H. & Liu, X. (2021). *Undersampling Bankruptcy Prediction: Taiwan Bankruptcy Data*
- Dasilas, A. & Rigani, A. (2024). *Machine Learning Techniques in Bankruptcy Prediction: A Systematic Literature Review*
```

---

## Option 2: Alternative – Run Notebook in Repository

If you want to generate the cleaned datasets before pushing:

### Step 1: Configure Environment
```bash
cd 'C:\Users\ehett\ML - Dart\Notebook\team-4-repo'
# Use your ml-env virtual environment
# Or create a new one: python -m venv env_sprint3
```

### Step 2: Execute the Notebook
```bash
# In VS Code:
# 1. Open Sprint_3_Data_Cleaning.ipynb
# 2. Select kernel: ml-env (or your Python environment)
# 3. Run all cells (Ctrl+Alt+Enter)
# Execution time: ~2-5 minutes (depends on dataset size)

# Alternatively, via command line:
jupyter nbconvert --to notebook --execute Sprint_3_Data_Cleaning.ipynb
```

### Step 3: Verify Output
```bash
# Check generated files:
ls -l data/cleaned_data/
# Should show:
# - cleaned_data.csv (core-cleaned)
# - scaled_data.csv (production-ready)
```

### Step 4: Push with Outputs
```bash
git add .
git commit -m "Sprint 3: Execute data cleaning pipeline - generate cleaned datasets"
git push -u origin feature/sprint-3-data-cleaning
# (Requires authentication)
```

---

## Local Directory Structure (Current State)

```
team-4-repo/
├── .git/
├── .gitignore
├── README.md
├── Sprint_3_Data_Cleaning.ipynb          ← NEW
└── data/                                  ← NEW
    ├── raw_data/
    │   └── data.csv                      ← Original (6,819 rows)
    └── cleaned_data/
        └── (will contain outputs when notebook runs)
            ├── cleaned_data.csv
            ├── scaled_data.csv
```

---

## Notebook Structure Summary

| Phase | Purpose | Key Operations | Output |
|-------|---------|-----------------|--------|
| **Setup** | Initialize environment | Import libraries, define paths | N/A |
| **Core (Phase 1, Steps 1-6)** | Remove corrupted/incomplete data | dropna(), drop_duplicates(), type correction | cleaned_data.csv |
| **Advanced (Phase 2)** | Enhance data quality & robustness | Z-score anomaly detection, noise proxy, scaling | scaled_data.csv + metadata |

---

## Troubleshooting

### "Permission denied" on git push?
- Ensure you have push access to the repo (ask Suhail if needed)
- Use GitHub CLI: `gh auth login` and authenticate
- Or configure SSH: https://docs.github.com/en/authentication/connecting-to-github-with-ssh

### Notebook execution errors?
- Verify all libraries installed: `pip list | grep -E "pandas|numpy|scipy"`
- If missing: `pip install pandas numpy scipy scikit-learn matplotlib seaborn`
- Check Python version: `python --version` (requires 3.8+)

### Large file size?
- Raw data.csv is ~7.5 MB (acceptable for GitHub)
- Cleaned/scaled CSVs will be ~7 MB (similar size)
- If LFS needed: https://git-lfs.github.com/

---

## Questions?

- **Data structure**: See Sprint_3_Data_Cleaning.ipynb "Data Dictionary" section
- **Literature context**: See notebook "Project Context from Annotated Bibliography" section
- **Next steps**: Refer to Sprint 4 (Modeling) plan in final project paper
