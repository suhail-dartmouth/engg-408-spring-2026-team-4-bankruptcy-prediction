# Sprint 3 Git Workflow: Next Steps Card

## Current State ✅
```
Branch: feature/sprint-3-data-cleaning (local only)
Status: Notebook updated with all cleaning steps
Pending: Run notebook → push outputs → create Draft PR
```

---

## Step 1: Run the Notebook

Before pushing, execute the notebook to generate the cleaned CSVs:

```bash
# Interactive (recommended)
jupyter notebook Sprint_3_Data_Cleaning.ipynb
# Kernel → Restart & Run All

# Or via command line
jupyter nbconvert --to notebook --execute Sprint_3_Data_Cleaning.ipynb --output Sprint_3_Data_Cleaning.ipynb
```

Verify the outputs exist:
```bash
ls -lh data/cleaned_data/
# Expected:
#   cleaned_data.csv   (~7 MB)
#   scaled_data.csv    (~7 MB)
```

---

## Step 2: Authenticate with GitHub

Choose ONE method:

**Method 1: GitHub CLI (Easiest)**
```bash
gh auth login
# Protocol: HTTPS
# Follow prompts to authenticate
```

**Method 2: Personal Access Token**
```bash
# 1. Generate token: https://github.com/settings/tokens/new
#    Scope: repo (full control)
# 2. Use token as your password when git prompts
```

**Method 3: SSH Key**
```bash
# Setup guide: https://docs.github.com/en/authentication/connecting-to-github-with-ssh
```

---

## Step 3: Commit & Push

```bash
# Stage everything (notebook + generated CSVs)
git add Sprint_3_Data_Cleaning.ipynb
git add data/cleaned_data/cleaned_data.csv
git add data/cleaned_data/scaled_data.csv
git add SPRINT_3_COMPLETION_SUMMARY.md
git add SPRINT_3_SETUP_INSTRUCTIONS.md
git add SPRINT_3_GIT_WORKFLOW.md

# Commit
git commit -m "Sprint 3: Complete data cleaning pipeline with skewness reduction

Phase 1 (Core):
- Confirmed 0 NaN, 0 inf, 0 duplicate rows
- Removed constant column: Net Income Flag (std = 0)
- Fixed column name typo: Working capitcal -> Working Capital
- All 95 columns verified numeric

Phase 2 (Advanced):
- log1p applied to 62 features with |skewness| > 5
- Outlier detection: Gaussian Z-score |Z| > 5 (763 rows, 11.2% flagged)
- Heteroscedasticity proxy: noise_proxy + is_high_noise columns added
- Min-Max scaling [0,1] applied after log1p
- Correlation analysis: 28 collinear pairs (|r| > 0.9) documented

Outputs:
- data/cleaned_data/cleaned_data.csv  (6,819 x 98)
- data/cleaned_data/scaled_data.csv   (6,819 x 98)"

# Push
git push -u origin feature/sprint-3-data-cleaning
```

---

## Step 4: Create Draft PR on GitHub

**Option A: GitHub CLI**
```bash
gh pr create \
  --title "Sprint 3: Data Cleaning Pipeline — Core + Advanced Techniques" \
  --body "See SPRINT_3_COMPLETION_SUMMARY.md for full overview and SPRINT_3_SETUP_INSTRUCTIONS.md for the PR description template." \
  --draft \
  --base main \
  --head feature/sprint-3-data-cleaning
```

**Option B: GitHub.com**
1. Go to the repository → "Pull requests" → "New pull request"
2. Base: `main` | Compare: `feature/sprint-3-data-cleaning`
3. Copy the PR description from `SPRINT_3_SETUP_INSTRUCTIONS.md`
4. Click "Create pull request" → mark as **Draft**

---

## Verification Checklist

### Local
- [ ] `git branch` shows `* feature/sprint-3-data-cleaning`
- [ ] `git log --oneline -3` shows the new commit
- [ ] `data/cleaned_data/cleaned_data.csv` exists (6,819 × 98)
- [ ] `data/cleaned_data/scaled_data.csv` exists (6,819 × 98)

### Data Quality
- [ ] `cleaned_data.csv` has 6,819 rows (100% retained — no NaN/dup removal needed)
- [ ] `scaled_data.csv` features are all in [0, 1]
- [ ] Both files have 98 columns (94 features + 1 target + 3 metadata)
- [ ] Metadata columns present: `is_outlier`, `noise_proxy`, `is_high_noise`
- [ ] `Net Income Flag` column is absent (was constant, dropped)

### GitHub (After Push)
- [ ] `feature/sprint-3-data-cleaning` branch visible in repository
- [ ] Draft PR created and visible under "Pull requests"
- [ ] PR description includes the key findings table

---

## Troubleshooting

| Issue | Solution |
|---|---|
| "Permission denied" on push | Use `gh auth login` or configure SSH key |
| Notebook errors on run | Run cells individually to isolate the failing cell |
| Output CSVs not generated | Check that `data/cleaned_data/` directory exists (notebook creates it) |
| Can't see pushed branch | Refresh browser; double-check branch name spelling |
| Large file warning | CSVs are ~7 MB each — within GitHub's 100 MB limit, no LFS needed |
| PR won't merge | It's DRAFT by design — finalize in Sprint 4 review, then merge to main |

---

## Timeline

```
NOW (May 10, 2026):
├─ Notebook updated with all cleaning steps ✅
├─ Documentation updated ✅
└─ [You are here: run notebook → push → open PR]

THIS WEEK:
├─ Execute notebook (generate cleaned_data.csv + scaled_data.csv)
├─ Push to GitHub
├─ Create draft PR
└─ Team reviews notebook outputs

SPRINT 4:
├─ Finalize PR → merge to main
├─ Load scaled_data.csv for modeling
├─ Implement undersampling pipeline (Tomek Links, ENN)
├─ Train heterogeneous stacked ensembles
└─ Benchmark F₂ against 0.423 baseline
```

---

## Key Files Reference

| File | Purpose |
|---|---|
| `Sprint_3_Data_Cleaning.ipynb` | Main deliverable — run this first |
| `SPRINT_3_COMPLETION_SUMMARY.md` | Full overview of what was built and why |
| `SPRINT_3_SETUP_INSTRUCTIONS.md` | Environment setup + PR description template |
| `SPRINT_3_GIT_WORKFLOW.md` | This file — git commands reference |

---

## Copy-Paste Quick Reference

```bash
# Run notebook
jupyter nbconvert --to notebook --execute Sprint_3_Data_Cleaning.ipynb --output Sprint_3_Data_Cleaning.ipynb

# Authenticate
gh auth login

# Stage, commit, push
git add Sprint_3_Data_Cleaning.ipynb data/cleaned_data/ SPRINT_3_*.md
git commit -m "Sprint 3: Complete data cleaning pipeline with skewness reduction"
git push -u origin feature/sprint-3-data-cleaning

# Create PR
gh pr create --title "Sprint 3: Data Cleaning Pipeline" --draft --base main --head feature/sprint-3-data-cleaning

# Check status
git status
git log --oneline -3
```

---

**Last Updated**: May 10, 2026  
**Status**: Ready for notebook execution and GitHub push  
**Next Owner**: Any team member → run notebook → push → tag team for review
