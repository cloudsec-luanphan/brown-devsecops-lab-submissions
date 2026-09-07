# Lab 5.2 — Submission

## Task 1: DAST with OWASP ZAP

### Baseline (unauthenticated) scan
Unauthenticated Scan:
  Total alerts: 10
  High: 0
  Medium: 2
  Low: 5
  Info: 3
  Unique URLs with findings: 14
### Authenticated full scan

ZAP Scan Comparison: Authenticated vs Unauthenticated
Generated: Thu Aug 27 02:36:16 AM EDT 2026
Authenticated Scan:
  Total alerts: 13
  High: 2
  Medium: 4
  Low: 3
  Info: 4
  Unique URLs with findings: 21

Saved to: labs/lab5/results/zap-comparison.txt


### The "10–20× more" claim : authenticated DAST finds 10–20× more issues than unauth
- Ratio (auth alerts / baseline alerts): <e.g., 18.5×>
- Pick **two specific alerts** that only the authenticated scan found. For each:
  1. Alert title + severity
  2. Why was it unreachable to the unauthenticated scan? (1 sentence)

## Bonus: SAST/DAST Correlation

### Correlation table

| # | OWASP cat | ZAP alert | ZAP URI | Semgrep rule | Semgrep file:line | Confidence |
|---|-----------|-----------|---------|--------------|--------------------|------------|
| 1 | A03:2021 Injection (CWE-89) | SQL Injection — High (Low) | `GET /rest/products/search?q=%27%28` (param `q`, attack `'(` → HTTP 500) | `javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection` | `routes/search.ts:23` | High (both agree) |
| 2 | A03:2021 Injection (CWE-89) | SQL Injection — High (Low) | `POST /rest/user/login` (param `email`, attack `'` → HTTP 500) | `javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection` | `routes/login.ts:34` | High (both agree) |

Both Semgrep findings are tagged `severity: ERROR`, `confidence: HIGH`, and mapped to `A01:2017/A03:2021/A05:2025 - Injection` / `CWE-89`, matching ZAP's independently-derived `High (Low)` risk rating for the same two endpoints.

### Strongest correlation deep-dive

Both instances are equally severe (High risk, sequelize-injection, CWE-89) and share the same root cause (raw string interpolation into `sequelize.query`), so `/rest/products/search` is used below as the representative deep-dive since it's the more classically exploitable (UNION-based) case.

**Vulnerable code — `routes/search.ts:19-23`:**
```ts
export function searchProducts () {
  return (req: Request, res: Response, next: NextFunction) => {
    let criteria: any = req.query.q === 'undefined' ? '' : req.query.q ?? ''
    criteria = (criteria.length <= 200) ? criteria : criteria.substring(0, 200)
    models.sequelize.query(`SELECT * FROM Products WHERE ((name LIKE '%${criteria}%' OR description LIKE '%${criteria}%') AND deletedAt IS NULL) ORDER BY name`)
      .then(([products]: any) => { ... })
```
The `q` query parameter is truncated to 200 chars but never escaped or parameterized before being spliced directly into the SQL string.

**Working ZAP payload:**
```
GET /rest/products/search?q=%27%28   (decoded: q=' ()
→ HTTP/1.1 500 Internal Server Error
```
The lone `'(` unbalances the quoting/parenthesis in the generated query and crashes the DB layer — enough to prove injection; a real attacker would follow with a UNION-based payload (Juice Shop's own `unionSqlInjectionChallenge` builds on this exact line).

**Fix — parameterized query:**
```ts
models.sequelize.query(
  'SELECT * FROM Products WHERE ((name LIKE :term OR description LIKE :term) AND deletedAt IS NULL) ORDER BY name',
  { replacements: { term: `%${criteria}%` }, type: QueryTypes.SELECT }
)
```
Using Sequelize's `replacements`/bind parameters (or the query-builder API, e.g. `Product.findAll({ where: { ... } })`) makes the driver treat `criteria` strictly as data, never as SQL syntax — the same fix pattern applies to `routes/login.ts:34` (replace the `${req.body.email}` / `${security.hash(...)}` interpolation with `replacements`).

**Why both tools caught it:** the flaw is a textbook tainted-sink pattern — Semgrep's dataflow rule sees user input (`req.query.q` / `req.body.email`) reach a `sequelize.query` template-literal sink with no sanitizer in between, while ZAP independently reaches the same conclusion at runtime by fuzzing the same parameters and observing a SQL-error-triggering HTTP 500 — one tool proves the *code path exists*, the other proves it's *actually reachable and triggerable* over HTTP.

### Reflection

I'd want the SAST finding first: it points straight at the exact file and line (`routes/search.ts:23`) so the fix is unambiguous, whereas the DAST evidence only proves an HTTP 500 was reachable without saying which line caused it. That said, the DAST result is what makes the finding non-negotiable in a PR review — it shows the vulnerability is exploitable through the live, authenticated request path (not just a theoretical taint chain), which is exactly why Lecture 5 calls a SAST+DAST match on the same endpoint the highest-confidence finding type: static analysis gives you the "where and why," dynamic analysis gives you the "yes, it really happens."
