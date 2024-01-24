# NVIDIA 预估服务代理

NVIDIA 预估服务代理提供了 Seldon 预估请求转发来运行 [NVIDIA 预估服务](https://docs.nvidia.com/deeplearning/sdk/inference-user-guide/index.html)。

## Configuration

Nvidia Proxy 有以下参数：

 | 参数 | 类型 | 值 | 示例 |
 |-----------|------|-------|---------|
 | url | STRING | URL to Nvidia Inference Server endpoint | 127.0.0.1:8000 |
 | model_name | STRING | model name | tensorrt_mnist |
 | protocol | STRING | API 协议： HTTP or GRPC | HTTP |


下面显示了从 MNIST 演示中获取的 SeldonDeployment Kubernetes 资源示例，以说明如何设置这些参数。该图由三个容器组成

  1. 对原始输入进行特征变换的 Seldon 变换器。
  1. 一个 NVIDIA 推理服务器模型代理。
  1. NVIDIA 推理服务器加载了一个模型。

![MNIST Example](./mnist-graph.png)


```json
{
    "apiVersion": "machinelearning.seldon.io/v1alpha2",
    "kind": "SeldonDeployment",
    "metadata": {
        "labels": {
            "app": "seldon"
        },
        "name": "nvidia-mnist",
	"namespace": "kubeflow"
    },
    "spec": {
        "name": "caffe2-mnist",
        "predictors": [
            {
                "componentSpecs": [{
                    "spec": {
                        "containers": [
                            {
                                "image": "seldonio/mnist-caffe2-transformer:0.1",
                                "name": "mnist-transformer"
                            },
                            {
                                "image": "seldonio/nvidia-inference-server-proxy:0.1",
                                "name": "nvidia-proxy"
                            },
			    {
				"args": [
				    "--model-store=gs://seldon-inference-server-model-store"
				],
				"command": [
				    "inference_server"
				],
				"image": "nvcr.io/nvidia/inferenceserver:18.08.1-py2",
				"livenessProbe": {
				    "failureThreshold": 3,
				    "handler":{
					"httpGet": {
					    "path": "/api/health/live",
					    "port": 8000,
					    "scheme": "HTTP"
					}
				    },
				    "initialDelaySeconds": 5,
				    "periodSeconds": 5,
				    "successThreshold": 1,
				    "timeoutSeconds": 1
				},
				"name": "inference-server",
				"ports": [
				    {
					"containerPort": 8000,
					"protocol": "TCP"
				    },
				    {
					"containerPort": 8001,
					"protocol": "TCP"
				    },
				    {
					"containerPort": 8002,
					"protocol": "TCP"
				    }
				],
				"readinessProbe": {
				    "failureThreshold": 3,
				    "handler":{
					"httpGet": {
					    "path": "/api/health/ready",
					    "port": 8000,
					    "scheme": "HTTP"
					}
				    },
				    "initialDelaySeconds": 5,
				    "periodSeconds": 5,
				    "successThreshold": 1,
				    "timeoutSeconds": 1
				},
				"resources": {
				    "limits": {
					"nvidia.com/gpu": "1"
				    },
				    "requests": {
					"cpu": "100m",
					"nvidia.com/gpu": "1"
				    }
				},
				"securityContext": {
				    "runAsUser": 1000
				}
			    }
			],
			"terminationGracePeriodSeconds": 1,
			"imagePullSecrets": [
			    {
				"name": "ngc"
			    }
			]
		    }
		}],
                "graph": {
                    "name": "mnist-transformer",
                    "endpoint": { "type" : "REST" },
                    "type": "TRANSFORMER",
		    "children": [
			{
			    "name": "nvidia-proxy",
			    "endpoint": { "type" : "REST" },
			    "type": "MODEL",
			    "children": [],
			    "parameters":
			    [
				{
				    "name":"url",
				    "type":"STRING",
				    "value":"127.0.0.1:8000"
				},
				{
				    "name":"model_name",
				    "type":"STRING",
				    "value":"tensorrt_mnist"
				},
				{
				    "name":"protocol",
				    "type":"STRING",
				    "value":"HTTP"
				}
			    ]
			}
		    ]
                },
                "name": "mnist-nvidia",
                "replicas": 1
            }
        ]
    }
}
```

