# Seldon Core：超快、行业就绪的机器学习
一个开源平台，用于在 Kubernetes 上大规模部署您的机器学习模型。

![](https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/core-logo-small.png)

## 概述

Seldon core 转换及的机器学习模型（Tensorflow, Pytorch, H2o, 等）或封装语言 (Python, Java, 等）为生产化额 REST/GRPC 微服务。

Seldon 处理扩展到数千个生产机器学习模型，并提供开箱即用的高级机器学习功能，包括高级指标、请求记录、解释器、异常值检测器、A/B 测试、金丝雀等。

* 阅读 [Seldon Core 文档](https://docs.seldon.io/projects/seldon-core/en/latest/)
* 加入 [Slack 社区](https://join.slack.com/t/seldondev/shared_invite/zt-vejg6ttd-ksZiQs3O_HOtPQsen_labg) 来问问题
* 从 [Seldon Core 笔记本示例]开始(https://docs.seldon.io/projects/seldon-core/en/latest/examples/notebooks.html)
* 加入我们每两周一次的 [在线工作组电话会议](https://docs.seldon.io/projects/seldon-core/en/latest/developer/community.html)：[Google 日历](https://calendar.google.com/event?action=TEMPLATE&tmeid=MXBtNzI1cjk0dG9kczhsZTRkcWlmcm1kdjVfMjAyMDA3MDlUMTUwMDAwWiBzZWxkb24uaW9fbTRuMnZtcmZubDI3M3FsczVnYjlwNjVpMHNAZw&tmsrc=seldon.io_m4n2vmrfnl273qls5gb9p65i0s%40group.calendar.google.com&scp=ALL)
* 了解如何 [开始贡献](https://docs.seldon.io/projects/seldon-core/en/latest/developer/contributing.html)
* 查看深入 Seldon Core 组件的 [博客](https://docs.seldon.io/projects/seldon-core/en/latest/tutorials/blogs.html)
* 观看使用 Seldon Core 的[视频和访谈](https://docs.seldon.io/projects/seldon-core/en/latest/tutorials/videos.html)

![](https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/seldon-core-high-level.jpg)

### 高级功能

Seldon Core 的安装量超过 200 万，用于跨组织管理机器学习模型的大规模部署，主要优势包括：

 * 超简单方式使用 [预包装推理服务](https://docs.seldon.io/projects/seldon-core/en/latest/servers/overview.html)封装你的机器学习模型，[自定义服务](https://docs.seldon.io/projects/seldon-core/en/latest/servers/custom.html)，或[封装语言](https://docs.seldon.io/projects/seldon-core/en/latest/wrappers/language_wrappers.html)。
 * 开箱即用的 [Swagger UI](https://docs.seldon.io/projects/seldon-core/en/latest/reference/apis/openapi.html?highlight=swagger)端点测试，[Seldon Python 客户端或 Curl / GRPCurl](https://docs.seldon.io/projects/seldon-core/en/latest/python/python_module.html#seldon-core-python-api-client)。
 * Cloud 无关并且在 [AWS EKS，Azure AKS，Google GKE，Alicloud，Digital Ocean 及 Openshift](https://docs.seldon.io/projects/seldon-core/en/latest/examples/notebooks.html#cloud-specific-examples) 都测试过。
 * 丰富强大的推理图来管理 [predictors, transformers, routers, combiners等](https://docs.seldon.io/projects/seldon-core/en/latest/examples/graph-metadata.html)。
 * 元数据输出以确保每个模型都能追溯 [训练系统，数据及指标](https://docs.seldon.io/projects/seldon-core/en/latest/reference/apis/metadata.html)。
 * 高级自定义 [Prometheus 和 Grafana](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/analytics.html) 指标实现。
 * 可审计的模型输入输出记录 [Elasticsearch 日志集成](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/log_level.html)。
 * 通过 [Jaeger 集成](https://docs.seldon.io/projects/seldon-core/en/latest/graph/distributed-tracing.html) 实现微服务分布式追踪，以了解各微服务点延迟。
 * 通过一致的[安全及更新策略](https://github.com/SeldonIO/seldon-core/blob/master/SECURITY.md)实现安全、可靠、强大的系统。


## 开始

通过我们的预包装服务器和封装语言可以非常容易的在 Seldon Core 发布模型。下面能查看一个 "hello world Iris" 发布示例。你可以在[快速开始文档](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/quickstart.html)查看有关这些工作流程的更多详细信息。

### 安装 Seldon Core

使用 Helm 3 快速安装（你也可以使用 Kustomize）：

```bash
kubectl create namespace seldon-system

helm install seldon-core seldon-core-operator \
    --repo https://storage.googleapis.com/seldon-charts \
    --set usageMetrics.enabled=true \
    --namespace seldon-system \
    --set istio.enabled=true
    # You can set ambassador instead with --set ambassador.enabled=true
```

### 使用预打包模型服务快速部署模型

我们为一些最流行的深度学习和机器学习框架提供优化的模型服务器，允许您部署经过训练的模型二进制文件/权重，而无需容器化或修改它们。

您只需要将模型二进制文件上传到您首选的对象存储中，在这种情况下，我们在 Google 存储桶中有一个经过训练的 scikit-learn iris 模型：

```console
gs://seldon-models/v1.14.0/sklearn/iris/model.joblib
```

创建一个命名空间来运行你的模型：

```bash
kubectl create namespace seldon
```

然后，我们可以使用预打包的 scikit-learn (SKLEARN_SERVER) 模型服务器通过 `kubectl apply` 将这个模型通过 Seldon Core 部署到我们的 Kubernetes 集群。

```yaml
$ kubectl apply -f - << END
apiVersion: machinelearning.seldon.io/v1
kind: SeldonDeployment
metadata:
  name: iris-model
  namespace: seldon
spec:
  name: iris
  predictors:
  - graph:
      implementation: SKLEARN_SERVER
      modelUri: gs://seldon-models/v1.14.0/sklearn/iris
      name: classifier
    name: default
    replicas: 1
END
```

#### 发送 API 请求到部署的模型

部署的每个模型都公开了一个标准化的用户界面，以使用我们的 OpenAPI 模式发送请求。

这可以通过端点访问 `http://<ingress_url>/seldon/<namespace>/<model-name>/api/v1.0/doc/`，它允许您直接通过浏览器发送请求。

![](https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/rest-openapi.jpg)

或者，您可以使用我们的 [Seldon Python Client](https://docs.seldon.io/projects/seldon-core/en/latest/python/seldon_client.html) 或其他 Linux CLI：

```console
$ curl -X POST http://<ingress>/seldon/seldon/iris-model/api/v1.0/predictions \
    -H 'Content-Type: application/json' \
    -d '{ "data": { "ndarray": [[1,2,3,4]] } }'

{
   "meta" : {},
   "data" : {
      "names" : [
         "t:0",
         "t:1",
         "t:2"
      ],
      "ndarray" : [
         [
            0.000698519453116284,
            0.00366803903943576,
            0.995633441507448
         ]
      ]
   }
}
```

### 使用语言封装部署您的自定义模型

对于更多具有自定义依赖项的自定义深度学习和机器学习用例（例如三方库、操作系统二进制文件甚至外部系统），我们可以使用任何 Seldon Core 语言封装器。

您只需要编写一个公开模型逻辑的类包装器；例如在 Python 中，我们可以创建一个文件 `Model.py`：

```python
import pickle
class Model:
    def __init__(self):
        self._model = pickle.loads( open("model.pickle", "rb") )

    def predict(self, X):
        output = self._model(X)
        return output
```

可使用 [Seldon Core s2i 工具](https://docs.seldon.io/projects/seldon-core/en/latest/wrappers/s2i.html) 容器化我们的类文件，来生成 `sklearn_iris` 镜像：

```console
s2i build . seldonio/seldon-core-s2i-python3:0.18 sklearn_iris:0.1
```

现在我们将它部署到我们的 Seldon Core Kubernetes 集群：

```yaml
$ kubectl apply -f - << END
apiVersion: machinelearning.seldon.io/v1
kind: SeldonDeployment
metadata:
  name: iris-model
  namespace: model-namespace
spec:
  name: iris
  predictors:
  - componentSpecs:
    - spec:
        containers:
        - name: classifier
          image: sklearn_iris:0.1
    graph:
      name: classifier
    name: default
    replicas: 1
END
```

#### 向您部署的模型发送 API 请求

部署的每个模型都公开了一个标准化的用户界面，以使用我们的 OpenAPI 模式发送请求。

这可以通过端点访问 `http://<ingress_url>/seldon/<namespace>/<model-name>/api/v1.0/doc/`，它允许您直接通过浏览器发送请求。

![](https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/rest-openapi.jpg)

或者，您可以使用我们的 [Seldon Python Client](https://docs.seldon.io/projects/seldon-core/en/latest/python/seldon_client.html) 或其他 Linux CLI 以编程方式发送请求：

```console
$ curl -X POST http://<ingress>/seldon/model-namespace/iris-model/api/v1.0/predictions \
    -H 'Content-Type: application/json' \
    -d '{ "data": { "ndarray": [1,2,3,4] } }' | json_pp

{
   "meta" : {},
   "data" : {
      "names" : [
         "t:0",
         "t:1",
         "t:2"
      ],
      "ndarray" : [
         [
            0.000698519453116284,
            0.00366803903943576,
            0.995633441507448
         ]
      ]
   }
}
```

### 深入了解高级生产机器学习集成

任何使用 Seldon Core 部署和编排的模型都可以提供开箱即用的机器学习洞察力，用于监控、管理、扩展和调试。

以下是一些核心组件以及指向日志的链接，这些日志提供了有关如何设置它们的进一步见解。

<table>
  <tr valign="top">
    <td width="50%" >
        <a href="https://docs.seldon.io/projects/seldon-core/en/latest/analytics/analytics.html">
            <br>
            <b>prometheus 的标准和自定义指标</b>
            <br>
            <br>
            <img src="https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/analytics/dashboard.png">
        </a>
    </td>
    <td width="50%">
        <a href="https://docs.seldon.io/projects/seldon-core/en/latest/analytics/logging.html">
            <br>
            <b>带有 ELK 请求日志记录的完整审计跟踪</b>
            <br>
            <br>
            <img src="https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/kibana-custom-search.png">
        </a>
    </td>
  </tr>
  <tr valign="top">
    <td width="50%">
        <a href="https://docs.seldon.io/projects/seldon-core/en/latest/analytics/explainers.html">
            <br>
            <b>机器学习可解释性解释器</b>
            <br>
            <br>
            <img src="https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/anchors.jpg">
        </a>
    </td>
    <td width="50%">
        <a href="https://docs.seldon.io/projects/seldon-core/en/latest/analytics/outlier_detection.html">
            <br>
            <b>用于监控的异常值和对抗性检测器</b>
            <br>
            <br>
            <img src="https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/adversarial-attack.png">
        </a>
    </td>
  </tr>
  <tr valign="top">
    <td width="50%">
        <a href="https://docs.seldon.io/projects/seldon-core/en/latest/analytics/cicd-mlops.html">
            <br>
            <b>大规模 MLOps 的 CI/CD</b>
            <br>
            <br>
            <img src="https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/cicd-seldon.jpg">
        </a>
    </td>
    <td width="50%">
        <a href="https://docs.seldon.io/projects/seldon-core/en/latest/graph/distributed-tracing.html">
            <br>
            <b>用于性能监控的分布式跟踪</b>
            <br>
            <br>
            <img src="https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/graph/jaeger-ui-rest-example.png">
        </a>
    </td>
  </tr>
</table>


## 接下来该怎么看

### 开始

* [快速入门指南 ](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/github-readme.html)
* [组件概述 ](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/overview.html)
* [在 Kubernetes 上安装 Seldon Core ](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/install.html)
* [加入社区 ](https://docs.seldon.io/projects/seldon-core/en/latest/developer/community.html)

### 深入 Seldon Core

* [详细安装参数 ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/helm.html)
* [预打包推理服务器 ](https://docs.seldon.io/projects/seldon-core/en/latest/servers/overview.html)
* [自定义模型的语言包装器 ](https://docs.seldon.io/projects/seldon-core/en/latest/wrappers/language_wrappers.html)
* [创建推理图 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/inference-graph.html)
* [部署您的模型 ](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/deploying.html)
* [测试您的模型端点  ](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/serving.html)
* [故障排除指南 ](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/troubleshooting.html)
* [使用情况报告 ](https://docs.seldon.io/projects/seldon-core/en/latest/workflow/usage-reporting.html)
* [升级 ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/upgrading.html)
* [变更日志 ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/changelog.html)

### 预打包推理服务器

* [MLflow 服务器 ](https://docs.seldon.io/projects/seldon-core/en/latest/servers/mlflow.html)
* [SKLearn 服务器 ](https://docs.seldon.io/projects/seldon-core/en/latest/servers/sklearn.html)
* [Tensorflow 服务 ](https://docs.seldon.io/projects/seldon-core/en/latest/servers/tensorflow.html)
* [XGBoost 服务器 ](https://docs.seldon.io/projects/seldon-core/en/latest/servers/xgboost.html)

### 语言封装 (生产)

* [Python 语言封装 [Production] ](https://docs.seldon.io/projects/seldon-core/en/latest/python/index.html)

### 语言封装 (孵化)

* [Java 语言封装 [Incubating] ](https://docs.seldon.io/projects/seldon-core/en/latest/java/README.html)
* [R 语言封装 [ALPHA] ](https://docs.seldon.io/projects/seldon-core/en/latest/R/README.html)
* [NodeJS 语言封装 [ALPHA] ](https://docs.seldon.io/projects/seldon-core/en/latest/nodejs/README.html)
* [Go 语言封装 [ALPHA] ](https://docs.seldon.io/projects/seldon-core/en/latest/go/go_wrapper_link.html)

### 流量入口

* [Ambassador 流量入口 ](https://docs.seldon.io/projects/seldon-core/en/latest/ingress/ambassador.html)
* [Istio 流量入口 ](https://docs.seldon.io/projects/seldon-core/en/latest/ingress/istio.html)

### 生产环境

* [支持的 API 协议 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/protocols.html)
* [大规模 CI/CD MLOps ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/cicd-mlops.html)
* [Prometheus 指标 ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/analytics.html)
* [ELK 日志负载记录 ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/logging.html)
* [Jaeger 链路追踪 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/distributed-tracing.html)
* [副本扩展 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/scaling.html)
* [熔断 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/disruption-budgets.html)
* [自定义推理服务器](https://docs.seldon.io/projects/seldon-core/en/latest/servers/custom.html)

### 高级推理

* [模型解释 ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/explainers.html)
* [异常值检测 ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/outlier_detection.html)
* [路由 (包括 多臂老虎机)  ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/routers.html)

### 示例

* [笔记本 ](https://docs.seldon.io/projects/seldon-core/en/latest/examples/notebooks.html)
* [文章/博客 ](https://docs.seldon.io/projects/seldon-core/en/latest/tutorials/blogs.html)
* [视频 ](https://docs.seldon.io/projects/seldon-core/en/latest/tutorials/videos.html)

### 参考

* [基于注解的配置 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/annotations.html)
* [基准测试 ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/benchmarking.html)
* [一般可用性 ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/ga.html)
* [Helm Charts ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/helm_charts.html)
* [镜像 ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/images.html)
* [日志记录和日志级别 ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/log_level.html)
* [私有 Docker 参考 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/private_registries.html)
* [预测 API ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/apis/index.html)
* [Python API 参考 ](https://docs.seldon.io/projects/seldon-core/en/latest/python/api/modules.html)
* [发布亮点 ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/release-highlights.html)
* [Seldon Deployment CRD ](https://docs.seldon.io/projects/seldon-core/en/latest/reference/seldon-deployment.html)
* [服务编排器 ](https://docs.seldon.io/projects/seldon-core/en/latest/graph/svcorch.html)
* [Kubeflow ](https://docs.seldon.io/projects/seldon-core/en/latest/analytics/kubeflow.html)

### 开发者

* [概述 ](https://docs.seldon.io/projects/seldon-core/en/latest/developer/readme.html)
* [为 Seldon Core 做贡献 ](https://docs.seldon.io/projects/seldon-core/en/latest/developer/contributing.html)
* [端到端测试 ](https://docs.seldon.io/projects/seldon-core/en/latest/developer/e2e.html)
* [路线图 ](https://docs.seldon.io/projects/seldon-core/en/latest/developer/roadmap.html)
* [使用私有仓库构建 ](https://docs.seldon.io/projects/seldon-core/en/latest/developer/build-using-private-repo.html)



## 关于「Seldon Core」

Seldon (ˈSɛldən) Core 名字的灵感来自 [the Foundation Series (Scifi Novel)](https://en.wikipedia.org/wiki/Foundation_series) ，它的前提是由一位名叫“Hari Seldon”的数学家组成，他毕生致力于发展一种心理史理论，这是一种新的有效的数学社会学，它为未来创造了条件可以在很长一段时间内（跨越数十万年）进行极其准确的预测。

## 商业支持

![](https://raw.githubusercontent.com/SeldonIO/seldon-core/master/doc/source/images/deploy-logo.png)

我们通过我们的企业产品 Seldon Deploy 提供商业支持。请访问 [https://www.seldon.io/](https://www.seldon.io/) 了解详情和试用。

