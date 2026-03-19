基于 OpenTelemetry Collector Contrib 的 Helm Chart 项目模板，目标是：

- 以 **Deployment 模式**在 Kubernetes 部署 Collector；
- 内置 **OTLP In/Out**（接收 OTLP、转发 OTLP）；
- 通过 GitHub Actions 将 chart 以 OCI 形式发布到 **GHCR**。

---

## 1. 项目命名建议

推荐仓库名：`otelcol-contrib-chart`

推荐 GHCR 包名（OCI Chart）：

```text
ghcr.io/<github_org_or_user>/charts/otelcol-contrib
```

推荐首个发布版本：`0.1.0`（对应标签：`chart-v0.1.0`）

---

## 2. 项目结构

```text
.
├── charts/
│   └── otelcol-contrib/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── _helpers.tpl
│           ├── configmap.yaml
│           ├── deployment.yaml
│           ├── service.yaml
│           ├── serviceaccount.yaml
│           └── NOTES.txt
└── .github/
    └── workflows/
        ├── chart-ci.yaml
        └── chart-release.yaml
```

---

## 3. 当前能力（第一版）

### Deployment 模式

`values.yaml` 中通过 `mode: deployment` 控制，当前首版只支持 deployment。

### OTLP In/Out

默认配置：

- Receiver: `otlp`（gRPC: `4317` / HTTP: `4318`）
- Exporter: `otlp`（可配置上游 endpoint）
- Pipelines: `traces` / `metrics` / `logs`

可通过 `values.yaml` 的 `config` 段覆盖 collector 配置。

---

## 4. Pipeline 设计

### CI（`.github/workflows/chart-ci.yaml`）

触发：PR、push 到 `main`。

步骤：

1. 安装 Helm
2. `helm lint charts/otelcol-contrib`
3. `helm template` 渲染校验

### 发布（`.github/workflows/chart-release.yaml`）

触发：

- 手动触发（`workflow_dispatch`）
- 推送 tag：`chart-v*`

步骤：

1. 登录 GHCR（使用 `GITHUB_TOKEN`）
2. `helm package` 生成 `.tgz`
3. `helm push` 到 `oci://ghcr.io/<owner>/charts`

---

## 5. 发布第一个 GHCR Chart

### 步骤 1：确认仓库权限

仓库 Actions 需要 `packages: write`（workflow 已配置）。

### 步骤 2：打 tag 并推送

```bash
git tag chart-v0.1.0
git push origin chart-v0.1.0
```

### 步骤 3：验证 chart

发布成功后可拉取：

```bash
helm pull oci://ghcr.io/<github_org_or_user>/charts/otelcol-contrib --version 0.1.0
```

---

## 6. 本地快速验证

```bash
helm lint charts/otelcol-contrib
helm template otelcol-contrib charts/otelcol-contrib
```

安装示例：

```bash
helm upgrade --install otelcol-contrib charts/otelcol-contrib -n observability --create-namespace
```