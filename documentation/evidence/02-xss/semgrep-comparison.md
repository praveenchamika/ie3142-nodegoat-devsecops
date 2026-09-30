# Semgrep Comparison: Stored XSS

## Before remediation

Files scanned:

- server.js
- app/routes/profile.js
- app/views/profile.html

Relevant Semgrep findings:

- Record the relevant finding, rule and location.
- If none were reported, state that no direct XSS finding was produced.

## After remediation

The same files were scanned using the same Semgrep configuration.

Relevant Semgrep findings:

- Record whether the original finding was removed.
- If no finding existed before, state that the scanner result was unchanged.

## Interpretation

Semgrep results were used together with manual browser testing and source-code
review. The absence of a Semgrep finding does not prove the absence of XSS.
The remediation was validated by repeating the exact same harmless test after
enabling output escaping and input validation.


