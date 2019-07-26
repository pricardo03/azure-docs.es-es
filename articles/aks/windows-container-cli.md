---
title: 'Versión preliminar: Creación de un contenedor de Windows Server en un clúster de Azure Kubernetes Service (AKS)'
description: Aprenda a crear rápidamente un clúster de Kubernetes y a implementar una aplicación en un contenedor de Windows Server en Azure Kubernetes Service (AKS) mediante la CLI de Azure.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/17/2019
ms.author: mlearned
ms.openlocfilehash: 305901007180cfb197cf5c0dfb338800449560a1
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68382036"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Versión preliminar: Creación de un contenedor de Windows Server en un clúster de Azure Kubernetes Service (AKS) mediante la CLI de Azure

Azure Kubernetes Service (AKS) es un servicio de Kubernetes administrado que le permite implementar y administrar clústeres rápidamente. En este artículo, implementará un clúster de AKS mediante la CLI de Azure. También implementará una aplicación de ejemplo de ASP.NET en un contenedor de Windows Server en el clúster.

Esta funcionalidad actualmente está en su versión preliminar.

![Imagen de cómo llegar a la aplicación de ejemplo de ASP.NET](media/windows-container/asp-net-sample-app.png)

En este artículo se presupone un conocimiento básico de los conceptos de Kubernetes. Para más información, consulte [Conceptos básicos de Kubernetes de Azure Kubernetes Service (AKS)][kubernetes-concepts].

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.61 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][azure-cli-install].

## <a name="before-you-begin"></a>Antes de empezar

Debe agregar un grupo de nodos adicionales después de crear el clúster que puede ejecutar contenedores de Windows Server. La adición de un grupo de nodos adicionales se trata en el último paso, pero primero deberá habilitar varias características en versión preliminar.

