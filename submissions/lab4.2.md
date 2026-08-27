# Lab 4.2 — Submission

## Task 1: Trivy Comparison

### Side-by-side counts
[
  {
    "severity": "CRITICAL",
    "count": 10
  },
  {
    "severity": "HIGH",
    "count": 61
  },
  {
    "severity": "LOW",
    "count": 29
  },
  {
    "severity": "MEDIUM",
    "count": 57
  }
]

### Why the difference?
Pick **two specific CVEs** that ONE tool found and the other didn't. For each:
1. CVE ID + tool that found it + tool that missed it
2. Why (likely): different CVE database refresh cadence? Different package matching rules? Different fix-version awareness?

(Lecture 4 mentioned that Grype and Trivy use slightly different DBs; this is where you see it.)

### When would you pick each?
2-3 sentences each:
- When does Syft+Grype's **decoupled** model win? (hint: SBOM-as-an-attestation, Lecture 4 + Lab 8)
- When does Trivy's **all-in-one** win? (hint: simpler CI step, broader scope including IaC + secrets + misconfig)

## Task 2: GitHub Actions SBOM + SCA Pipeline

### Workflow file
Paste the full content of `.github/workflows/lab4-sbom-sca.yml`:

### Successful workflow run
- Direct link to a **green (Success)** workflow run: <URL>

### Job step explanation
Explain the purpose of each part of the `sbom-and-sca` job (2-3 sentences each):

#### Triggers (`on:`)
What events start this workflow, and why run SBOM + SCA on both `push` and `pull_request`?

#### Job: `sbom-and-sca` / `runs-on: ubuntu-latest`
What is this job, and why does it run on a GitHub-hosted Ubuntu runner?

#### Step: Pull container image
Why does the workflow pull the image explicitly before Syft runs?

#### Step: Generate CycloneDX SBOM with Syft
What does `anchore/sbom-action@v0` do? What is the output file used for?

#### Step: Scan SBOM with Grype
What does `anchore/scan-action@v7` do with the SBOM? Why is `fail-build: false` set here?

#### Step: Scan image with Trivy
How does this step differ from the Grype step? Why run both in the same pipeline?

#### Step: Upload security reports
What artifact is uploaded, and why use `if: always()`?

### One-paragraph reflection (2-3 sentences)
How does this CI pipeline complement the local Syft + Grype workflow from Lab 4.1?
