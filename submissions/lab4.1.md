# Lab 4.1 — Submission

## Task 1: Syft + Grype on Juice Shop

### SBOM stats
- `juice-shop.cdx.json` component count: <jq '.components | length' output>
- `juice-shop.cdx.json` size: <ls output>
- `juice-shop.spdx.json` component count: <jq '.packages | length' output>

### Grype severity breakdown (paste table or JSON)
[
  {
    "severity": "Critical",
    "count": 12
  },
  {
    "severity": "High",
    "count": 80
  },
  {
    "severity": "Low",
    "count": 10
  },
  {
    "severity": "Medium",
    "count": 53
  },
  {
    "severity": "Negligible",
    "count": 7
  },
  {
    "severity": "Unknown",
    "count": 3
  }
]


### Top 10 CVEs (paste from jq output)
[
  {
    "cve": "GHSA-c7hr-j4mj-j2w6",
    "severity": "Critical",
    "package": "jsonwebtoken",
    "version": "0.1.0",
    "fix": "4.2.2"
  },
  {
    "cve": "GHSA-c7hr-j4mj-j2w6",
    "severity": "Critical",
    "package": "jsonwebtoken",
    "version": "0.4.0",
    "fix": "4.2.2"
  },
  {
    "cve": "GHSA-jf85-cpcp-j695",
    "severity": "Critical",
    "package": "lodash",
    "version": "2.4.2",
    "fix": "4.17.12"
  },
  {
    "cve": "GHSA-mp2f-45pm-3cg9",
    "severity": "Critical",
    "package": "decompress",
    "version": "4.2.1",
    "fix": ""
  },
  {
    "cve": "GHSA-xwcq-pm8m-c4vf",
    "severity": "Critical",
    "package": "crypto-js",
    "version": "3.3.0",
    "fix": "4.2.0"
  },
  {
    "cve": "GHSA-23hp-3jrh-7fpw",
    "severity": "Critical",
    "package": "tar",
    "version": "4.4.19",
    "fix": "7.5.19"
  },
  {
    "cve": "GHSA-23hp-3jrh-7fpw",
    "severity": "Critical",
    "package": "tar",
    "version": "6.2.1",
    "fix": "7.5.19"
  },
  {
    "cve": "GHSA-23hp-3jrh-7fpw",
    "severity": "Critical",
    "package": "tar",
    "version": "7.5.15",
    "fix": "7.5.19"
  },
  {
    "cve": "CVE-2026-5450",
    "severity": "Critical",
    "package": "libc6",
    "version": "2.41-12+deb13u2",
    "fix": ""
  },
  {
    "cve": "CVE-2026-34182",
    "severity": "Critical",
    "package": "libssl3t64",
    "version": "3.5.5-1~deb13u2",
    "fix": "3.5.6-1~deb13u2"
  }
]



### Fix-available rate
Out of the top 10 CVEs, how many have a fix available? What does that say about your
patch cadence priorities? (2-3 sentences. Reference Lecture 4's triage shortcut:
*sort by fix-available AND severity ≥ HIGH first*.)
