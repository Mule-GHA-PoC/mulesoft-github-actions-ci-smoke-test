# mulesoft-github-actions-ci-smoke-test

Small Mule 4 application used to exercise the
[mulesoft-github-actions](https://github.com/Mule-GHA-PoC/mulesoft-github-actions) pipeline.

`.github/workflows/pipeline.yml` is the whole pipeline:

- **Pull request:** unit tests, package.
- **Merge to `develop`:** unit tests, package, publish `X.Y.Z-dev.<run>` to Exchange, deploy to Sandbox.
- **Merge to `main`:** unit tests, package, publish release `X.Y.Z` to Exchange (bump the POM version first).
- **Run workflow (manual):** deploy a published version to `qa`, `uat` or `prod`.

Configuration and setup commands are in the framework README.
