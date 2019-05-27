---
title: 'Obtener una vista previa: creación de un contenedor de Windows Server en un clúster de Azure Kubernetes Service (AKS)'
description: Aprenda a crear un clúster de Kubernetes rápidamente, implementar una aplicación en un contenedor de Windows Server en Azure Kubernetes Service (AKS) mediante la CLI de Azure.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: zarhoads
ms.openlocfilehash: 328fb9707c7151b8696cfb55e13567db90e45b7f
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991153"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Obtener una vista previa: creación de un contenedor de Windows Server en un clúster de Azure Kubernetes Service (AKS) mediante la CLI de Azure

Azure Kubernetes Service (AKS) es un servicio de Kubernetes administrado que le permite implementar y administrar clústeres rápidamente. En este artículo, implemente un clúster de AKS mediante la CLI de Azure. También implementa una aplicación de ejemplo ASP.NET en un contenedor de Windows Server en el clúster.

Esta funcionalidad actualmente está en su versión preliminar.

![Imagen de desplazamiento a la aplicación de ejemplo ASP.NET](media/windows-container/asp-net-sample-app.png)

En este artículo se da por supuesto un conocimiento básico de los conceptos de Kubernetes. Para más información, consulte [Conceptos básicos de Kubernetes de Azure Kubernetes Service (AKS)][kubernetes-concepts].

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, este artículo se requiere que se está ejecutando la CLI de Azure versión 2.0.61 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli-install].

## <a name="before-you-begin"></a>Antes de empezar

Debe agregar un grupo de nodos adicionales después de crear el clúster que se puede ejecutar contenedores de Windows Server. Agregar un grupo de nodos adicionales se trata en un paso posterior, pero primero deberá habilitar algunas características de vista previa.

> [!IMPORTANT]
> Características de versión preliminar AKS son autoservicio y participación. Las versiones preliminares se proporcionan para recopilar comentarios y los errores de nuestra comunidad. Sin embargo, no se admiten por soporte técnico de Azure. Si crea un clúster, o agregar estas características para clústeres existentes, ese clúster no se admite hasta que la característica ya no está en versión preliminar y se aprueba para disponibilidad general (GA).
>
> Si tiene problemas con las características de vista previa, [abra una incidencia en el repositorio de GitHub de AKS] [ aks-github] con el nombre de la característica de vista previa en el título del error.

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI
    
Los comandos de CLI para crear y administrar varios grupos de nodos que están disponibles en el *-versión preliminar de aks* extensión de la CLI. Instalar el *-versión preliminar de aks* extensión de CLI de Azure mediante el [Agregar extensión az] [ az-extension-add] de comandos, tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Si ha instalado anteriormente el *-versión preliminar de aks* instalar haya disponible de extensión, actualizaciones utilizando el `az extension update --name aks-preview` comando.

### <a name="register-windows-preview-feature"></a>Registrar la característica de vista previa de Windows

Para crear un clúster de AKS que puede usar varios grupos de nodos y ejecutar contenedores de Windows Server, primero hay que habilitar la *WindowsPreview* marcas en su suscripción de características. El *WindowsPreview* característica también usa clústeres de grupo de varios nodos y escalado de máquinas virtuales que se establezca para administrar la implementación y configuración de los nodos de Kubernetes. Registrar el *WindowsPreview* característica marca mediante el [register de la característica de az] [ az-feature-register] comando tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Cualquier clúster de AKS crea después de que se ha registrado correctamente el *WindowsPreview* marca de características usa esta experiencia de clúster de versión preliminar. Para seguir creando clústeres normales, totalmente compatible, no habilite características de vista previa en las suscripciones de producción. Utilice una prueba independiente o la suscripción de Azure de desarrollo para probar características de vista previa.

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Limitaciones

Al crear y administrar clústeres AKS que admiten varios grupos de nodos, se aplican las siguientes limitaciones:

* Varios grupos de nodos están disponibles para los clústeres creados después de registrar correctamente el *WindowsPreview*. Varios grupos de nodos también están disponibles si se registra el *MultiAgentpoolPreview* y *VMSSPreview* características para su suscripción. No se puede agregar o administrar grupos de nodos con un clúster de AKS existente creado antes de que estas características se han registrado correctamente.
* No se puede eliminar el primer grupo de nodos.

