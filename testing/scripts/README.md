# 端到端测试

## 执行测试套件

### 要求

我们使用：

[Kind](https://github.com/kubernetes-sigs/kind) v0.6.1

[S2I](https://github.com/openshift/source-to-image) v1.1.14

[Kustomize](https://github.com/kubernetes-sigs/kustomize/blob/master/site/content/en/docs/Getting%20started/installation.md) v3.2.3

[Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.16.2

[Pytest](https://docs.pytest.org/en/latest/getting-started.html#install-pytest)
`pip install -U pytest`

[gRPC.io Tools](https://grpc.io/docs/languages/python/quickstart/#grpc-tools)
`python -m pip install grpcio-tools`

### 设置

执行以下命令设置所有：

```console
kind_test_setup.sh
```

可用的 kind kubernetes 配置：

```console
export KUBECONFIG="$(kind get kubeconfig-path)"
```

### 运行

运行测试并输出日志到文件：

```console
make test_sequential > test_sequential.log
```
```console
make test_parallel > test_parallel.log
```
```console
make test_notebooks > test_notebooks.log
```

运行单个测试
```console
pytest -sv test_tags.py::TestTagsPythonS2iK8s::test_model_single_grpc > test_model_single_grpc.log
```

### 日志

在独立终端查看测试日志：

```console
tail -f test_sequential.log
```

在独立的终端查看控制器日志：

```console
export KUBECONFIG="$(kind get kubeconfig-path)"
kubectl logs -f -n seldon-system $(kubectl get pods -n seldon-system -l app=seldon -o jsonpath='{.items[0].metadata.name}')
```

## 编写新测试：

### 个人命名空间

每个测试都应该运行在他们各自的 Kubernetes 命名空间。
这允许更清洁的环境，并使其中一些能够平行化 (参考 [串行和并行测试](#Sequential-and-parallel-tests))。

为使创建和删除命名空间更简化，你可以
使用 `namespace` 装置。
这个 fixture 负责创建一个具有唯一名称的名称空间，并在最后将其拆下。
要使用它，你仅需指定 `namespace` 参数作为传参的一部分传入到测试函数。
该 fixture 会在后台取值 `namespace` 参数值，并创建一个新的命名空间。

```python
def test_foo(..., namespace, ...):
  print(f"the new namespace's name is {namespace}")
```

### 串行及并行测试

我们借助 `pytest-xdist` 并行化执行测试来加速测试套件。
然而一些测试可能跟其他测试一起执行时产生副作用。
比如，操作器更新测试会改变整个集群的 Seldon 操作器，这
会影响并行测试执行。

为了区分串行和并行化的测试，你需要使用 `sequential` 标记。
标记 `pytest` 以允许 [选择测试子集](http://doc.pytest.org/en/latest/example/markers.html)。

要标记串行测试，你可以；

```python
@pytest.mark.sequential
def test_foo(...):
  print("the scripts will run this test sequentially")
```

测试整合脚本会确保测试标记为 `sequential` 以运行在独立的 worker 中。
