# Kubebuilder v2 Seldon Operator

## 开发

我们推荐:

 * Go 1.13
 * Kubebuilder 2.3.0

### 讨论

 * Generated CRD is not structural: https://github.com/kubernetes-sigs/controller-tools/issues/304
   There is a PR for this: https://github.com/kubernetes-sigs/controller-tools/pull/312

### 要求

For running locally `kind`, `kustomize` and `kubebuilder` should be installed.

### 测试

If you installed kubebuilder outside of `/usr/local/kubebuilder` then you will need to set the env var `KUBEBUILDER_ASSETS` for example:

```
export KUBEBUILDER_ASSETS=/home/clive/tools/kubebuilder_2.3.0_linux_amd64/bin
```


Start a kind cluster

```
kind create cluster
export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"
```

Install CRD and cert-manager

```
make install
make install-cert-manager
```

Build image

```
make docker-build
```

## 标准部署

```
make deploy
```

## 本地开发部署


Deploy with webhook config set to point to host

```
make deploy-local
```

When running update tls certificates locally

```
make tls-extract
```

Now delete the cluster Deployment as we will run the manager locally:

```
kubectl delete deployment -n seldon-system seldon-controller-manager
``` 

Now we can run locally:

```
make run
```

You should now be able to create SeldonDeployments and Webhook calls will hit the local running manager. The same applies if you debug from GoLand. Though for GoLand you will need to export the KUBECONFIG to the debug configuration.

You should delete the Operator running in the cluster at this point.

# 构建 Helm Chart

Use the Makefile in the `./helm` directory. Ensure you have `pyyaml` in your python environment.

# Openshift

参考 [这里](openshift.md)