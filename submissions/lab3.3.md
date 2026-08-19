# Lab 3.3 — Submission

## Task: Gitleaks CI Scan

### Workflow file
Paste the full content of `.github/workflows/lab3-gitleaks-scan.yml`:

### Successful workflow run
- Direct link to a **green (Success)** workflow run: https://github.com/cloudsec-luanphan/brown-devsecops-lab-submissions/actions/runs/32242127393

### Job step explanation
Explain the purpose of each part of the `gitleaks` job (2-3 sentences each):

#### Triggers (`on:`)
What events start this workflow, and why scan on both `push` and `pull_request`?

#### Job: `gitleaks` / `runs-on: ubuntu-latest`
What is this job, and why does it run on a GitHub-hosted Ubuntu runner?

#### Step: Checkout repository
What does `actions/checkout@v4` do? Why is `fetch-depth: 0` important for gitleaks?

#### Step: Run Gitleaks
What does `gitleaks/gitleaks-action@v2` do? What is `GITHUB_TOKEN` used for?

### One-paragraph reflection (2-3 sentences)
Why is CI scanning still necessary if every developer already has a gitleaks pre-commit hook?