> [!IMPORTANT]
> Las características en versión preliminar de AKS son de autoservicio y se tienen que habilitar. Se proporcionan para recopilar comentarios y errores de nuestra comunidad. En la versión preliminar, estas características no están diseñadas para su uso en producción. Las características en versión preliminar pública se incluyen en el soporte técnico de "mejor esfuerzo". Los equipos de soporte técnico de AKS ofrecen asistencia solo durante el horario laboral en la zona horaria del Pacífico (PST). Para más información, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS][aks-support-policies]
> * [Preguntas más frecuentes de soporte técnico de Azure][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI

Para usar contenedores de Windows Server, necesitará la versión 0.4.1 de la extensión de la CLI *aks-preview* o una posterior. Instale la extensión de la CLI de Azure *aks-preview* mediante el comando [az extension add][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-windows-preview-feature"></a>Registro de las características en versión preliminar de Windows

Para crear un clúster de AKS que pueda usar varios grupos de nodos y ejecutar contenedores de Windows Server, primero hay que habilitar las marcas de la característica *WindowsPreview* en la suscripción. La característica *WindowsPreview* también usa clústeres de grupos de varios nodos y conjuntos de escalado de máquinas virtuales para administrar la implementación y configuración de los nodos de Kubernetes. Para registrar la marca de característica *WindowsPreview*, use el comando [az feature register][az-feature-register] tal como se muestra en el siguiente ejemplo:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Todos los clústeres que cree una vez que haya registrado satisfactoriamente la marca de característica *WindowsPreview* usarán la versión preliminar de esta experiencia de clúster. Para seguir creando clústeres normales, totalmente compatibles, no habilite características en versión preliminar en las suscripciones de producción. Utilice una suscripción independiente de prueba o de desarrollo de Azure para probar características en versión preliminar.

El proceso de registro tarda unos minutos en completarse. Compruebe el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Cuando el estado de registro sea `Registered`, presione CTRL+C para detener la supervisión.  Después, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Limitaciones

Se aplican las siguientes limitaciones cuando crea y administra clústeres de AKS que admiten varios grupos de nodos:

* Hay varios grupos de nodos disponibles para los clústeres creados después de haber registrado correctamente *WindowsPreview*. También hay varios grupos de nodos disponibles si registra las características *MultiAgentpoolPreview* y *VMSSPreview* para la suscripción. No se pueden agregar ni administrar grupos de nodos con un clúster de AKS existente creado antes de que estas características se registraran satisfactoriamente.
* No puede eliminar el primer grupo de nodos.

Aunque esta característica está en versión preliminar, se aplican las siguientes limitaciones adicionales:

* El clúster de AKS puede tener un máximo de ocho grupos de nodos.
* El clúster de AKS puede tener un máximo de 400 nodos distribuidos entre esos ocho grupos de nodos.
* El nombre del grupo de nodos de Windows Server tiene un límite de 6 caracteres.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un grupo lógico en el que se implementan y se administran los recursos de Azure. Cuando se crea un grupo de recursos, se le pide que especifique una ubicación. Dicha ubicación es donde se almacenan los metadatos del grupo de recursos, así como el lugar en el que los recursos se ejecutan en Azure si no se especifica otra región al crear los recursos. Cree un grupo de recursos con el comando [az group create][az-group-create].

En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*.

> [!NOTE]
> En este artículo se utiliza sintaxis de Bash para los comandos del tutorial.
> Si usa Azure Cloud Shell, asegúrese de que la lista desplegable de la superior izquierda de la ventana de Cloud Shell está establecida en **Bash**.

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

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

Para ejecutar un clúster de AKS que admita grupos de nodos para contenedores de Windows Server, el clúster debe utilizar una directiva de red que use el complemento de red (avanzado) de [Azure CNI][azure-cni-about] (advanced) network plugin. For more detailed information to help plan out the required subnet ranges and network considerations, see [configure Azure CNI networking][use-advanced-networking]. Use el comando [az aks create][az-aks-create] para crear un clúster de AKS denominado *myAKSCluster*. Este comando creará los recursos de red necesarios en caso de que no existan.
  * El clúster se configura con un nodo
  * Los parámetros *windows-admin-password* y *windows-admin-username* establecen las credenciales de administrador de todos los contenedores de Windows Server creados en el clúster.

Proporcione su *PASSWORD_WIN* seguro propio (recuerde que los comandos de este artículo se han agregado a un shell de BASH):

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --enable-addons monitoring \
    --kubernetes-version 1.14.1 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --enable-vmss \
    --network-plugin azure
```

> [!Note]
> Si recibe un error de validación de contraseña, intente crear el grupo de recursos en otra región.
> A continuación, intente crear el clúster con el nuevo grupo de recursos.

Transcurridos unos minutos, el comando se completa y devuelve información en formato JSON sobre el clúster.

## <a name="add-a-windows-server-node-pool"></a>Adición de un grupo de nodos de Windows Server

De forma predeterminada, se crea un clúster de AKS con un grupo de nodos que puede ejecutar contenedores de Linux. Use el comando `az aks nodepool add` para agregar un grupo de nodos adicionales que pueda ejecutar contenedores de Windows Server.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.14.1
```

El comando anterior crea un nuevo grupo de nodos denominado *npwin* y lo agrega a *myAKSCluster*. Al crear un grupo de nodos para ejecutar contenedores de Windows Server, el valor predeterminado para *node-vm-size* es *Standard_D2s_v3*. Si decide establecer el parámetro *node-vm-size*, compruebe la lista de [tamaños de máquina virtual restringidos][restricted-vm-sizes]. El tamaño mínimo recomendado es *Standard_D2s_v3*. El comando anterior también usa la subred predeterminada en la red virtual predeterminada que se crea al ejecutar `az aks create`.

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
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
aksnpwin987654                      Ready    agent   108s   v1.14.1
```

## <a name="run-the-application"></a>Ejecución de la aplicación

Un archivo de manifiesto de Kubernetes define un estado deseado del clúster, por ejemplo, qué imágenes de contenedor se van a ejecutar. En este artículo, se utiliza un manifiesto para crear todos los objetos necesarios para ejecutar la aplicación de ejemplo de ASP.NET en un contenedor de Windows Server. Este manifiesto se incluye una [implementación de Kubernetes][kubernetes-deployment] for the ASP.NET sample application and an external [Kubernetes service][kubernetes-service] para acceder a la aplicación desde internet.

La aplicación de ejemplo de ASP.NET se proporciona como parte de los [ejemplos de .NET Framework][dotnet-samples] y se ejecuta en un contenedor de Windows Server. AKS requiere contenedores de Windows Server que se basen en las imágenes de *Windows Server 2019* u otra versión posterior. El archivo de manifiesto de Kubernetes también debe definir un [selector de nodos][node-selector] que le indique al clúster de AKS que ejecute el pod de la aplicación de ejemplo de ASP.NET en un nodo que pueda ejecutar contenedores de Windows Server.

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
            memory: 800M
          requests:
            cpu: .1
            memory: 300M
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

En la salida de ejemplo siguiente se muestran las implementaciones y los servicios creados correctamente:

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

En un primer momento, el parámetro *EXTERNAL-IP* del servicio de *ejemplo* se muestra como *pendiente*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Cuando la dirección *EXTERNAL-IP* cambie de *pendiente* a una dirección IP pública real, use `CTRL-C` pata detener el proceso de inspección de `kubectl`. En la salida del ejemplo siguiente se muestra una dirección IP pública válida asignada al servicio:

```
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Para ver la aplicación de ejemplo en acción, abra un explorador web en la dirección IP externa del servicio.

![Imagen de cómo llegar a la aplicación de ejemplo de ASP.NET](media/windows-container/asp-net-sample-app.png)

## <a name="delete-cluster"></a>Eliminación de clúster

Cuando el clúster ya no se necesite, puede usar el comando [az group delete][az-group-delete] para eliminar el grupo de recursos, el servicio de contenedor y todos los recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Cuando elimina el clúster, la entidad de servicio Azure Active Directory que utiliza el clúster de AKS no se quita. Para conocer los pasos que hay que realizar para quitar la entidad de servicio, consulte [Consideraciones principales y eliminación de AKS][sp-delete].

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha implementado un clúster de Kubernetes y una aplicación de ejemplo de ASP.NET en un contenedor de Windows Server de este. [Acceda al panel web de Kubernetes][kubernetes-dashboard] del clúster que acaba de crear.

Para obtener más información sobre AKS y un ejemplo completo desde el código hasta la implementación, continúe con el tutorial del clúster de Kubernetes.

> [!div class="nextstepaction"]
> [Tutorial de AKS][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
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
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
