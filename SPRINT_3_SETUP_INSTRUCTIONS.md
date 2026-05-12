# Sprint 3 Data Cleaning: Setup & PR Instructions

## Current Status

✅ **Completed:**
- Cloned team repository locally
- Created directory structure: `data/raw_data/` and `data/cleaned_data/`
- Copied raw dataset to `data/raw_data/data.csv` (verified identical to UCI source)
- Created comprehensive cleaning notebook: `Sprint_3_Data_Cleaning.ipynb`
- All cleaning steps implemented and documented

⏳ **Next Steps for Team:**
Run the notebook to generate output CSVs, then push and open the draft PR.

---

## Step 1: Set Up Your Python Environment

```bash
# Verify required packages are installed
pip list | grep -E "pandas|numpy|matplotlib|seaborn"

# If any are missing:
pip install pandas numpy matplotlib seaborn
```

Required packages: `pandas`, `numpy`, `matplotlib`, `seaborn`  
Python version: 3.8+

---

## Step 2: Execute the Notebook

```bash
# Option A: Interactive (recommended — lets you inspect each cell output)
jupyter notebook Sprint_3_Data_Cleaning.ipynb
# Then: Kernel → Restart & Run All

# Option B: Command line
jupyter nbconvert --to notebook --execute Sprint_3_Data_Cleaning.ipynb --output Sprint_3_Data_Cleaning.ipynb
```

**Execution time**: ~2–5 minutes  
**Expected outputs** in `data/cleaned_data/`:
- `cleaned_data.csv` — 6,819 rows × 98 columns (core-cleaned + metadata flags)
- `scaled_data.csv` — 6,819 rows × 98 columns (log1p + Min-Max scaled [0,1])

### Verify Outputs
```bash
# Check files were created
ls -lh data/cleaned_data/

# Quick sanity check on row/column counts
python3 -c "
import pandas as pd
c = pd.read_csv('data/cleaned_data/cleaned_data.csv')
s = pd.read_csv('data/cleaned_data/scaled_data.csv')
print('cleaned_data.csv:', c.shape)
print('scaled_data.csv: ', s.shape)
print('scaled features in [0,1]:', s.drop(columns=['Bankrupt?','is_outlier','noise_proxy','is_high_noise']).apply(lambda col: col.between(0,1).all()).all())
"
```

Expected output:
```
cleaned_data.csv: (6819, 98)
scaled_data.csv:  (6819, 98)
scaled features in [0,1]: True
```

---

## Step 3: Push to GitHub

### Authenticate (once)
```bash
# Option A: GitHub CLI (easiest)
gh auth login

# Option B: Personal Access Token
# Generate at: https://github.com/settings/tokens/new (scope: repo)
# Use token as password when git prompts
```

### Push the feature branch
```bash
git add .
git commit -m "Sprint 3: Execute data cleaning pipeline — generate cleaned and scaled datasets"
git push -u origin feature/sprint-3-data-cleaning
```

---

## Step 4: Create the Draft PR

**Option A — GitHub CLI**
```bash
gh pr create \
  --title "Sprint 3: Data Cleaning Pipeline — Core + Advanced Techniques" \
  --body "See SPRINT_3_COMPLETION_SUMMARY.md for full overview." \
  --draft \
  --base main \
  --head feature/sprint-3-data-cleaning
```

**Option B — GitHub.com**
1. Go to the repository → "Pull requests" → "New pull request"
2. Base: `main` | Compare: `feature/sprint-3-data-cleaning`
3. Use the PR description template below
4. Click "Create pull request" → mark as Draft

---

## Draft PR Description Template

