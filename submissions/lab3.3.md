# Lab 3.3 — Submission

## Task: Gitleaks CI Scan

### Workflow file
Paste the full content of `.github/workflows/lab3-gitleaks-scan.yml`:

### Successful workflow run
- Direct link to a **green (Success)** workflow run: https://github.com/cloudsec-luanphan/brown-devsecops-lab-submissions/actions/runs/32242127393

### Triggers (`on:`)

The workflow runs on both `push` and `pull_request` events. Scanning both ensures secrets are detected when code is pushed and before changes are merged through a pull request.

### Job: `gitleaks` / `runs-on: ubuntu-latest`

The `gitleaks` job scans the repository for exposed secrets. It uses a GitHub-hosted Ubuntu runner because it provides a clean environment for running the security scan.

### Step: Checkout repository

`actions/checkout@v4` downloads the repository code to the GitHub Actions runner. `fetch-depth: 0` fetches the full Git history, which is important for Gitleaks to detect secrets in previous commits.

### Step: Run Gitleaks

`gitleaks/gitleaks-action@v2` runs Gitleaks to scan the repository for exposed secrets. `GITHUB_TOKEN` allows the action to authenticate with GitHub and interact with the repository when required.

### One-paragraph reflection

CI scanning provides an additional security layer because developers may bypass or forget to run pre-commit hooks. It ensures secrets are scanned centrally before code is merged or deployed.
