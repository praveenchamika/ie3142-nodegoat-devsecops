# Hardcoded Secret Review

## Search method

The repository was searched for password, secret, API key and token assignment
patterns. Dependency directories, Git metadata, images, JSON scanner output
and generated text evidence were excluded.

## Findings review

### SESSION_SECRET

The application obtains the runtime session secret from the
SESSION_SECRET environment variable. The real value is stored as a GitHub
Actions repository secret and is not committed.

### .env.example

The .env.example file contains only placeholder values and is intentionally
tracked to document the required configuration.

### Seeded NodeGoat accounts

The repository contains predefined demonstration accounts used by the
intentionally vulnerable NodeGoat training application. These credentials
apply only to the isolated local training database and are not real
production credentials.

### GITHUB_TOKEN

Workflow references to secrets.GITHUB_TOKEN use GitHub's automatically
provided workflow token. No manually entered token value is stored in the
repository.

## Conclusion

No genuine production password, session secret, API key or personal access
token was intentionally committed to the repository.

## Historical TLS private key

Gitleaks identified a private-key file at `artifacts/cert/server.key`. The file
was inherited from the training application and was not required by the active
local HTTP configuration.

The private key was removed from the current repository, and `.gitignore` was
updated to prevent private-key and certificate-key files from being committed.
A README was added to explain that TLS private keys must be supplied through
an external secret-management mechanism at runtime.

Because Gitleaks scans Git history, the removed file may remain detectable in
an earlier commit. Any historical exception is restricted to the exact
reviewed fingerprint. New private-key findings remain blocking.
