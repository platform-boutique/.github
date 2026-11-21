# Contributing

Thanks for looking at platform-boutique. The project is an online shop built as the workload of a platform-engineering demo; everything is public and changes are welcome. This file is the default for every repository of the organisation; a repository with its own `CONTRIBUTING.md` wins. Please also read the [code of conduct](https://github.com/platform-boutique/.github/blob/main/CODE_OF_CONDUCT.md). Security problems go through the [security policy](https://github.com/platform-boutique/.github/blob/main/SECURITY.md), not through issues.

## Where things are

The [organisation profile](https://github.com/platform-boutique/.github/blob/main/profile/README.md) lists every repository by category (`app-*`, `platform-demo-*`, `infra-*`) and the naming conventions. The master reference is [platform-demo-docs](https://github.com/platform-boutique/platform-demo-docs); the open work is on the [roadmap board](https://github.com/orgs/platform-boutique/projects/1).

## Workflow

The project is trunk-based: `main` is protected and every change is a pull request.

1. Fork the repository, or create a branch if you have write access. Keep the branch short-lived and name it `feat/<topic>`, `fix/<topic>`, `docs/<topic>` or `chore/<topic>`.
2. Make small commits that follow [Conventional Commits](https://www.conventionalcommits.org/): `feat:`, `fix:`, `docs:`, `test:`, `chore:`, optionally with a scope (`feat(cart): ...`). A new dependency needs a one-line reason in its commit message, a pinned version, and a release that is at least 7 days old.
3. Install the local guard once per clone: `pipx install pre-commit && pre-commit install`. It runs gitleaks on every commit and push and blocks secrets before they leave your machine (GitHub push protection and the CI secret scan are the enforcing layers).
4. Run the tests and linters of the repository (its README says how) and open a pull request against `main`. Fill in the pull request template: what and why, how you tested it (write "not run" for what you did not run) and the checklist.
5. Resolve every review conversation. The branch does not have to be up to date with `main`, and the history stays linear.
6. A maintainer merges. Pull requests are **squash-merged**, and the pull request title becomes the commit message, so write the title as a conventional commit.

Never commit secrets, tokens or real personal data. Demo credentials live only in `local/compose.yaml` of the platform repository.

## How CI works

Pull requests run the shared pipeline from [platform-demo-workflows](https://github.com/platform-boutique/platform-demo-workflows): each repository has a small `ci.yaml` that calls a reusable workflow, so a fix lands once for all of them. Nothing runs in CI on a plain push to a branch.

For a service repository the pull request run starts the guards (change detection, secret scan with gitleaks, workflow lint, docs check) together with lint, build, tests, dependency scan, code scan and an image build and CVE scan. One final job, **`ci / gate`**, waits for all of them and is the **only required status check** on `main`: it is green when every job passed or was skipped for a known reason (for example a documentation-only change skips the code jobs). Pull requests from forks run the same checks but with a read-only token, so the image job does not push and a maintainer merges them with the normal Merge button.

After the merge, the **Publish** run of the same repository builds or reuses the image (one image per source tree), scans, publishes and signs it, and opens the promotion pull request for the `dev` environment in `platform-demo-helm`; `stage` and `prod` need an approval by the owner in that run. You do not need any of this to contribute: it starts after a maintainer merges.

Other repositories (Helm charts, the portal, observability, infrastructure) use the same idea with checks suited to them; the required checks of a repository are listed in its branch protection (Settings, Branches).

More detail, written for the people who change the pipeline:

- [Calling the workflows](https://github.com/platform-boutique/platform-demo-workflows/blob/main/docs/calling.md) (what `ci.yaml` and `publish.yaml` look like and what `gate` checks)
- [Release model](https://github.com/platform-boutique/platform-demo-workflows/blob/main/docs/release-model.md) (trunk, tags, promotion)
- [Changing a workflow](https://github.com/platform-boutique/platform-demo-workflows/blob/main/docs/changing.md)

## Issues and questions

Open an issue in the repository the question is about, and say what you expected, what happened and how to reproduce it. Small fixes (typos, docs) can go straight to a pull request.

## License

By contributing you agree that your contribution is licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0), the license of the project's repositories.
