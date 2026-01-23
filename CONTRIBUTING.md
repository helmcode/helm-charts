# Contributing Guide

This guide explains how to add new charts and release updates.

## Adding a New Chart

### 1. Create the chart structure

```bash
mkdir -p charts/<chart-name>/templates
```

### 2. Required files

```
charts/<chart-name>/
├── Chart.yaml      # Chart metadata (name, version, description)
├── values.yaml     # Default configuration values
├── README.md       # Chart documentation
├── .helmignore     # Files to exclude from package
└── templates/      # Kubernetes manifest templates
```

### 3. Chart.yaml example

```yaml
apiVersion: v2
name: my-chart
description: Short description of what this chart deploys
type: application
version: 0.1.0
appVersion: "1.0.0"
```

### 4. Validate your chart

```bash
# Lint
helm lint charts/<chart-name>

# Test template rendering
helm template my-release charts/<chart-name>

# Test with custom values
helm template my-release charts/<chart-name> -f custom-values.yaml
```

### 5. Document your chart

Create `charts/<chart-name>/README.md` with:
- Description
- Prerequisites
- Installation command
- Configuration table (parameters, descriptions, defaults)
- Usage examples

### 6. Update root README

Add your chart to the table in `README.md`:

```markdown
| [my-chart](./charts/my-chart) | Short description |
```

### 7. Submit a Pull Request

```bash
git checkout -b add-my-chart
git add charts/<chart-name> README.md
git commit -m "Add my-chart"
git push origin add-my-chart
```

Open a PR against `main`. The CI pipeline will automatically lint and validate your chart.

## Releasing a Chart

### 1. Update the version

Edit `charts/<chart-name>/Chart.yaml`:

```yaml
version: 0.2.0  # Bump this
```

### 2. Commit and push

```bash
git add charts/<chart-name>/Chart.yaml
git commit -m "Bump <chart-name> to 0.2.0"
git push origin main
```

### 3. Create and push the tag

The tag format is `<chart-name>-<version>`:

```bash
git tag <chart-name>-0.2.0
git push origin <chart-name>-0.2.0
```

### 4. Verify the release

The release workflow will:
1. Validate the tag version matches `Chart.yaml`
2. Lint and package the chart
3. Push to GitHub Container Registry

Check the workflow status at: https://github.com/helmcode/helm-charts/actions

Once complete, the chart is available at:
```
oci://ghcr.io/helmcode/helm-charts/<chart-name>
```

## Versioning Rules

- **Single source of truth**: Version is defined ONLY in `Chart.yaml`
- **Semantic versioning**: Use `MAJOR.MINOR.PATCH`
  - MAJOR: Breaking changes
  - MINOR: New features (backwards compatible)
  - PATCH: Bug fixes
- **Tag must match**: The release pipeline validates that tag version equals `Chart.yaml` version

## Code Standards

- All code, comments, and documentation in English
- Follow [Helm best practices](https://helm.sh/docs/chart_best_practices/)
- Include meaningful defaults in `values.yaml`
- Document all configurable parameters
