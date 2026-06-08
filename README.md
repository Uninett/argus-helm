# argus-helm

Helm repo containing this helm chart: https://uninett.github.io/argus-helm/

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
