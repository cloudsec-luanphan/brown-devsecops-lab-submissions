# Lab 5.1 — Submission

## Task 1: SAST with Semgrep

### Semgrep severity breakdown
[
  {
    "severity": "ERROR",
    "count": 13
  },
  {
    "severity": "WARNING",
    "count": 14
  }
]


### Top 10 rules by frequency

[
  {
    "rule": "javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection",
    "count": 6
  },
  {
    "rule": "yaml.github-actions.security.run-shell-injection.run-shell-injection",
    "count": 5
  },
  {
    "rule": "javascript.express.security.audit.express-check-directory-listing.express-check-directory-listing",
    "count": 4
  },
  {
    "rule": "javascript.express.security.audit.express-res-sendfile.express-res-sendfile",
    "count": 4
  },
  {
    "rule": "yaml.github-actions.security.github-actions-mutable-action-tag.github-actions-mutable-action-tag",
    "count": 4
  },
  {
    "rule": "javascript.express.security.audit.express-open-redirect.express-open-redirect",
    "count": 1
  },
  {
    "rule": "javascript.jsonwebtoken.security.jwt-hardcode.hardcoded-jwt-secret",
    "count": 1
  },
  {
    "rule": "javascript.lang.security.audit.code-string-concat.code-string-concat",
    "count": 1
  },
  {
    "rule": "yaml.github-actions.security.gha-curl-pipe-shell.gha-curl-pipe-shell",
    "count": 1
  }
]


### Triage shortcut (Lecture 5 slide 8)
Looking at the top 10 — which **one rule** would you fix first if you had time for only one?
Why? (2-3 sentences. Likely answer: the highest-frequency rule that's not a duplicate
of patterns the team already knows about; one fix at the module level closes many findings.)

### False-positive sample
Pick **one** finding you'd suppress as a false positive after review. Quote the file path +
rule + 1-sentence reason. (NOT generic — must reference the specific code.)
