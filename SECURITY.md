# Security policy

This policy applies to every repository of the [platform-boutique](https://github.com/platform-boutique) organisation that does not carry its own `SECURITY.md`.

## Reporting a vulnerability

**Please do not open a public issue or pull request for a vulnerability.** Report it privately with GitHub's private vulnerability reporting:

1. Open the repository that contains the problem.
2. Go to the **Security** tab and choose **Report a vulnerability** (direct link: `https://github.com/platform-boutique/<repository>/security/advisories/new`).
3. Describe what you found: the affected repository, file or image, the version or commit, the steps to reproduce it and what an attacker gains. Do not include real secrets or personal data.

Only the maintainers of the repository can read the report. If the **Report a vulnerability** button is not shown, open a public issue that says only "I would like to report a security problem privately" (no details), and a maintainer will enable a private channel.

Do not test against anything that is not yours: this project is a demo, but its container images are public.

## Supported versions

Only the current `main` branch of each repository, and the container images and Helm values that the pipeline publishes from it, receive security fixes. Older commits, old release tags (`vX.Y.Z`, `<app>-vX.Y.Z`) and old image digests are not patched: update to `main`.

## What to expect

The project is maintained by volunteers, so these are best-effort targets, not guarantees:

- an acknowledgement of your report within about 7 days;
- an assessment (accepted, needs more information, or not a vulnerability) within about 14 days;
- a fix on `main` as soon as practical, and a GitHub security advisory for confirmed issues, crediting you if you wish.

Please keep the details private until the fix is on `main` or the maintainers have told you that the report is closed.

## Out of scope

- Demo credentials in `local/compose.yaml` of the development stack: they are throwaway values for a local machine, not shared secrets.
- Findings that only affect a modified copy of the code or a self-hosted deployment with a different configuration.
- Vulnerabilities in third-party dependencies that have no fixed release yet and that Dependabot or the scheduled scans already report; tell us if you can show that the project is exploitable through one.

## For maintainers

Secret scanning and push protection are on for every repository; a leaked secret is rotated, never just removed from the history. Dependency updates arrive as grouped Dependabot pull requests and go through the same `ci / gate` as any other change (see [CONTRIBUTING.md](https://github.com/platform-boutique/.github/blob/main/CONTRIBUTING.md)).
