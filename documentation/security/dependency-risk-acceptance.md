# Dependency Vulnerability Baseline

## Current result

The NodeGoat dependency scan currently reports:

- Low: 8
- Moderate: 33
- High: 66
- Critical: 38
- Total: 145

## Reason for the baseline

NodeGoat is an intentionally vulnerable legacy training application. Several
dependency findings require major breaking upgrades, while other transitive
dependencies have no compatible fix.

Running npm audit fix --force proposed major upgrades to core packages,
including database drivers, testing tools and other legacy components.
Applying these upgrades immediately could break application functionality and
invalidate the completed vulnerability-remediation evidence.

## Security policy

The dependency gate generates and uploads the complete npm audit report on
every execution.

The current critical-vulnerability count of 38 is recorded as the temporary
legacy baseline. The pipeline fails if a future change increases the number
of critical vulnerabilities above this baseline.

The baseline does not classify the existing vulnerabilities as resolved.
They remain documented residual risks requiring staged dependency
modernisation.

## Compensating controls

- The application is executed only in an isolated local environment.
- The vulnerable version is not exposed to the public internet.
- Semgrep checks source-code vulnerabilities.
- Custom authorization rules protect against IDOR and missing role checks.
- Gitleaks checks for committed secrets.
- Trivy checks critical container operating-system vulnerabilities.
- GitHub Actions preserves dependency scan reports as build artifacts.

## Future remediation

A future iteration should migrate the template engine, database driver,
test framework and outdated transitive dependencies in controlled stages.
Each major upgrade should be followed by regression and security testing.

