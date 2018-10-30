---
title: 'Guía de inicio rápido: Clúster de Azure Kubernetes para Linux'
description: Averigüe rápidamente cómo puede crear un clúster de Kubernetes para contenedores de Linux en AKS con la CLI de Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: iainfou
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: a24d0080dcb714f409506bf6abe514e3f5022ccd
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457902"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster"></a>Guía de inicio rápido: Implementación de un clúster de Azure Kubernetes Service (AKS)

En esta guía de inicio rápido se implementa un clúster de AKS mediante la CLI de Azure. A continuación, se ejecuta en el clúster una aplicación de varios contenedores que consta de un front-end web y una instancia de Redis. Una vez finalizado el proceso, la aplicación es accesible a través de Internet.

![Imagen de la exploración hasta Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

En esta guía de inicio rápido se supone que tiene unos conocimientos básicos de los conceptos de Kubernetes. Para obtener información detallada sobre esta plataforma, consulte la [documentación de Kubernetes][kubernetes-documentation].

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si elige instalar y usar la CLI en un entorno local, para esta guía de inicio rápido es preciso que ejecute la versión 2.0.46 de la CLI de Azure, o cualquier versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando [az group create][az-group-create]. Un grupo de recursos de Azure es un grupo lógico en el que se implementan y se administran los recursos de Azure. Cuando se crea un grupo de recursos, se le pide que especifique una ubicación. Esta ubicación es donde se ejecutan los recursos en Azure.

En el ejemplo siguiente, se crea un grupo de recursos denominado *myAKSCluster* en la ubicación *eastus*.

```azurecli-interactive
az group create --name myAKSCluster --location eastus
```

Salida:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myAKSCluster",
  "location": "eastus",
  "managedBy": null,
  "name": "myAKSCluster",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>Creación de un clúster de AKS

Use el comando [az aks create][az-aks-create] para crear un clúster de AKS. En el siguiente ejemplo se crea un clúster denominado *myAKSCluster* con un nodo. Azure Monitor para contenedores también se habilita usando el parámetro *--enable-addons monitoring*. Para más información sobre cómo habilitar la solución de seguimiento de estado del contenedor, consulte [Monitor Azure Kubernetes Service health][aks-monitor] (Supervisión del estado de Azure Kubernetes Service).

```azurecli-interactive
az aks create --resource-group myAKSCluster --name myAKSCluster --node-count 1 --enable-addons monitoring --generate-ssh-keys
```

Unos minutos después, el comando se completa y devuelve información en formato JSON acerca del clúster.

## <a name="connect-to-the-cluster"></a>Conexión al clúster

Para administrar un clúster de Kubernetes, use [kubectl][kubectl], el cliente de línea de comandos de Kubernetes.

Si usa Azure Cloud Shell, `kubectl` ya está instalado. Si desea instalarlo localmente, use el comando [az aks install-cli][az-aks-install-cli].


```azurecli
az aks install-cli
```

Para configurar `kubectl` para conectarse a su clúster de Kubernetes, use el comando [az aks get-credentials][az-aks-get-credentials]. Con este paso se descargan las credenciales y se configura la CLI de Kubernetes para usarlas.

```azurecli-interactive
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

Para comprobar la conexión con el clúster, use el comando [kubectl get][kubectl-get] para que devuelva una lista de los nodos del clúster. Los nodos pueden tardar unos minutos en aparecer.

```azurecli-interactive
kubectl get nodes
```

Salida:

```
NAME                          STATUS    ROLES     AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     agent     2m        v1.7.7
```

## <a name="run-the-application"></a>Ejecución de la aplicación

Un archivo de manifiesto de Kubernetes define un estado deseado del clúster, por ejemplo, qué imágenes de contenedor se deben ejecutar. En esta guía de inicio rápido, se usa un manifiesto para crear todos los objetos necesarios para ejecutar la aplicación Azure Vote. Este manifiesto incluye dos [implementaciones de Kubernetes][kubernetes-deployment], una para las aplicaciones Python de Azure Vote y otra para una instancia de Redis. Además, se crean dos [servicios de Kubernetes][kubernetes-service], un servicio interno para la instancia de Redis y un servicio externo para acceder a la aplicación Azure Vote desde Internet.

> [!TIP]
> En esta guía de inicio rápido, se crean e implementan manualmente los manifiestos de aplicación en el clúster de AKS. En escenarios más reales, puede usar [Azure Dev Spaces] [azure-dev-spaces] para iterar rápidamente y depurar el código directamente en el clúster de AKS. Puede usar Dev Spaces entre plataformas de sistemas operativos y entornos de desarrollo, y trabajar junto con otras personas de su equipo.

Cree un archivo denominado `azure-vote.yaml` y copie el siguiente código YAML. Si está trabajando en Azure Cloud Shell, este archivo se puede crear mediante vi o Nano, como si trabajara en un sistema físico o virtual.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-back
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  selector:
    matchLabels:
      app: azure-vote-front
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Use el comando [kubectl apply][kubectl-apply] para ejecutar la aplicación.

```azurecli-interactive
kubectl apply -f azure-vote.yaml
```

Salida:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Prueba de la aplicación

Cuando se ejecuta la aplicación, se crea un [servicio de Kubernetes][kubernetes-service] que expone el front-end de la aplicación a Internet. Este proceso puede tardar unos minutos en completarse.

Para supervisar el progreso, utilice el comando [kubectl get service][kubectl-get] con el argumento `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

En un primer momento, el parámetro *EXTERNAL-IP* del servicio *azure-vote-front* aparece como *pendiente*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Una vez que la dirección *EXTERNAL-IP* haya cambiado de *pendiente* a una *dirección IP*, use `CTRL-C` para detener el proceso de inspección de kubectl.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Ahora, vaya a la dirección IP externa para ver la aplicación Azure Vote.

![Imagen de la exploración hasta Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="monitor-health-and-logs"></a>Supervisión de estado y registros

Cuando se creó el clúster de AKS, se habilitó la supervisión para capturar métricas de mantenimiento para los nodos de clúster y los pods. Estas métricas de mantenimiento están disponibles en Azure Portal. Para obtener más información sobre la supervisión del estado de los contenedores, consulte [Monitor Azure Kubernetes Service health][aks-monitor] (Supervisión del estado de Azure Kubernetes Service).

Para ver el estado actual, el tiempo de actividad y el uso de recursos para los pods de Azure Vote, complete los pasos siguientes:

1. En un explorador web, vaya a Azure Portal[https://portal.azure.com][azure-portal].
1. Seleccione el grupo de recursos, como *myResourceGroup*, a continuación, seleccione el clúster de AKS, como *myAKSCluster*.
1. En la opción **Supervisión** de la izquierda, elija **Insights (versión preliminar)**
1. En la parte superior, elija **+ Agregar filtro**
1. Seleccione *Namespace* como propiedad y, después, elija *\<All but kube-system\>*
1. Elija ver los **contenedores**.

Se muestran los contenedores *azure-vote-back* y *azure-vote-front*, como aparece en el ejemplo siguiente:

![Visualización del estado de ejecución de contenedores en AKS](media/kubernetes-walkthrough-portal/monitor-containers.png)

Para ver los registros del pod `azure-vote-front`, seleccione el vínculo **View container logs** (Ver registros del contenedor) a la derecha de la lista de contenedores. Estos registros incluyen los flujos *stdout* y *stderr* del contenedor.

![Visualización de los registros de contenedores en AKS](media/kubernetes-walkthrough-portal/monitor-container-logs.png)

## <a name="delete-cluster"></a>Eliminación de clúster

Cuando el clúster ya no se necesite, puede usar el comando [az group delete][az-group-delete] para eliminar el grupo de recursos, el servicio de contenedor y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myAKSCluster --yes --no-wait
```

> [!NOTE]
> Cuando elimina el clúster, la entidad de servicio Azure Active Directory que utiliza el clúster de AKS no se quita. Para conocer los pasos que hay que realizar para quitar la entidad de servicio, consulte [Consideraciones principales y eliminación de AKS][sp-delete].

## <a name="get-the-code"></a>Obtención del código

En esta guía de inicio rápido, se han usado imágenes de un contenedor creado previamente para crear una implementación de Kubernetes. El código de la aplicación relacionada, Dockerfile, y el archivo de manifiesto de Kubernetes están disponibles en GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha implementado un clúster de Kubernetes y una aplicación de varios contenedores en él.

Para obtener más información sobre AKS y un ejemplo completo desde el código hasta la implementación, continúe con el tutorial del clúster de Kubernetes.

> [!div class="nextstepaction"]
> [Tutorial de AKS][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-deployment]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[azure-dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/

<!-- LINKS - internal -->
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
