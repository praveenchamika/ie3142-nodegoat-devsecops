# TLS Certificate Configuration
Private TLS keys must not be committed to this repository.
For local HTTPS testing, generate or supply certificate files outside source
control and mount them into the container through an ignored local path.
The current development configuration runs on HTTP localhost and does not
require the removed private key.

For a production deployment:
- Store the private key in a managed secret store.
- Mount the key at runtime.
- Restrict filesystem permissions.
- Never commit the private key to Git.
- Use a certificate issued for the deployed environment.
