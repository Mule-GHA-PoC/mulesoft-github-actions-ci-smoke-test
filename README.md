# mulesoft-github-actions-ci-smoke-test

Minimal Mule 4 application used to test the
[mulesoft-github-actions](https://github.com/nsripriya731/mulesoft-github-actions) CI/CD framework.

The application makes no external calls. A scheduled flow logs one line per minute through a sub-flow,
and an MUnit test checks that the sub-flow leaves the payload unchanged.

## Layout

| Path | Purpose |
| --- | --- |
| `pom.xml` | `mule-application` packaging, `mule-maven-plugin` with the `mule-application` classifier, MUnit, Exchange distribution management |
| `mule-artifact.json` | Mule application descriptor (`minMuleVersion`, Java 17) |
| `src/main/mule/logger-app.xml` | Scheduler flow and logging sub-flow |
| `src/test/munit/logger-app-test-suite.xml` | MUnit test |
| `.github/workflows/ci.yml` | Caller for the reusable framework pipeline |

## Local build

Use Java 17:

```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
mvn -B clean verify
```

## CI behaviour

| Event | Result |
| --- | --- |
| Pull request to `develop` or `main` | validate, test, version, package. No secrets, no publish, no deploy. |
| Push to `develop` | the above, plus Exchange publication of a dev version and deployment to `Sandbox` |
| Push to `main` | validate, test, version, package |
| Tag `vX.Y.Z` | the above, plus an immutable release publication to Exchange. No deploy. |

## Required configuration

Variables (repository, or the `Sandbox` environment):

- `ANYPOINT_ORG_ID`: organization ID; must equal the POM `groupId`
- `ANYPOINT_ENVIRONMENT`: `Sandbox`
- `ANYPOINT_DEPLOYMENT_TARGET`: CloudHub 2.0 target name
- `MULE_RUNTIME_VERSION`: runtime version available to the target
- Optional: `ANYPOINT_BASE_URI`, `ANYPOINT_BUSINESS_GROUP_ID`, `MULE_APPLICATION_NAME`, `MULE_RELEASE_CHANNEL`, `MULE_JAVA_VERSION`, `MULE_VCORES`, `MULE_REPLICAS`, `MULE_DEPLOYMENT_TIMEOUT`, `MULE_MAVEN_PLUGIN_VERSION`

Secrets (`Sandbox` environment or repository): either `ANYPOINT_CLIENT_ID` and `ANYPOINT_CLIENT_SECRET`,
or `ANYPOINT_USERNAME` and `ANYPOINT_PASSWORD`. See the framework's `docs/setup.md`.

Setup commands are in the framework's [docs/setup.md](https://github.com/nsripriya731/mulesoft-github-actions/blob/main/docs/setup.md).
