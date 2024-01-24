# seldon-core-loadtesting

![Version: 0.2.0](https://img.shields.io/static/v1?label=Version&message=0.2.0&color=informational&style=flat-square)

seldon core 的 Loadtesting

## 用法

使用本chart，首先要添加 `seldonio` Helm 仓库：

```bash
helm repo add seldonio https://storage.googleapis.com/seldon-charts
helm repo update
```

一旦完成，可使用如下进行部署 chart：

```bash
kubectl create namespace seldon-system
helm install seldon-core-loadtesting seldonio/seldon-core-loadtesting --namespace seldon-system
```

**主页:** <https://github.com/SeldonIO/seldon-core>

## 源码

* <https://github.com/SeldonIO/seldon-core>
* <https://github.com/SeldonIO/seldon-core/tree/master/helm-charts/seldon-core-loadtesting>

## 设置值

| 键 | 类型 | 默认值 | 描述 |
|-----|------|---------|-------------|
| data.size | int | `2` |  |
| image.release | float | `0.8` |  |
| loadtest.id | int | `1` |  |
| loadtest.sendFeedback | int | `0` |  |
| locust.clients | int | `10` |  |
| locust.hatchRate | int | `1` |  |
| locust.host | string | `"http://seldon-apiserver:8080"` |  |
| locust.maxWait | int | `1100` |  |
| locust.minWait | int | `990` |  |
| locust.script | string | `"predict_rest_locust.py"` |  |
| oauth.enabled | bool | `true` |  |
| oauth.key | string | `"key"` |  |
| oauth.secret | string | `"secret"` |  |
| replicaCount | int | `1` |  |
| rest.pathPrefix | string | `nil` |  |