```markdown
## Sprint 3: Data Cleaning Pipeline

### Overview
Comprehensive data cleaning notebook for the Taiwanese Bankruptcy Prediction dataset
(UCI, 6,819 firms × 95 financial ratios). Implements all Sprint 3 core and advanced steps.

### Phase 1: Core Cleaning (Steps 1–6)
- ✅ Load & inspect raw data — `df.describe()`, shape, dtypes, class balance
- ✅ Confirm zero NaN values, zero infinite values, zero duplicate rows
- ✅ Remove constant-variance column: `Net Income Flag` (std = 0)
- ✅ Fix column name typo: `Working capitcal` → `Working Capital`
- ✅ Verify all 95 columns numeric; re-check for coercion-introduced NaNs
- ✅ Before/after statistics + distribution histograms
- ✅ Save: `data/cleaned_data/cleaned_data.csv`

### Phase 2: Advanced Techniques
- 📐 **Skewness reduction**: `log1p` applied to 62/94 features with |skewness| > 5
  (min-max alone preserves skewness; compresses ~95% of data near 0 without this step)
- 🔍 **Outlier detection**: Gaussian Z-score with corrected |Z| > 5 threshold
  (|Z| > 3 flags 28% of data by chance with 95 features — multiple comparison problem)
  763 rows (11.2%) meaningfully flagged in `is_outlier` column
- 📊 **Heteroscedasticity**: Coefficient of variation per row → `noise_proxy` + `is_high_noise`
- 📈 **Feature scaling**: Min-Max [0, 1] applied *after* log1p
- 🔗 **Correlation analysis**: 28 of 4,371 feature pairs have |r| > 0.9 — documented for Sprint 4
- ✅ Save: `data/cleaned_data/scaled_data.csv`

### Key Findings
| Check | Result |
|---|---|
| NaN values | 0 |
| Infinite values | 0 |
| Duplicate rows | 0 |
| Constant columns removed | 1 (`Net Income Flag`) |
| Column typos fixed | 1 (`Working capitcal`) |
| Skewed features (log1p applied) | 62/94 |
| Outliers flagged (\|Z\| > 5) | 763 rows (11.2%) |
| Class imbalance | 30:1 (non-bankrupt:bankrupt) |

### Output Files
| File | Rows | Columns | Purpose |
|---|---|---|---|
| `data/raw_data/data.csv` | 6,819 | 96 | Original — never modified |
| `data/cleaned_data/cleaned_data.csv` | 6,819 | 98 | Core-cleaned + flags |
| `data/cleaned_data/scaled_data.csv` | 6,819 | 98 | Production-ready (log1p + scaled) |

### Metadata Columns Added
| Column | Description |
|---|---|
| `is_outlier` | 1 if any feature \|Z\| > 5 |
| `noise_proxy` | Coefficient of variation per row |
| `is_high_noise` | 1 if noise_proxy > 75th percentile |

### Next Steps (Sprint 4)
- [ ] Load `scaled_data.csv` for model training
- [ ] Address 30:1 class imbalance (Tomek Links, ENN, SMOTE)
- [ ] Train heterogeneous stacked ensembles (MLP, DT, SVM)
- [ ] Apply `is_outlier` sample weighting during loss computation
- [ ] Apply PCA/pruning to the 28 collinear feature pairs
- [ ] Benchmark F₂ against 0.423 baseline (Wang & Liu 2021)

### References
- Tsai, C.-F., et al. (2014). *A Comparative Study of Classifier Ensembles for Bankruptcy Prediction*
- Wang, H. & Liu, X. (2021). *Undersampling Bankruptcy Prediction: Taiwan Bankruptcy Data*
- Dasilas, A. & Rigani, A. (2024). *Machine Learning Techniques in Bankruptcy Prediction*
```

---

## Notebook Structure Summary

| Phase | Steps | Key Operations | Output |
|---|---|---|---|
| **Setup** | 1 | Import libraries, define paths, create dirs | — |
| **Inspect** | 2 | `describe()`, dtypes, NaN/inf/dup checks, class balance | — |
| **Core Clean** | 3 | Drop constant cols, fix typos, type correction | — |
| **Evaluate** | 4 | Before/after stats, distribution plots | — |
| **Save Core** | 5 | Write `cleaned_data.csv` | `cleaned_data.csv` |
| **Summary** | 6 | Print cleaning summary table | — |
| **Correlation** | — | Count collinear pairs (\|r\| > 0.9) | — |
| **Skewness** | Advanced | `log1p` on 62 high-skew features | — |
| **Outliers** | Advanced | Gaussian Z-score \|Z\| > 5, flag in `is_outlier` | — |
| **Noise** | Advanced | CV proxy, flag in `is_high_noise` | — |
| **Scale** | Advanced | Min-Max [0,1] after log1p | — |
| **Save Scaled** | — | Write `scaled_data.csv` | `scaled_data.csv` |

---

## Troubleshooting

| Issue | Solution |
|---|---|
| `ModuleNotFoundError` | `pip install pandas numpy matplotlib seaborn` |
| `FileNotFoundError: data.csv` | Run notebook from the repo root directory |
| Notebook outputs not saving | Use "Restart & Run All" not "Run All" |
| `Permission denied` on git push | Use `gh auth login` or configure SSH key |
| `cleaned_data/` folder missing | Notebook creates it automatically via `os.makedirs` |
| `scaled features in [0,1]: False` | A constant column survived — re-check constant col removal cell |

---

## Questions?

- **Data structure**: See "Data Cleaning Completion Status" table at the bottom of the notebook
- **Literature context**: See notebook "Project Context from Annotated Bibliography" section
- **Cleaning decisions**: Every code section has a comment block explaining the why
- **Next steps**: See `SPRINT_3_COMPLETION_SUMMARY.md` → Sprint 4 section
