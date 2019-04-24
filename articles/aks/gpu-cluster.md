---
title: Uso de las GPU en Azure Kubernetes Service (AKS)
description: Obtenga información acerca de cómo usar GPU para cargas de trabajo de cálculo de alto rendimiento o de uso intensivo de gráficos en Azure Kubernetes Service (AKS)
services: container-service
author: zr-msft
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/28/2019
ms.author: zarhoads
ms.openlocfilehash: 150eaa6a4df558ed0c737d99cbcc8010baf63e96
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466399"
---
# <a name="use-gpus-for-compute-intensive-workloads-on-azure-kubernetes-service-aks"></a>Uso de GPU para cargas de trabajo de cálculo intensivo en Azure Kubernetes Service (AKS)

Por lo general, las unidades de procesamiento gráfico (GPU) se usan para cargas de trabajo de cálculo intensivo, como cargas de trabajo de visualización y gráficos. AKS admite la creación de grupos de nodos compatibles con GPU para ejecutar estas cargas de trabajo de cálculo intensivo en Kubernetes. Para obtener más información sobre las máquinas virtuales habilitadas para GPU disponibles, consulte [GPU optimized VM sizes in Azure][gpu-skus] (Tamaños de VM optimizadas para GPU en Azure). Para nodos AKS, se recomienda un tamaño mínimo de *Standard_NC6*.

