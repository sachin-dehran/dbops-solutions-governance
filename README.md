# DBOps Solutions Governance
> GitHub-Native · Auto-Versioned · Zero External Tools

**Live Dashboard:** https://sachin-dehran.github.io/dbops-solutions-governance

---

## 🚀 Quick Setup (< 30 minutes)

### Step 1 — Create 3 GitHub Repos

```
sachin-dehran/dbops-solutions-governance   ← This repo (dashboard + registry)
sachin-dehran/dbops_versol1                ← Solution 1 repo
sachin-dehran/dbops_versol2                ← Solution 2 repo
```

### Step 2 — Push Files

```bash
# Governance repo
cd dbops-solutions-governance
git init && git add . && git commit -m "init: DBOps governance system"
git remote add origin https://github.com/sachin-dehran/dbops-solutions-governance.git
git push -u origin main

# Solution repos
cd ../dbops_versol1
git init && git add . && git commit -m "init: solution1 v2026.1.3"
git remote add origin https://github.com/sachin-dehran/dbops_versol1.git
git push -u origin main

cd ../dbops_versol2
git init && git add . && git commit -m "init: solution2 v2026.2.0"
git remote add origin https://github.com/sachin-dehran/dbops_versol2.git
git push -u origin main
```

### Step 3 — Set Branch Protection on main (each solution repo)

GitHub → Settings → Branches → Add rule for `main`:
- ✅ Require a pull request before merging
- ✅ Require approvals: 1
- ✅ Require status checks: `version-check`
- ✅ Restrict who can push: `sachin-dehran` only

### Step 4 — Set Up Secrets

In each solution repo → Settings → Secrets:
- `GOVERNANCE_TOKEN` — Personal Access Token with repo scope (to trigger governance sync)

In governance repo → Settings → Secrets:
- `RELEASE_TOKEN` — Same PAT

### Step 5 — Enable GitHub Pages

Governance repo → Settings → Pages:
- Source: `gh-pages` branch
- Your dashboard will be live at: `https://sachin-dehran.github.io/dbops-solutions-governance`

### Step 6 — Create Project Branches

```bash
# In each solution repo, create branches for each team
git checkout -b project-team1 && git push origin project-team1
git checkout -b project-team2 && git push origin project-team2
git checkout -b project-team3 && git push origin project-team3
git checkout -b project-team4 && git push origin project-team4
git checkout -b project-team5 && git push origin project-team5
```

### Step 7 — Give Teams Branch Access

GitHub → Settings → Collaborators:
- Add team1-lead with Write access (they can only push to project-team1 via branch protection)

---

## How It Works

### For Project Leads (Daily Work)
```
1. Work freely on your project-teamX branch
2. Push any commits — Actions auto-tags your version
3. To request a Master change:
   - Open PR to main
   - Title: "feat: your change" or "fix: your change"
   - Add label: minor or patch
   - Solution Owner reviews and merges
```

### For Solution Owner (Minimal Effort)
```
1. Review PRs to main (email notification from GitHub)
2. Approve if valid — Actions handle the rest automatically
3. Check dashboard quarterly: sachin-dehran.github.io/dbops-solutions-governance
4. Fill GitHub Project board items each quarter
```

### Version Format
```
Master versions:  v2026.1.3   (vYEAR.MINOR.PATCH)
Branch versions:  team2-v2026.1.3-local.5
```

### Version Bump Rules
| Commit Prefix | PR Label | Result         |
|---------------|----------|----------------|
| fix:          | patch    | v2026.1.3 → v2026.1.4 |
| feat:         | minor    | v2026.1.3 → v2026.2.0 |
| breaking:     | major    | v2026.1.3 → v2027.0.0 |

---

## Repo Structure

```
dbops-solutions-governance/
├── registry.json              ← Auto-updated by Actions (source of truth)
├── docs/
│   └── index.html             ← Live dashboard (GitHub Pages)
├── audit-reports/             ← Auto-generated quarterly
└── .github/
    ├── workflows/
    │   ├── 4-sync-registry.yml      ← Updates registry when solution repos push
    │   └── 5-quarterly-audit.yml    ← Scheduled quarterly automation
    └── ISSUE_TEMPLATE/

dbops_versol1/  (and dbops_versol2/)
├── VERSION                    ← Auto-maintained: "v2026.1.3" or "team2-v2026.1.3-local.5"
├── CHANGELOG.md               ← Auto-updated on every Master release
├── solution/                  ← Actual solution files
├── .dbops/
│   ├── master.json            ← Master release metadata (auto-updated)
│   └── config.json            ← Branch config (auto-updated, set deviation manually)
└── .github/
    ├── CODEOWNERS             ← Enforces Solution Owner on main
    └── workflows/
        ├── 1-branch-auto-tag.yml      ← Auto-tags every branch push
        ├── 2-pr-version-check.yml     ← Validates PR before merge
        ├── 3-master-release.yml       ← Releases new Master on merge
        └── 4-notify-governance.yml    ← Syncs to governance repo
```

---

## Dashboard Features
- **Live** — reads registry.json from GitHub, refreshes every 5 min
- **Overview** — health score, quarterly trend, action items
- **Adoption Matrix** — all teams × all solutions, colour-coded
- **Master Compare** — row-by-row version comparison with branch links
- **Changelog** — auto-generated release history per solution
- **Workflows** — visual version flow + all 5 workflow descriptions
- **? buttons** — plain-English explanation on every card

---

*Built with GitHub Actions + GitHub Pages. Zero external tools.*
