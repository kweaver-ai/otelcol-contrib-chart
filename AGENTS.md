# Repository Guidelines

## Project Structure & Module Organization

This repository contains a single Helm chart for OpenTelemetry Collector Contrib.

- `charts/otelcol-contrib/Chart.yaml`: chart metadata and versioning
- `charts/otelcol-contrib/values.yaml`: default image, service, and collector config
- `charts/otelcol-contrib/templates/`: Kubernetes manifests rendered by Helm
- `.github/workflows/chart-ci.yaml`: CI checks
- `.github/workflows/chart-release.yaml`: OCI packaging and GHCR release
- `README.md`: usage, release flow, and repository conventions

Keep changes focused. Most feature work should touch `values.yaml`, one or more files in `templates/`, and sometimes `Chart.yaml`.

## Build, Test, and Development Commands

Use Helm for all local validation:

- `helm lint charts/otelcol-contrib`: validate chart structure and schema
- `helm template otelcol-contrib charts/otelcol-contrib`: render manifests locally
- `helm upgrade --install otelcol-contrib charts/otelcol-contrib -n observability --create-namespace`: install for manual verification
- `helm pull oci://ghcr.io/<owner>/charts/otelcol-contrib --version <version>`: verify a published OCI chart

Before opening a PR, run at least `helm lint` and `helm template`.

## Coding Style & Naming Conventions

Write YAML and Helm templates with 2-space indentation. Keep keys ordered logically: metadata, image, runtime settings, then config. Prefer explicit names over abbreviations.

- Template helpers stay in `templates/_helpers.tpl`
- Resource templates use lowercase filenames such as `deployment.yaml`
- Values use camelCase keys already established in `values.yaml`

Do not hardcode release-specific names in templates; use Helm values and helper templates.

## Testing Guidelines

There is no dedicated unit test suite in this repository. Validation is template-based:

- run `helm lint`
- run `helm template`
- inspect rendered image, ports, ConfigMap content, and labels

When changing defaults, verify the rendered Deployment still references the expected image and config.

## Commit & Pull Request Guidelines

Git history uses short Conventional Commit-style subjects, for example `feat: add Helm chart for OpenTelemetry Collector Contrib`. Follow that pattern with prefixes like `feat:`, `fix:`, `docs:`, and `chore:`.

PRs should include:

- a short summary of the chart change
- any default value changes or breaking behavior
- local validation commands you ran
- linked issue or release context when applicable

If a change affects published artifacts, mention the chart version or release tag, such as `chart-v0.1.0`.