> [!NOTE]
> Las máquinas virtuales habilitadas para GPU contienen hardware especializado que está sujeto a una mayor disponibilidad de precios y región. Consulte la herramienta de [precios][azure-pricing] y el sitio de [disponibilidad por región][azure-availability].

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS con nodos que admiten GPU. El clúster de AKS debe ejecutar Kubernetes 1.10 o una versión posterior. Si necesita un clúster de AKS que cumpla estos requisitos, consulte la primera sección de este artículo para [crear un clúster de AKS](#create-an-aks-cluster).

También necesita la CLI de Azure versión 2.0.59 o posterior instalado y configurado. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

Si necesita un clúster de AKS que cumpla los requisitos mínimos (nodo habilitado para GPU y Kubernetes versión 1.10 o posterior), siga estos pasos. Si ya tiene un clúster de AKS que cumpla estos requisitos, [ir a la siguiente sección](#confirm-that-gpus-are-schedulable).

Primero, cree un grupo de recursos para el clúster con el comando [az group create][az-group-create]. En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la región *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ahora cree un clúster de AKS con el comando [az aks create][az-aks-create]. En el ejemplo siguiente se crea un clúster con un solo nodo de tamaño `Standard_NC6`, y se ejecuta Kubernetes versión 1.11.7:

```azurecli
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-vm-size Standard_NC6 \
    --node-count 1 \
    --kubernetes-version 1.11.8
```

Obtenga las credenciales para el clúster de AKS mediante el comando [az aks get-credentials][az-aks-get-credentials]:

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="confirm-that-gpus-are-schedulable"></a>Confirmación de que las GPU son programables

Con el clúster de AKS creado, confirme que las GPU son programables en Kubernetes. Primero, enumere los nodos del clúster con el comando [kubectl get nodes][kubectl-get]:

```
$ kubectl get nodes

NAME                       STATUS   ROLES   AGE   VERSION
aks-nodepool1-28993262-0   Ready    agent   6m    v1.11.7
```

Ahora, use el comando [kubectl describe node][kubectl-describe] para confirmar que las GPU son programables. En la sección *Capacidad*, la GPU debe aparecer como `nvidia.com/gpu:  1`. Si no ve las GPU, consulte la sección [Solución de problemas de disponibilidad de GPU](#troubleshoot-gpu-availability).

En el siguiente ejemplo reducido se muestra que una GPU está disponible en el nodo denominado *aks-nodepool1-18821093-0*:

```
$ kubectl describe node aks-nodepool1-28993262-0

Name:               aks-nodepool1-28993262-0
Roles:              agent
Labels:             accelerator=nvidia

[...]

Capacity:
 cpu:                6
 ephemeral-storage:  30428648Ki
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             57713780Ki
 nvidia.com/gpu:     1
 pods:               110
Allocatable:
 cpu:                5916m
 ephemeral-storage:  28043041951
 hugepages-1Gi:      0
 hugepages-2Mi:      0
 memory:             52368500Ki
 nvidia.com/gpu:     1
 pods:               110
System Info:
 Machine ID:                 9148b74152374d049a68436ac59ee7c7
 System UUID:                D599728C-96F3-B941-BC79-E0B70453609C
 Boot ID:                    a2a6dbc3-6090-4f54-a2b7-7b4a209dffaf
 Kernel Version:             4.15.0-1037-azure
 OS Image:                   Ubuntu 16.04.5 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://1.13.1
 Kubelet Version:            v1.11.7
 Kube-Proxy Version:         v1.11.7
PodCIDR:                     10.244.0.0/24
ProviderID:                  azure:///subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Compute/virtualMachines/aks-nodepool1-28993262-0
Non-terminated Pods:         (9 in total)
  Namespace                  Name                                     CPU Requests  CPU Limits  Memory Requests  Memory Limits  AGE
  ---------                  ----                                     ------------  ----------  ---------------  -------------  ---
  gpu-resources              nvidia-device-plugin-97zfc               0 (0%)        0 (0%)      0 (0%)           0 (0%)         2m4s

[...]
```

## <a name="run-a-gpu-enabled-workload"></a>Ejecución de una carga de trabajo habilitada para GPU

Para ver la GPU en acción, programe una carga de trabajo compatible con la GPU con la solicitud de recursos adecuada. En este ejemplo se va a ejecutar un trabajo [Tensorflow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners) en el [conjunto de datos MNIST](http://yann.lecun.com/exdb/mnist/).

Cree un archivo denominado *samples-tf-mnist-demo.yaml* y pegue el siguiente manifiesto YAML. El siguiente manifiesto de trabajo incluye un límite de recursos de `nvidia.com/gpu: 1`:

> [!NOTE]
> Si recibe un error de coincidencia de versión al llamar a los controladores, como, por ejemplo, que la versión del controlador CUDA no es suficiente para la versión en tiempo de ejecución de CUDA, revise el gráfico de compatibilidad de la matriz de controladores de nVidia- [https://docs.nvidia.com/deploy/cuda-compatibility/index.html](https://docs.nvidia.com/deploy/cuda-compatibility/index.html)

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  labels:
    app: samples-tf-mnist-demo
  name: samples-tf-mnist-demo
spec:
  template:
    metadata:
      labels:
        app: samples-tf-mnist-demo
    spec:
      containers:
      - name: samples-tf-mnist-demo
        image: microsoft/samples-tf-mnist-demo:gpu
        args: ["--max_steps", "500"]
        imagePullPolicy: IfNotPresent
        resources:
          limits:
           nvidia.com/gpu: 1
      restartPolicy: OnFailure
```

Use el comando [kubectl apply][kubectl-apply] para ejecutar el trabajo. Este comando analiza el archivo de manifiesto y crea los objetos de Kubernetes definidos:

```console
kubectl apply -f samples-tf-mnist-demo.yaml
```

## <a name="view-the-status-and-output-of-the-gpu-enabled-workload"></a>Ver el estado y la salida de la carga de trabajo habilitada para GPU

Supervise el progreso del trabajo mediante el comando [kubectl get jobs][kubectl-get] con el argumento `--watch`. Puede tardar unos minutos en extraer primero la imagen y procesar el conjunto de datos. Cuando la columna *COMPLETIONS* (FINALIZACIONES) muestra *1/1*, significa que el trabajo ha finalizado correctamente:

```
$ kubectl get jobs samples-tf-mnist-demo --watch

NAME                    COMPLETIONS   DURATION   AGE

samples-tf-mnist-demo   0/1           3m29s      3m29s
samples-tf-mnist-demo   1/1   3m10s   3m36s
```

Para ver la salida de la carga de trabajo habilitada para GPU, obtenga primero el nombre de los pod con el comando [kubectl get pods][kubectl-get]:

```
$ kubectl get pods --selector app=samples-tf-mnist-demo

NAME                          READY   STATUS      RESTARTS   AGE
samples-tf-mnist-demo-smnr6   0/1     Completed   0          3m
```

Ahora, use el comando [kubectl logs][kubectl-logs] para ver los registros del pod. En el siguiente ejemplo, los registros de pod confirman que se ha descubierto el dispositivo GPU adecuado, `Tesla K80`. Proporcione el nombre para su pod:

```
$ kubectl logs samples-tf-mnist-demo-smnr6

2019-02-28 23:47:34.749013: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2019-02-28 23:47:34.879877: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: 3130:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2019-02-28 23:47:34.879915: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: 3130:00:00.0, compute capability: 3.7)
2019-02-28 23:47:39.492532: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
Accuracy at step 40: 0.8685
Accuracy at step 50: 0.8864
Accuracy at step 60: 0.901
Accuracy at step 70: 0.905
Accuracy at step 80: 0.9103
Accuracy at step 90: 0.9126
Adding run metadata for 99
Accuracy at step 100: 0.9176
Accuracy at step 110: 0.9149
Accuracy at step 120: 0.9187
Accuracy at step 130: 0.9253
Accuracy at step 140: 0.9252
Accuracy at step 150: 0.9266
Accuracy at step 160: 0.9255
Accuracy at step 170: 0.9267
Accuracy at step 180: 0.9257
Accuracy at step 190: 0.9309
Adding run metadata for 199
Accuracy at step 200: 0.9272
Accuracy at step 210: 0.9321
Accuracy at step 220: 0.9343
Accuracy at step 230: 0.9388
Accuracy at step 240: 0.9408
Accuracy at step 250: 0.9394
Accuracy at step 260: 0.9412
Accuracy at step 270: 0.9422
Accuracy at step 280: 0.9436
Accuracy at step 290: 0.9411
Adding run metadata for 299
Accuracy at step 300: 0.9426
Accuracy at step 310: 0.9466
Accuracy at step 320: 0.9458
Accuracy at step 330: 0.9407
Accuracy at step 340: 0.9445
Accuracy at step 350: 0.9486
Accuracy at step 360: 0.9475
Accuracy at step 370: 0.948
Accuracy at step 380: 0.9516
Accuracy at step 390: 0.9534
Adding run metadata for 399
Accuracy at step 400: 0.9501
Accuracy at step 410: 0.9552
Accuracy at step 420: 0.9535
Accuracy at step 430: 0.9545
Accuracy at step 440: 0.9533
Accuracy at step 450: 0.9526
Accuracy at step 460: 0.9566
Accuracy at step 470: 0.9547
Accuracy at step 480: 0.9548
Accuracy at step 490: 0.9545
Adding run metadata for 499
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para eliminar los objetos Kubernetes asociados creados en este artículo, use el comando [kubectl delete job][kubectl delete] como sigue:

```console
kubectl delete jobs samples-tf-mnist-demo
```

## <a name="troubleshoot-gpu-availability"></a>Solución de problemas de disponibilidad de GPU

Si no ve las GPU como disponibles en los nodos, deberá implementar un objeto DaemonSet para el complemento del dispositivo nVidia. Este objeto DaemonSet ejecuta un pod en cada nodo para proporcionar los controladores requeridos para las GPU.

En primer lugar, cree un espacio de nombres mediante el comando [kubectl create namespace][kubectl-create], como *gpu-resources*:

```console
kubectl create namespace gpu-resources
```

Cree un archivo denominado *nvidia-device-plugin-ds.yaml* y pegue el siguiente manifiesto YAML. Actualice `image: nvidia/k8s-device-plugin:1.11` a mitad del manifiesto para que coincida con la versión de Kubernetes. Por ejemplo, si el clúster de AKS ejecuta Kubernetes versión 1.12, actualice la etiqueta a `image: nvidia/k8s-device-plugin:1.12`.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  labels:
    kubernetes.io/cluster-service: "true"
  name: nvidia-device-plugin
  namespace: gpu-resources
spec:
  template:
    metadata:
      # Mark this pod as a critical add-on; when enabled, the critical add-on scheduler
      # reserves resources for critical add-on pods so that they can be rescheduled after
      # a failure.  This annotation works in tandem with the toleration below.
      annotations:
        scheduler.alpha.kubernetes.io/critical-pod: ""
      labels:
        name: nvidia-device-plugin-ds
    spec:
      tolerations:
      # Allow this pod to be rescheduled while the node is in "critical add-ons only" mode.
      # This, along with the annotation above marks this pod as a critical add-on.
      - key: CriticalAddonsOnly
        operator: Exists
      containers:
      - image: nvidia/k8s-device-plugin:1.11 # Update this tag to match your Kubernetes version
        name: nvidia-device-plugin-ctr
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
        volumeMounts:
          - name: device-plugin
            mountPath: /var/lib/kubelet/device-plugins
      volumes:
        - name: device-plugin
          hostPath:
            path: /var/lib/kubelet/device-plugins
      nodeSelector:
        beta.kubernetes.io/os: linux
        accelerator: nvidia
```

Ahora, use el comando [kubectl apply][kubectl-apply] para crear el Daemonset:

```
$ kubectl apply -f nvidia-device-plugin-ds.yaml

daemonset "nvidia-device-plugin" created
```

Ejecute el comando [kubectl describe node][kubectl-describe] de nuevo para comprobar si la GPU está ahora disponible en el nodo.

## <a name="next-steps"></a>Pasos siguientes

Para ejecutar trabajos de Apache Spark, consulte [Ejecución de trabajos de Apache Spark en AKS][aks-spark].

Para obtener más información acerca de la ejecución de cargas de trabajo de aprendizaje automático (ML) en Kubernetes, vea [Kubeflow Labs][kubeflow-labs].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubeflow-labs]: https://github.com/Azure/kubeflow-labs
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[azure-pricing]: https://azure.microsoft.com/pricing/
[azure-availability]: https://azure.microsoft.com/global-infrastructure/services/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-spark]: spark-job.md
[gpu-skus]: ../virtual-machines/linux/sizes-gpu.md
[install-azure-cli]: /cli/azure/install-azure-cli
