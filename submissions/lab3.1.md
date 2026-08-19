lab3 signing test - Luan


┌──(kali㉿kali)-[~/Desktop/OCSO_Lab/brown-devsecops-lab-submissions]
└─$ git commit -m "test: should be blocked by gitleaks"

Detect hardcoded secrets.................................................Failed
- hook id: gitleaks
- exit code: 1

○
    │╲
    │ ○
    ○ ░
    ░    gitleaks

Finding:     GH_PAT=REDACTED
Secret:      REDACTED
RuleID:      github-pat
Entropy:     4.143943
File:        submissions/leak-attempt.txt
Line:        2
Fingerprint: submissions/leak-attempt.txt:github-pat:2

6:12AM INF 0 commits scanned.
6:12AM INF scanned ~358 bytes (358 bytes) in 32.1ms
6:12AM WRN leaks found: 1

detect private key.......................................................Passed
check for added large files..............................................Passed

 
