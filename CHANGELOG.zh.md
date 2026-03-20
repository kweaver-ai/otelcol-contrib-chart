# 更新日志

本文件记录此项目的所有重要变更。

## 0.1.0 - 2026-03-20

### 新增

- 新增 `opensearchExporter` 配置项，用于配置 OpenSearch exporter 的 endpoint、TLS、Basic Auth、dataset、namespace、索引命名和 mapping 行为。
- 新增可选的 `debugExporter` 注入能力，便于在本地排障时通过 collector 标准输出查看 traces 和 logs。
- 新增 `scripts/create-nodeport-service.sh`，用于在本地开发场景下通过 NodePort 暴露 OTLP gRPC 和 HTTP 接收端口。
- 新增本地开发验证文档，说明如何安装 `telemetrygen`、发送测试 trace 以及在 collector 日志和 OpenSearch 中验证数据写入。

### 变更

- 默认 collector pipeline 从 OTLP 转发调整为将 traces 和 logs 导出到 OpenSearch。
- 默认 collector 镜像仓库调整为 SWR 国内镜像源 `swr.cn-north-4.myhuaweicloud.com/ddn-k8s/ghcr.io/open-telemetry/opentelemetry-collector-releases/opentelemetry-collector-contrib`。
- 默认副本数调整为 `1`。

### 文档

- 更新 README，使其反映当前 chart 结构、发布命令、本地 OpenSearch 开发流程以及当前默认运行配置。
