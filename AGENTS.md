# Sanctuary Distribution working rules

This repository distributes signed Sanctuary catalogs. It is not a build worker or a place for private signing keys.

- Accept publication only from the trusted local Release Center after exact READY commit, tests, runtime, merge/tree, package identity, certificate, downgrade, size, and SHA-256 checks have passed.
- GitHub Actions artifacts from self-hosted runners are unsigned candidates. They are never a trusted catalog input by themselves. Release Center must download them, verify their manifest and SHA-256 again, and perform privileged signing and publication locally.
- Keep `catalog.json` and `catalog.sig` paired in each channel. Sanctuary Hub trusts the verified ECDSA catalog, not a workflow run, job status, or Actions artifact.
- Keep Stable, Beta, and Dev separate. Do not promote a test candidate to Stable merely because a workflow passed.
- Never commit ECDSA private keys, production application signing keys, tokens, user data, or backups here or put them in Actions secrets.
- Preserve exact source commit, release asset URL, SHA-256, size, package identity, certificate, and monotonic version metadata. Fail closed on mismatches or missing evidence.
