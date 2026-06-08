# argus-helm

Helm repo containing this helm chart: https://uninett.github.io/argus-helm/

## Installation

Add the Helm repo and install the chart:

```bash
helm repo add argus https://uninett.github.io/argus-helm/
helm repo update
helm install argus argus/argus --values <your-values.yaml>
```

See [charts/argus/values.yaml](charts/argus/values.yaml) for all available configuration options.

## Dependencies

This chart includes [Bitnami's PostgreSQL chart](https://github.com/bitnami/charts/tree/main/bitnami/postgresql) as a dependency, enabled by default via `postgresql.enabled=true`. When enabled, a PostgreSQL instance is deployed alongside Argus in the same release.

To use an external database instead, set `postgresql.enabled=false`. Read [values.yaml](charts/argus/values.yaml) to see options on how to configure the connection to the external database.

The postgres dependency is bundled with the Argus chart in the official releases, but if you need to have access to the postgres chart locally you can add it with:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### Updating Chart.lock

`Chart.lock` pins the exact dependency versions used in a release. To upgrade the PostgreSQL dependency to a newer version matching the constraint in `Chart.yaml`, run:

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm dependency update charts/argus
```

This rewrites `Chart.lock` with the resolved versions.

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

### Chart tests

[Helm chart tests](https://helm.sh/docs/topics/chart_tests/) are defined in [charts/argus/templates/tests/](charts/argus/templates/tests/) and run against a live cluster after installation. Unlike unit tests, they deploy real pods and verify the running chart behaves correctly.

The `charttest` workflow in [.github/workflows/test.yml](.github/workflows/test.yml) runs these automatically on every PR: it spins up a temporary [kind](https://kind.sigs.k8s.io/) cluster, installs the chart, and runs `helm test` against it.

To run chart tests manually against an existing cluster:

```bash
helm install argus charts/argus --values <your-values.yaml>
helm test argus
```
