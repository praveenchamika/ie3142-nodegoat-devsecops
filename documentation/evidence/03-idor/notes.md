# Vulnerability 3: Insecure Direct Object Reference

## Before remediation

- Date tested:
- Vulnerable commit:
- Logged-in test user:
- Normal allocation URL:
- Manipulated allocation URL:
- Unauthorized object owner:
- Observed result:
- Screenshot references:

## Root cause

The allocation route used the user identifier supplied in the URL through
req.params.userId. The route did not verify that this identifier belonged to
the authenticated session.

## Security impact

An authenticated user could change the URL identifier and retrieve another
user's allocation information. This violated object-level authorization and
could expose private financial information.

## Correction

The route was changed to retrieve the authenticated user's identifier from
req.session.userId instead of trusting req.params.userId.

## After remediation

## SAST evidence

The generic Semgrep auto configuration did not directly identify the IDOR
because object ownership was application-specific. A project-specific Semgrep
rule was therefore created to identify allocation lookups that derive the
target user identifier from req.params.userId.

Before remediation, the custom rule reported the unsafe route-based object
reference. After replacing the route parameter with req.session.userId, the
same rule reported zero findings.

The custom rule supplements, rather than replaces, the two-user manual test.
The successful after-fix test confirmed that changing the URL no longer
exposed another user's allocation information.
