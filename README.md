# mulesoft-github-actions-ci-smoke-test

Small Mule 4 application used to exercise the
[mulesoft-github-actions](https://github.com/Mule-GHA-PoC/mulesoft-github-actions) pipeline.

`.github/workflows/pipeline.yml` is the whole pipeline:

- **Pull request to `develop`:** unit tests, package.
- **Pull request from `develop` to `main`:** nothing runs; the checks from the merge to `develop` show on it.
- **Merge to `develop`:** unit tests, package, publish the SNAPSHOT to Exchange, deploy to Sandbox.
- **Merge to `main`:** unit tests, release (publishes `X.Y.Z` to Exchange, tags it, moves both branches
  to the next SNAPSHOT version).
- **Run workflow (manual):** deploy a published version to `qa`, `uat` or `prod`.

Configuration and setup commands are in the framework README.
