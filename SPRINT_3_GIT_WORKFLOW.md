# Sprint 3 Git Workflow: Next Steps Card

## Current State ✅
```
Branch: feature/sprint-3-data-cleaning (local only)
Status: Files staged & committed locally
Pending: Push to GitHub + Create Draft PR
```

---

## OPTION A: Push & Create PR (Recommended for Suhail)

### Step 1: Authenticate with GitHub
Choose ONE method:

**Method 1: GitHub CLI (Easiest)**
```powershell
gh auth login
# Follow prompts:
# - Protocol: HTTPS
# - Enter GitHub credentials
# - Authorize "git credential manager" access
```

**Method 2: Personal Access Token**
```powershell
# 1. Generate token: https://github.com/settings/tokens/new
#    - Scope: repo (full control of private repositories)
# 2. When prompted for password, paste the token instead
```

**Method 3: SSH Key**
```powershell
# Setup once: https://docs.github.com/en/authentication/connecting-to-github-with-ssh
# Then use SSH URL for cloning
```

### Step 2: Push Feature Branch
```powershell
cd 'C:\Users\ehett\ML - Dart\Notebook\team-4-repo'
git push -u origin feature/sprint-3-data-cleaning

# Expected output:
# Enumerating objects: 5, done.
# Counting objects: 100% (5, done.
# ...
# remote: Create a pull request for 'feature/sprint-3-data-cleaning' on GitHub by visiting:
# remote:      https://github.com/suhail-dartmouth/engg-408-spring-2026-team-4-bankruptcy-prediction/pull/new/feature/sprint-3-data-cleaning
```

### Step 3: Create Draft PR on GitHub

**Option A: Use GitHub CLI**
```powershell
gh pr create \
  --title "Sprint 3: Data Cleaning Pipeline with Core & Advanced Techniques" \
  --body "See SPRINT_3_COMPLETION_SUMMARY.md for overview" \
  --draft \
  --base main \
  --head feature/sprint-3-data-cleaning
```

**Option B: Manual on GitHub.com**
1. Navigate: https://github.com/suhail-dartmouth/engg-408-spring-2026-team-4-bankruptcy-prediction/pulls
2. Click "New Pull Request"
3. Set:
   - Base: `main`
   - Compare: `feature/sprint-3-data-cleaning`
4. Fill in:
   - **Title**: "Sprint 3: Data Cleaning Pipeline with Core & Advanced Techniques"
   - **Description**: Copy from `SPRINT_3_COMPLETION_SUMMARY.md` or `SPRINT_3_SETUP_INSTRUCTIONS.md`
5. Click "Create pull request"
6. Check "Still in progress?" to mark as DRAFT

---

## OPTION B: Generate Outputs Locally First (If Large Dataset Processing Needed)

### Step 1: Set Up Python Environment
```powershell
# Verify ml-env exists and has required packages
python -m venv venv_sprint3  # Or use existing ml-env

# Activate environment
.\venv_sprint3\Scripts\Activate.ps1

# Install dependencies (if needed)
pip install pandas numpy scipy scikit-learn matplotlib seaborn
```

### Step 2: Run Notebook
```powershell
cd 'C:\Users\ehett\ML - Dart\Notebook\team-4-repo'

# Option A: Via Jupyter (interactive)
jupyter notebook Sprint_3_Data_Cleaning.ipynb
# Click "Cell" → "Run All"

# Option B: Via nbconvert (non-interactive)
jupyter nbconvert --to notebook --execute Sprint_3_Data_Cleaning.ipynb --output-dir=.

# Execution time: 2-5 minutes
```

### Step 3: Verify Outputs
```powershell
Get-ChildItem -Path .\data\cleaned_data\ -File

# Expected:
# Mode                 LastWriteTime         Length Name
# ----                 ----                  ------ ----
# -a---          5/8/2026  1:30 PM        7645283 cleaned_data.csv
# -a---          5/8/2026  1:35 PM        7654021 scaled_data.csv
```

### Step 4: Commit Generated Files
```powershell
git add .\data\cleaned_data\*.csv
git commit -m "Sprint 3: Execute data cleaning pipeline - generate cleaned and scaled datasets

- Cleaned dataset: 6,750 rows × 97 columns (99% retention)
- Scaled dataset: Min-Max normalized [0,1] + metadata columns
- Metadata: is_outlier, noise_proxy, is_high_noise for downstream weighting
- Ready for Sprint 4 modeling pipeline"

# Then push
git push
```

---

## Verification Checklist

After any of the above steps, verify:

### Local Repository
- [ ] `git branch` shows `* feature/sprint-3-data-cleaning`
- [ ] `git log --oneline` shows both commits:
  - `[current] Sprint 3: Execute data cleaning...` (if outputs generated)
  - `[previous] Sprint 3: Add comprehensive...`
- [ ] `ls data/cleaned_data/` lists:
  - `cleaned_data.csv` (or will be generated after notebook runs)
  - `scaled_data.csv` (or will be generated after notebook runs)

### GitHub (After Push)
- [ ] Visit: https://github.com/suhail-dartmouth/engg-408-spring-2026-team-4-bankruptcy-prediction/branches
- [ ] See `feature/sprint-3-data-cleaning` listed
- [ ] Pull Request created (visible in "Pull requests" tab)
- [ ] PR marked as DRAFT

### Data Quality
- [ ] `cleaned_data.csv` has ~6,750 rows
- [ ] `scaled_data.csv` features are in [0, 1] range
- [ ] All 95 financial ratio columns present
- [ ] Metadata columns (is_outlier, noise_proxy, is_high_noise) included

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Permission denied" on push | Use GitHub CLI (`gh auth login`) or configure SSH key |
| Notebook won't run | Verify `ml-env` has pandas, numpy, scipy: `pip list` |
| Output CSVs not generating | Check notebook for errors; run cell-by-cell in Jupyter |
| Can't see pushed branch on GitHub | Refresh browser; check spelling of branch name |
| PR won't merge? | It's DRAFT — finalize in Sprint 4 review, then merge to main |

---

## Timeline

```
NOW (May 8, 2026):
├─ Notebook created ✅
├─ Branch committed locally ✅
└─ [You are here]

THIS WEEK:
├─ Execute notebook (if outputs not generated)
├─ Push to GitHub
├─ Create draft PR
└─ Team reviews structure

NEXT WEEK (Sprint 4):
├─ Finalize PR (run final tests)
├─ Merge feature branch to main
├─ Load scaled_data.csv for modeling
└─ Begin ensemble training
```

---

## Key Files & Docs

| File | Purpose | When to Read |
|------|---------|--------------|
| `Sprint_3_Data_Cleaning.ipynb` | Main deliverable | Before running; review markdown sections |
| `SPRINT_3_COMPLETION_SUMMARY.md` | Overview & findings | Quick understanding of what was built |
| `SPRINT_3_SETUP_INSTRUCTIONS.md` | Detailed setup guide | If stuck on any step |
| `SPRINT_3_GIT_WORKFLOW.md` | This file | Step-by-step git commands |

---

## Copy-Paste Commands (Quick Reference)

```powershell
# Authenticate (do once)
gh auth login

# Push branch
git push -u origin feature/sprint-3-data-cleaning

# Create PR (via CLI)
gh pr create --title "Sprint 3: Data Cleaning Pipeline..." --draft --base main --head feature/sprint-3-data-cleaning

# Check status
git status
git log --oneline -3
git branch -a
```

---

**Last Updated**: May 8, 2026  
**Status**: Ready for team execution  
**Next Owner**: Suhail (GitHub push) → Team (Review) → Erika (Sprint 4 modeling)