Aunque esta característica está en versión preliminar, se aplican las siguientes limitaciones adicionales:

* El clúster de AKS puede tener un máximo de ocho grupos de nodos.
* El clúster de AKS puede tener un máximo de 400 nodos entre esos grupos de ocho nodos.
* El nombre del grupo de nodos de Windows Server tiene un límite de 6 caracteres.
* Grupos de nodos de Windows Server no están disponibles en las regiones de Canadá en este momento.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un grupo lógico en el que se implementan y se administran los recursos de Azure. Cuando se crea un grupo de recursos, se le pide que especifique una ubicación. Dicha ubicación es donde se almacenan los metadatos del grupo de recursos, así como el lugar en el que los recursos se ejecutan en Azure si no se especifica otra región al crear los recursos. Cree un grupo de recursos con el comando [az group create][az-group-create].

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

En la siguiente salida de ejemplo se muestra que los recursos se crearon correctamente:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-aks-cluster"></a>Creación de un clúster de AKS
Para ejecutar un clúster de AKS que admite grupos de nodos para contenedores de Windows Server, debe usar una directiva de red que usa el clúster [Azure CNI] [ azure-cni-about] complemento de red (avanzada). Para obtener más información para ayudarle a planear los intervalos de subred necesarios y las consideraciones de red, consulte [configurar redes de Azure CNI][use-advanced-networking]. Use la [crear az aks] [ az-aks-create] comando para crear un clúster de AKS denominado *myAKSCluster*. Este comando creará los recursos de red es necesario si no existen.
  * El clúster está configurado con un nodo
  * El *windows-admin-password* y *windows-admin-username* parámetros establecen las credenciales de administrador para los contenedores de Windows Server creados en el clúster.

Proporcione sus propios segura *PASSWORD_WIN*.

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --enable-addons monitoring \
    --kubernetes-version 1.14.0 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --enable-vmss \
    --network-plugin azure
```

Transcurridos unos minutos, el comando se completa y devuelve información en formato JSON sobre el clúster.

## <a name="add-a-windows-server-node-pool"></a>Agregar un grupo de nodos de Windows Server

De forma predeterminada, se crea un clúster de AKS con un grupo de nodos que se puede ejecutar contenedores de Linux. Use `az aks nodepool add` comando para agregar un grupo de nodos adicionales que se puede ejecutar contenedores de Windows Server.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.14.0
```

El comando anterior crea un nuevo grupo de nodo denominado *npwin* y lo agrega a la *myAKSCluster*. Al crear un grupo de nodos para ejecutar contenedores de Windows Server, el valor predeterminado para *tamaño de máquina virtual de nodo* es *Standard_D2s_v3*. Si decide establecer la *tamaño de máquina virtual de nodo* parámetro, compruebe la lista de [restringida de tamaños de máquina virtual][restricted-vm-sizes]. El tamaño mínimo recomendado es *Standard_D2s_v3*. El comando anterior también usa la subred predeterminada en la red virtual predeterminado creada al ejecutar `az aks create`.

## <a name="connect-to-the-cluster"></a>Conexión al clúster

Para administrar un clúster de Kubernetes, usará [kubectl][kubectl], el cliente de línea de comandos de Kubernetes. Si usa Azure Cloud Shell, `kubectl` ya está instalado. Para instalar `kubectl` localmente, use el comando [az aks install-cli][az-aks-install-cli]:

```azurecli
az aks install-cli
```

Para configurar `kubectl` para conectarse a su clúster de Kubernetes, use el comando [az aks get-credentials][az-aks-get-credentials]. Con este comando se descargan las credenciales y se configura la CLI de Kubernetes para usarlas.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Para comprobar la conexión con el clúster, use el comando [kubectl get][kubectl-get] para que devuelva una lista de los nodos del clúster.

```azurecli-interactive
kubectl get nodes
```

La salida del ejemplo siguiente muestra el nodo único creado en los pasos anteriores. Asegúrese de que el estado del nodo es *Listo*:

```
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.0
aksnpwin987654                      Ready    agent   108s   v1.14.0
```

## <a name="run-the-application"></a>Ejecución de la aplicación

