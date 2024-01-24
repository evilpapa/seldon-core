# seldon-core-analytics

![Version: 1.14.0](https://img.shields.io/static/v1?label=Version&message=1.14.0&color=informational&style=flat-square)

使用基本的 Grafana 仪表板安装 Prometheus 和 Grafana，并显示 Seldon 为每个部署的推理图所公开的默认 Prometheus 指标。

## 用法

使用本chart，首先要添加 `seldonio` Helm 仓库：

```bash
helm repo add seldonio https://storage.googleapis.com/seldon-charts
helm repo update
```

一旦完成，你就能使用以下命令部署 chart：

```bash
kubectl create namespace seldon-system
helm install seldon-core-analytics seldonio/seldon-core-analytics --namespace seldon-system
```

**主页:** <https://github.com/SeldonIO/seldon-core>

## 源码

* <https://github.com/SeldonIO/seldon-core>
* <https://github.com/SeldonIO/seldon-core/tree/master/helm-charts/seldon-core-analytics>

## 基本要求

| 仓库 | 名称 | 版本 |
|------------|------|---------|
| https://charts.helm.sh/stable | grafana | ~5.1.4 |
| https://charts.helm.sh/stable | prometheus | ~11.4.0 |

## 设置值

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| alertmanager.config.enabled | bool | `false` |  |
| grafana.adminPassword | string | `"password"` |  |
| grafana.adminUser | string | `"admin"` |  |
| grafana.datasources."datasources.yaml".apiVersion | int | `1` |  |
| grafana.datasources."datasources.yaml".datasources[0].access | string | `"proxy"` |  |
| grafana.datasources."datasources.yaml".datasources[0].name | string | `"prometheus"` |  |
| grafana.datasources."datasources.yaml".datasources[0].type | string | `"prometheus"` |  |
| grafana.datasources."datasources.yaml".datasources[0].url | string | `"http://seldon-core-analytics-prometheus-seldon"` |  |
| grafana.enabled | bool | `true` |  |
| grafana.sidecar.dashboards.enabled | bool | `true` |  |
| grafana.sidecar.dashboards.label | string | `"seldon_dashboard"` |  |
| prometheus.enabled | bool | `true` |  |
| prometheus.server.configPath | string | `"/etc/prometheus/conf/prometheus-config.yaml"` |  |
| prometheus.server.extraConfigmapMounts[0].configMap | string | `"prometheus-server-conf"` |  |
| prometheus.server.extraConfigmapMounts[0].mountPath | string | `"/etc/prometheus/conf/"` |  |
| prometheus.server.extraConfigmapMounts[0].name | string | `"prometheus-config-volume"` |  |
| prometheus.server.extraConfigmapMounts[0].readOnly | bool | `true` |  |
| prometheus.server.extraConfigmapMounts[0].subPath | string | `""` |  |
| prometheus.server.extraConfigmapMounts[1].configMap | string | `"prometheus-rules"` |  |
| prometheus.server.extraConfigmapMounts[1].mountPath | string | `"/etc/prometheus-rules"` |  |
| prometheus.server.extraConfigmapMounts[1].name | string | `"prometheus-rules-volume"` |  |
| prometheus.server.extraConfigmapMounts[1].readOnly | bool | `true` |  |
| prometheus.server.extraConfigmapMounts[1].subPath | string | `""` |  |
| prometheus.server.name | string | `"seldon"` |  |
| prometheus.server.persistentVolume.enabled | bool | `false` |  |
| prometheus.server.persistentVolume.existingClaim | string | `"seldon-claim"` |  |
| prometheus.server.persistentVolume.mountPath | string | `"/seldon-data"` |  |
| prometheus.service_type | string | `"ClusterIP"` |  |
| rbac.enabled | bool | `true` |  |
