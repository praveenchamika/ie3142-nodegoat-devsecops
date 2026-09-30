# Failed Dependency Security Gate

## Workflow

- Workflow: DevSecOps Security Pipeline
- Job: Dependency Security Gate
- Branch: evidence/security-gate-demo
- Commit:
- Run URL:
- Date:

## Security policy

The dependency scan was configured using:

`npm audit --audit-level=critical`

Replace `critical` with `high` if that was the selected threshold.

## Detected issue

- Affected package:
- Severity:
- Installed version:
- Vulnerable range:
- Advisory:
- Fix availability:

## Blocking result

npm audit returned a non-zero status after detecting a dependency
vulnerability at or above the configured severity threshold. GitHub Actions
therefore marked the dependency-security job and workflow as failed.

## Evidence

- P01-failed-workflow-overview.png
- P02-failed-dependency-security-job.png
- P03-npm-audit-advisory.png
- P04-audit-threshold-configuration.png

