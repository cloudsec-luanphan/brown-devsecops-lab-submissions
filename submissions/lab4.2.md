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
