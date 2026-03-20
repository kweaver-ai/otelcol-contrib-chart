# Changelog

All notable changes to this project will be documented in this file.

## 0.1.0 - 2026-03-20

### Added

- Added `opensearchExporter` values to configure the OpenSearch exporter endpoint, TLS, basic auth, dataset, namespace, index naming, and mapping behavior.
- Added optional `debugExporter` injection so traces and logs can be inspected from collector stdout during local troubleshooting.
- Added `scripts/create-nodeport-service.sh` to expose OTLP gRPC and HTTP receiver ports through a NodePort service for local development.
- Added a local development guide for installing `telemetrygen`, sending test traces, and verifying data in collector logs and OpenSearch.

### Changed

- Changed the default collector pipeline from OTLP forwarding to OpenSearch export for traces and logs.
- Changed the default collector image repository to the SWR mirror `swr.cn-north-4.myhuaweicloud.com/ddn-k8s/ghcr.io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-contrib`.
- Changed the default replica count to `1`.

### Documentation

- Updated the README to reflect the current chart structure, release commands, local OpenSearch development flow, and current runtime defaults.
