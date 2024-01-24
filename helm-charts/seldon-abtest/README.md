# seldon-abtest

![Version: 0.2.0](https://img.shields.io/static/v1?label=Version&message=0.2.0&color=informational&style=flat-square)

在 Seldon Core 中部署 AB 测试的 Chart。允许您在两个模型之间分配流量。

## 用法

使用本chart，首先要添加 `seldonio` Helm 仓库：

```bash
helm repo add seldonio https://storage.googleapis.com/seldon-charts
helm repo update
```

一旦完成，你就能使用以下预估图模板：

```bash
helm template $MY_MODEL_NAME seldonio/seldon-abtest --namespace $MODELS_NAMESPACE
```

注意你也可以直接部署预估图到集群中：
使用：

```bash
helm install $MY_MODEL_NAME seldonio/seldon-abtest --namespace $MODELS_NAMESPACE
```

**Homepage:** <https://github.com/SeldonIO/seldon-core>

## 源码

* <https://github.com/SeldonIO/seldon-core>
* <https://github.com/SeldonIO/seldon-core/tree/master/helm-charts/seldon-abtest>

## 设置值

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| modela.image.name | string | `"seldonio/mock_classifier"` |  |
| modela.image.version | string | `"1.14.0"` |  |
| modela.name | string | `"classifier-1"` |  |
| modelb.image.name | string | `"seldonio/mock_classifier"` |  |
| modelb.image.version | string | `"1.14.0"` |  |
| modelb.name | string | `"classifier-2"` |  |
| predictor.name | string | `"default"` |  |
| replicas | int | `1` |  |
| separate_pods | bool | `true` |  |
| traffic_modela_percentage | float | `0.5` |  |