Un archivo de manifiesto de Kubernetes define un estado deseado del clúster, por ejemplo, qué imágenes de contenedor se van a ejecutar. En este artículo, se utiliza un manifiesto para crear todos los objetos necesarios para ejecutar la aplicación de ejemplo ASP.NET en un contenedor de Windows Server. Este manifiesto se incluye un [implementación de Kubernetes] [ kubernetes-deployment] para la aplicación de ejemplo ASP.NET y una referencia externa [servicio Kubernetes] [ kubernetes-service] a acceso a la aplicación desde internet.

La aplicación de ejemplo ASP.NET se proporciona como parte de la [ejemplos de .NET Framework] [ dotnet-samples] y se ejecuta en un contenedor de Windows Server. AKS requiere contenedores de Windows Server se base en las imágenes de *Windows Server 2019* o superior. También debe definir el archivo de manifiesto de Kubernetes un [selector del nodo] [ node-selector] para indicar a su clúster AKS para ejecutar el pod de la aplicación de ejemplo ASP.NET en un nodo que se puede ejecutar contenedores de Windows Server.

Cree un archivo denominado `sample.yaml` y cópielo en la siguiente definición de código YAML. Si usa Azure Cloud Shell, este archivo se puede crear mediante `vi` o `nano` como si funcionara en un sistema físico o virtual:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample
  labels:
    app: sample
spec:
  replicas: 1
  template:
    metadata:
      name: sample
      labels:
        app: sample
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": windows
      containers:
      - name: sample
        image: mcr.microsoft.com/dotnet/framework/samples:aspnetapp
        resources:
          limits:
            cpu: 1
            memory: 800m
          requests:
            cpu: .1
            memory: 300m
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sample
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: sample
```

Implemente la aplicación mediante el comando [kubectl apply][kubectl-apply] y especifique el nombre del manifiesto de YAML:

```azurecli-interactive
kubectl apply -f sample.yaml
```

La salida del ejemplo siguiente muestra la implementación y el servicio que se creó correctamente:

```
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Prueba de la aplicación

Cuando se ejecuta la aplicación, un servicio de Kubernetes expone el front-end de la aplicación a Internet. Este proceso puede tardar unos minutos en completarse.

Para supervisar el progreso, utilice el comando [kubectl get service][kubectl-get] con el argumento `--watch`.

```azurecli-interactive
kubectl get service sample --watch
```

Inicialmente el *EXTERNAL-IP* para el *ejemplo* servicio se muestra como *pendiente*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Cuando la dirección *EXTERNAL-IP* cambie de *pendiente* a una dirección IP pública real, use `CTRL-C` pata detener el proceso de inspección de `kubectl`. En la salida del ejemplo siguiente se muestra una dirección IP pública válida asignada al servicio:

```
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Para ver la aplicación de ejemplo en acción, abra un explorador web para la dirección IP externa del servicio.

![Imagen de desplazamiento a la aplicación de ejemplo ASP.NET](media/windows-container/asp-net-sample-app.png)

## <a name="delete-cluster"></a>Eliminar clúster

Cuando el clúster ya no se necesite, puede usar el comando [az group delete][az-group-delete] para eliminar el grupo de recursos, el servicio de contenedor y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Cuando elimina el clúster, la entidad de servicio Azure Active Directory que utiliza el clúster de AKS no se quita. Para conocer los pasos que hay que realizar para quitar la entidad de servicio, consulte [Consideraciones principales y eliminación de AKS][sp-delete].

## <a name="next-steps"></a>Pasos siguientes

En este artículo, implementó un clúster de Kubernetes y había implementado una aplicación de ejemplo ASP.NET en un contenedor de Windows Server en ella. [Acceda al panel web de Kubernetes][kubernetes-dashboard] del clúster que acaba de crear.

Para obtener más información sobre AKS y un ejemplo completo desde el código hasta la implementación, continúe con el tutorial del clúster de Kubernetes.

> [!div class="nextstepaction"]
> [Tutorial de AKS][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
[aks-github]: https://github.com/azure/aks/issues
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-provider-register]: /cli/azure/provider#az-provider-register
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cni-about]: concepts-network.md#azure-cni-advanced-networking
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[restricted-vm-sizes]: quotas-skus-regions.md#restricted-vm-sizes
[use-advanced-networking]: configure-advanced-networking.md
