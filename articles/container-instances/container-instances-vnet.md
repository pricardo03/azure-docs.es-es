---
title: Implementación de instancias de contenedor en una red virtual de Azure
description: Aprenda a implementar grupos de contenedores en una red virtual de Azure nueva o existente.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 09/24/2018
ms.author: danlep
ms.openlocfilehash: feb9547b004141a3c1d02ef4b356b9d00b74fc95
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902379"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>Implementación de instancias de contenedor en una red virtual de Azure

[Azure Virtual Network](../virtual-network/virtual-networks-overview.md) proporciona acceso de red seguro y privado, con filtros, enrutamiento y emparejamiento, para los recursos locales y de Azure. Al implementar grupos de contenedores en una red virtual de Azure, los contenedores pueden comunicarse de forma segura con otros recursos de la red virtual.

Los grupos de contenedores implementados en una red virtual de Azure permiten escenarios como:

* Comunicación directa entre grupos de contenedores en la misma subred
* Enviar la salida de la carga de trabajo [basada en tareas](container-instances-restart-policy.md) procedente de las instancias de contenedor a una base de datos en la red virtual
* Recuperar contenido de las instancias de contenedor desde un [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) en la red virtual
* Comunicación de contenedor con máquinas virtuales en la red virtual
* Comunicación de contenedor con recursos locales mediante una [puerta de enlace VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) o [ExpressRoute](../expressroute/expressroute-introduction.md)

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar y se [aplican algunas limitaciones](#preview-limitations). Las versiones preliminares están a su disposición a condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.

## <a name="virtual-network-deployment-limitations"></a>Limitaciones de la implementación de redes virtuales

Se aplican ciertas limitaciones al implementar grupos de contenedores en una red virtual.

* No se admiten contenedores de Windows
* Para implementar grupos de contenedores en una subred, la subred no puede contener otros tipos de recursos. Quite todos los recursos existentes de una subred existente antes de implementar grupos de contenedores en ella o crear una nueva subred.
* Los grupos de contenedores implementados en una red virtual no admiten actualmente direcciones IP públicas ni etiquetas de nombre DNS.
* Debido a los recursos de red adicionales implicados, implementar un grupo de contenedores en una red virtual suele ser algo más lento que implementar una instancia de contenedor estándar.

## <a name="preview-limitations"></a>Limitaciones de vista previa

Aunque esta característica está en versión preliminar, las siguientes limitaciones se aplican al implementar instancias de contenedor en una red virtual.

Regiones **admitidas**:

* Europa Occidental (westeurope)
* Oeste de EE. UU. (westus)

Recursos de red **no admitidos**:

* Grupo de seguridad de red (NSG)
* Azure Load Balancer

**La eliminación de recursos de red** requiere [pasos adicionales](#delete-network-resources) cuando se han implementado grupos de contenedores en la red virtual.

## <a name="required-network-resources"></a>Recursos de red necesarios

Hay tres recursos de Azure Virtual Network necesarios para implementar grupos de contenedores en una red virtual: la propia [red virtual](#virtual-network), una [subred delegada](#subnet-delegated) dentro de la red virtual y un [perfil de red](#network-profile).

### <a name="virtual-network"></a>Virtual network

Una red virtual define el espacio de direcciones en el que crear una o varias subredes. A continuación, implemente los recursos de Azure (por ejemplo, los grupos de contenedores) en las subredes de la red virtual.

### <a name="subnet-delegated"></a>Subred (delegada)

Las subredes segmentan la red virtual en espacios de direcciones independientes utilizables por los recursos de Azure que coloque en ellos. Crea una o varias subredes dentro de una red virtual.

La subred que usó para los grupos de contenedores puede contener solo grupos de contenedores. Cuando implementa por primera vez un grupo de contenedores en una subred, Azure delega esa subred para Azure Container Instances. Una vez delegada, la subred se puede usar solo para grupos de contenedores. Si intenta implementar recursos distintos de grupos de contenedores en una subred delegada, se produce un error en la operación.

### <a name="network-profile"></a>Perfil de red

Un perfil de red es una plantilla de configuración de red para los recursos de Azure. Especifica determinadas propiedades de red para el recurso, por ejemplo, la subred en la que debe implementarse. La primera vez que implementa un grupo de contenedores en una subred (y, por tanto, en una red virtual), Azure crea un perfil de red para usted. A continuación, puede usar ese perfil de red para implementaciones futuras en la subred.

En el diagrama siguiente, se han implementado varios grupos de contenedores en una subred delegada en Azure Container Instances. Una vez implementado un grupo de contenedores en una subred, puede implementar grupos de contenedores adicionales en él si especifica el mismo perfil de red.

![Grupos de contenedores dentro de una red virtual][aci-vnet-01]

## <a name="deploy-to-virtual-network"></a>Implementación en una red virtual

Puede implementar grupos de contenedores en una nueva red virtual y permitir a Azure crear los recursos de red necesarios para usted, o implementarlos en una red virtual existente.

### <a name="new-virtual-network"></a>Nueva red virtual

Para implementarlos en una nueva red virtual y hacer que Azure cree automáticamente los recursos de red, especifique lo siguiente al ejecutar [az container create][az-container-create]:

* Nombre de la red virtual
* Prefijo de dirección de red virtual en formato CIDR
* Nombre de subred
* Prefijo de dirección de subred en formato CIDR

Los prefijos de direcciones de subred y red virtual especifican los espacios de direcciones para la subred y la red virtual, respectivamente. Estos valores se representan en notación de Enrutamiento de interdominios sin clases (CIDR), por ejemplo `10.0.0.0/16`. Para más información sobre cómo trabajar con las subredes, consulte [Incorporación, cambio o eliminación de una subred de red virtual](../virtual-network/virtual-network-manage-subnet.md).

Cuando haya implementado el primer grupo de contenedores con este método, puede implementarlo en la misma subred; para ello, especifique los nombres de la red virtual y la subred, o el perfil de red que Azure crea automáticamente para usted. Dado que Azure delega la subred en Azure Container Instances, *solo* puede implementar grupos de contenedores en la subred.

### <a name="existing-virtual-network"></a>Red virtual existente

Para implementar un grupo de contenedores en una red virtual existente:

1. Cree una subred dentro de la red virtual existente o vacíe una subred existente de *todos* los demás recursos
1. Implemente un grupo de contenedores con [az container create][az-container-create] y especifique uno de los siguientes:
   * Nombre de red virtual y nombre de subred</br>
    o
   * Nombre de perfil de red o identificador

Una vez implemente su primer grupo de contenedores en una subred, Azure delega esa subred a Azure Container Instances. Ya no puede implementar recursos distintos de grupos de contenedores para esa subred.

Las secciones siguientes describen cómo implementar grupos de contenedores en una red virtual con la CLI de Azure. En los ejemplos de comandos se da formato para el shell de **Bash**. Si prefiere otro shell como PowerShell o el símbolo del sistema, ajuste los caracteres de continuación de línea según corresponda.

## <a name="deploy-to-new-virtual-network"></a>Implementación en una nueva red virtual

En primer lugar, implemente un grupo de contenedores y especifique los parámetros para una nueva red virtual y subred. Cuando especifique estos parámetros, Azure crea la red virtual y la subred, delega la subred en Azure Container Instances y también crea un perfil de red. Una vez creados estos recursos, el grupo de contenedores se implementa en la subred.

Ejecute el siguiente comando [az container create][az-container-create] que especifica valores para una nueva red virtual y subred. Este comando implementa el contenedor [microsoft/aci-helloworld][aci-helloworld] que ejecuta un pequeño servidor web de Node.js que sirve una página web estática. En la siguiente sección, implementará un segundo grupo de contenedores en la misma subred y probará la comunicación entre las dos instancias de contenedor.

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image microsoft/aci-helloworld \
    --vnet-name aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

Al implementar en una nueva red virtual mediante este método, la implementación puede tardar unos minutos mientras se crean los recursos de red. Tras la implementación inicial, las implementaciones de grupos de contenedores adicionales se completan más rápido.

## <a name="deploy-to-existing-virtual-network"></a>Implementación en una red virtual existente

Ahora que ha implementado un grupo de contenedores en una nueva red virtual, implemente un segundo grupo en la misma subred y compruebe la comunicación entre las dos instancias.

En primer lugar, obtenga la dirección IP del primer grupo de contenedores que implementó, *appcontainer*:

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

La salida debe mostrar la dirección IP del grupo de contenedores en la subred privada:

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

Ahora, establezca `CONTAINER_GROUP_IP` en la dirección IP que recuperó con el comando `az container show` y ejecute el siguiente comando `az container create`. Este segundo contenedor, *commchecker*, ejecuta una imagen basada en Linux de Alpine y `wget` con la dirección IP de la subred privada del primer grupo de contenedores.

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet-name aci-vnet \
    --subnet aci-subnet
```

Una vez completada esta segunda implementación de contenedor, extraiga sus registros para que pueda ver la salida del comando `wget` que ejecutó:

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

Si el segundo contenedor se comunicó correctamente con el primero, la salida debe ser similar a la siguiente:

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

La salida del registro debe mostrar que `wget` fue capaz de conectarse y descargar el archivo de índice del primer contenedor usando su dirección IP privada en la subred local. El tráfico de red entre los dos grupos de contenedores permanece dentro de la red virtual.

## <a name="deploy-to-existing-virtual-network---yaml"></a>Implementación en una red virtual existente: YAML

También puede implementar un grupo de contenedores en una red virtual existente mediante un archivo YAML. Para implementar en una subred de una red virtual, especifique varias propiedades adicionales en el archivo YAML:

* `ipAddress`: la configuración de direcciones IP para el grupo de contenedores.
  * `ports`: los puertos que deben abrirse, si hay alguno.
  * `protocol`: el protocolo (TCP o UDP) para el puerto abierto.
* `networkProfile`: especifica la configuración de red, como la red virtual y la subred para un recurso de Azure.
  * `id`: el identificador de recurso del Administrador de recursos completo de `networkProfile`.

Para implementar un grupo de contenedores en una red virtual con un archivo YAML, primero deberá obtener el identificador del perfil de red. Ejecute el comando [az network profile list][az-network-profile-list], especificando el nombre del grupo de recursos que contenga la red virtual y la subred delegada.

``` azurecli
az network profile list --resource-group myResourceGroup --query [0].id --output tsv
```

La salida del comando muestra el identificador de recurso completo para el perfil de red:

```console
$ az network profile list --resource-group myResourceGroup --query [0].id --output tsv
/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-aci-subnet
```

Una vez que tenga el identificador del perfil de red, copie el siguiente YAML en un nuevo archivo denominado *vnet-deploy-aci.yaml*. En `networkProfile`, reemplace el valor de `id` con el identificador recién recuperado y, después, guarde el archivo. Este YAML crea un grupo de contenedores denominado *appcontaineryaml* en la red virtual.

```YAML
apiVersion: '2018-09-01'
location: westus
name: appcontaineryaml
properties:
  containers:
  - name: appcontaineryaml
    properties:
      image: microsoft/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  ipAddress:
    type: Private
    ports:
    - protocol: tcp
      port: '80'
  networkProfile:
    id: /subscriptions/<Subscription ID>/resourceGroups/container/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-subnet
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Implemente el grupo de contenedores con el comando [az container create][az-container-create] y especifique el nombre del archivo YAML para el parámetro `--file`:

```azurecli
az container create --resource-group myResourceGroup --file vnet-deploy-aci.yaml
```

Una vez completada la implementación, ejecute el comando [az container show][az-container-show] para mostrar su estado:

```console
$ az container show --resource-group myResourceGroup --name appcontaineryaml --output table
Name              ResourceGroup    Status    Image                     IP:ports     Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  ------------------------  -----------  ---------  ---------------  --------  ----------
appcontaineryaml  myResourceGroup  Running   microsoft/aci-helloworld  10.0.0.5:80  Private    1.0 core/1.5 gb  Linux     westus
```

## <a name="clean-up-resources"></a>Limpieza de recursos

### <a name="delete-container-instances"></a>Eliminación de instancias de contenedor

Cuando termine de trabajar con las instancias de contenedor que creó, elimínelas con los siguientes comandos:

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
az container delete --resource-group myResourceGroup --name appcontaineryaml -y
```

### <a name="delete-network-resources"></a>Eliminación de recursos de red

La versión preliminar inicial de esta característica requiere varios comandos adicionales para eliminar los recursos de red que creó anteriormente. Si ha usado los comandos de ejemplo en las secciones anteriores de este artículo para crear la red virtual y la subred, puede usar el siguiente script para eliminar esos recursos de red.

Antes de ejecutar el script, establezca la variable `RES_GROUP` en el nombre del grupo de recursos que contenga la red virtual y la subred que se deben eliminar. El script tiene el formato adecuado para el shell de Bash. Si prefiere otro shell como PowerShell o el símbolo del sistema, deberá ajustar los descriptores de acceso y la asignación de variables según corresponda.

> [!WARNING]
> Este script elimina los recursos. Elimina la red virtual y todas las subredes que contiene. Asegúrese de que ya no necesita *ninguno* de los recursos en la red virtual, incluidas las subredes que contiene, antes de ejecutar este script. Una vez eliminado, **estos recursos son irrecuperables**.

```azurecli
# Replace <my-resource-group> with the name of your resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Get the service association link (SAL) ID
SAL_ID=$(az network vnet subnet show --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --query id --output tsv)/providers/Microsoft.ContainerInstance/serviceAssociationLinks/default

# Delete the default SAL ID for the subnet
az resource delete --ids $SAL_ID --api-version 2018-07-01

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet --remove delegations 0

# Delete the subnet
az network vnet subnet delete --resource-group $RES_GROUP --vnet-name aci-vnet --name aci-subnet

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo se trataron varios recursos de red virtual y características, aunque de forma breve. La documentación de Azure Virtual Network trata estos temas ampliamente:

* [Red virtual](../virtual-network/manage-virtual-network.md)
* [Subred](../virtual-network/virtual-network-manage-subnet.md)
* [Puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [ExpressRoute](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-profile-list]: /cli/azure/network/profile#az-network-profile-list