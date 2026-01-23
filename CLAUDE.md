# CLAUDE.md

This file provides context for Claude Code when working on this repository.

## Project Overview

This is a public Helm charts repository maintained by Helmcode. Charts are published as OCI artifacts to GitHub Container Registry (ghcr.io/helmcode/helm-charts).

## Repository Structure

```
helm-charts/
├── .github/workflows/
│   ├── helm.yml          # CI pipeline (lint, build, publish on main)
│   └── release.yml       # Release pipeline (triggered by version tags)
├── charts/
│   └── <chart-name>/     # Each chart in its own directory
│       ├── Chart.yaml
│       ├── values.yaml
│       ├── README.md     # Chart-specific documentation
│       ├── .helmignore
│       └── templates/
├── .gitignore
├── LICENSE               # Apache 2.0
├── README.md
└── CLAUDE.md
```

## Development Commands

```bash
# Lint a chart
helm lint charts/<chart-name>

# Package a chart
helm package charts/<chart-name>

# Template a chart (dry-run)
helm template my-release charts/<chart-name>

# Validate chart with custom values
helm template my-release charts/<chart-name> -f custom-values.yaml
```

## CI/CD Pipelines

### helm.yml (Continuous Integration)
Runs on every push and PR to main:
1. **lint**: Validates all charts
2. **build**: Packages charts into `.tgz` artifacts
3. **publish**: Pushes to GHCR (only on main branch)

### release.yml (Release)
Triggered by chart-specific tags using format `<chart-name>-<version>`:
- Extracts chart name and version from tag
- Validates chart exists and version matches `Chart.yaml`
- Lints, packages, and publishes only that chart to GHCR

To release a chart:
```bash
# 1. Update version in charts/<chart-name>/Chart.yaml
# 2. Commit and push changes
# 3. Create and push tag
git tag rabbitmq-0.2.0
git push origin rabbitmq-0.2.0
```

**Important**: The tag version must match the version in `Chart.yaml`.

## Adding a New Chart

1. Create a new directory under `charts/`
2. Include at minimum: `Chart.yaml`, `values.yaml`, `templates/`
3. Add `README.md` with chart documentation (prerequisites, configuration, examples)
4. Add `.helmignore` to exclude unnecessary files
5. Update root `README.md` with the new chart in the table

## Versioning

- **Single source of truth**: Each chart's version is defined ONLY in its `Chart.yaml`
- **No hardcoded versions in docs**: READMEs use `helm install` without `--version` flag
- **Tag format**: `<chart-name>-<semver>` (e.g., `rabbitmq-0.2.0`)
- The release pipeline validates that tag version matches `Chart.yaml`

## Code Standards

- All code, comments, and documentation must be in English
- Follow Helm best practices for chart structure
- Use semantic versioning in `Chart.yaml`
- Include meaningful default values in `values.yaml`
