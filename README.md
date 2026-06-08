# argus-helm

Helm repo containing this helm chart: https://uninett.github.io/argus-helm/

## Releasing

Releases are created automatically by the [Release Charts](.github/workflows/release.yml) workflow when a commit is pushed to `main`. If a GitHub release for the current chart version already exists, the workflow skips without error.

To release a new version:

1. Bump `version` in [charts/argus/Chart.yaml](charts/argus/Chart.yaml) (follows [semver](https://semver.org/))
2. If you are tracking a new version of Argus, also update `appVersion` in [charts/argus/Chart.yaml](charts/argus/Chart.yaml) and `image.tag` in [charts/argus/values.yaml](charts/argus/values.yaml)
3. Merge to `main` — the release workflow runs automatically

## Development

### Pre-commit

Install [pre-commit](https://pre-commit.com/#install), then run:

```bash
pre-commit install
```

This sets up a git hook that runs `helm lint` before every commit.

### Unit tests

Install the [helm-unittest](https://github.com/helm-unittest/helm-unittest) plugin:

```bash
helm plugin install https://github.com/helm-unittest/helm-unittest --verify=false
```

Run all tests:

```bash
helm unittest charts/argus
```

Run a single test suite:

```bash
helm unittest -f 'tests/deployment_test.yaml' charts/argus
```
