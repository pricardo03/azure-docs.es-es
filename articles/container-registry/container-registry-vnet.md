---
title: Implementación de Azure container registry en una red virtual
description: Permitir el acceso a Azure container registry sólo de recursos en una red virtual de Azure o de intervalos de direcciones IP públicas.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.openlocfilehash: 15b67218b129b5e017e67651587c389af412d7a1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60867411"
---
# <a name="restrict-access-to-an-azure-container-registry-using-an-azure-virtual-network-or-firewall-rules"></a>Restringir el acceso a Azure container registry mediante una red virtual de Azure o las reglas de firewall

[Red Virtual de Azure](../virtual-network/virtual-networks-overview.md) proporciona redes, privada y segura para los de Azure y los recursos locales. Al implementar el registro de contenedor privado de Azure en una red virtual de Azure, puede asegurarse de que solo los recursos de la red virtual de acceso al registro. Para escenarios entre locales, también puede configurar reglas de firewall para permitir el acceso de registro solo desde direcciones IP específicas.

En este artículo se muestra dos escenarios para crear las reglas de acceso de red para limitar el acceso a Azure container registry: desde una máquina virtual implementada en la misma red, o desde la dirección IP pública de la máquina virtual.

> [!IMPORTANT]
> Esta funcionalidad actualmente está en su versión preliminar y se [aplican algunas limitaciones](#preview-limitations). Las versiones preliminares están a su disposición a condición de que acepte los [términos de uso adicionales][terms-of-use]. Es posible que algunos de los aspectos de esta característica cambien antes de ofrecer disponibilidad general.
>

## <a name="preview-limitations"></a>Limitaciones de vista previa

* Solo un **Premium** registro de contenedor puede configurarse con reglas de acceso de red. Para obtener información acerca de los niveles de servicio de registro, vea [SKU de Azure Container Registry](container-registry-skus.md). 

* Solo un [Azure Kubernetes Service](../aks/intro-kubernetes.md) clúster o Azure [máquina virtual](../virtual-machines/linux/overview.md) puede usarse como un host para tener acceso a un registro de contenedor en una red virtual. *Actualmente no se admiten otros servicios de Azure como Azure Container Instances.*

* [Tareas de ACR](container-registry-tasks-overview.md) operaciones no se admiten actualmente en un registro de contenedor que se implementa en una red virtual.

* Cada registro admite un máximo de 100 reglas de red virtual.

## <a name="prerequisites"></a>Requisitos previos

* Para usar la CLI de los pasos de este artículo, la CLI de Azure versión 2.0.58 o posterior es necesaria. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][azure-cli].

* Si aún no tiene un registro de contenedor, crear uno (es necesario SKU Premium) e insertar una imagen de ejemplo como `hello-world` desde Docker Hub. Por ejemplo, use el [portal de Azure] [ quickstart-portal] o [CLI de Azure] [ quickstart-cli] para crear un registro. 

## <a name="about-network-rules-for-a-container-registry"></a>Acerca de las reglas de red para un registro de contenedor

De forma predeterminada Azure container registry acepta las conexiones a través de internet de los hosts de cualquier red. Con una red virtual, puede permitir que solo los recursos de Azure como un clúster de AKS o máquina virtual de Azure para obtener acceso seguro a del registro, sin traspasar un límite de red. También puede configurar reglas de firewall de red a la lista blanca concretos de internet pública intervalos de direcciones IP. 

Para limitar el acceso a un registro, primero cambie la acción predeterminada del registro para que lo deniega todas las conexiones de red. A continuación, agregue las reglas de acceso de red. Los clientes de conceder acceso a través de las reglas de red debe seguir [autenticarse en el registro de contenedor](https://docs.microsoft.com/azure/container-registry/container-registry-authentication) y esté autorizado para acceder a los datos.

### <a name="service-endpoint-for-subnets"></a>Punto de conexión de servicio para subredes

Para permitir el acceso desde una subred en una red virtual, deberá agregar una [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) para el servicio de Azure Container Registry. 

Servicios de varios inquilinos, como Azure Container Registry, utilizan un único conjunto de direcciones IP para todos los clientes. Un extremo de servicio asigna un punto de conexión para tener acceso a un registro. Este punto de conexión proporciona tráfico una ruta óptima a los recursos a través de la red troncal de Azure. Las identidades de la red virtual y la subred también se transmiten con cada solicitud.

### <a name="firewall-rules"></a>Reglas de firewall

Las reglas de red IP, proporcionar permitido internet intervalos de direcciones con notación CIDR como *16.17.18.0/24* o direcciones IP individual como *16.17.18.19*. Reglas de red IP solo se permiten para *pública* direcciones IP de internet. No se permiten intervalos de direcciones IP reservados para las redes privadas (tal y como se define en RFC 1918) en las reglas IP.

## <a name="create-a-docker-enabled-virtual-machine"></a>Crear una máquina virtual habilitada para Docker

En este artículo, use una VM de Ubuntu Docker habilitados para tener acceso a Azure container registry. Para usar la autenticación de Azure Active Directory en el registro también instalar la [CLI de Azure] [ azure-cli] en la máquina virtual. Si ya tiene una máquina virtual de Azure, omita este paso de creación.

Puede usar el mismo grupo de recursos para la máquina virtual y el registro de contenedor. Este programa de instalación simplifica la limpieza al final, pero no es necesario. Si decide crear un grupo de recursos independiente para la máquina virtual y la red virtual, ejecute [crear grupo az][az-group-create]. En el ejemplo siguiente se crea un grupo de recursos denominado *myResourceGroup* en el *westcentralus* ubicación:

```azurecli
az group create --name myResourceGroup --location westus
```

Implementar ahora de forma predeterminada una máquina virtual de Ubuntu Azure [crear az vm][az-vm-create]. En el ejemplo siguiente se crea una máquina virtual denominada *myDockerVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

La máquina virtual tarda unos minutos en crearse. Cuando se complete el comando, anote el valor `publicIpAddress` mostrado por la CLI de Azure. Use esta dirección para establecer conexiones SSH a la máquina virtual y, opcionalmente, para la instalación posterior de reglas de firewall.

### <a name="install-docker-on-the-vm"></a>Instalación de Docker en la máquina virtual

Después de ejecutar la máquina virtual, establezca una conexión SSH con la máquina virtual. Reemplace *publicIpAddress* por la dirección IP pública de la máquina virtual.

```bash
ssh azureuser@publicIpAddress
```

Ejecute el siguiente comando para instalar a Docker en la VM de Ubuntu:

```bash
sudo apt install docker.io -y
```

Después de la instalación, ejecute el siguiente comando para comprobar que Docker se ejecute correctamente en la máquina virtual:

```bash
sudo docker run -it hello-world
```

Salida:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
[...]
```

### <a name="install-the-azure-cli"></a>Instalación de la CLI de Azure

Siga los pasos del artículo [Instalación de la CLI de Azure con apt](/cli/azure/install-azure-cli-apt?view=azure-cli-latest) para instalar la CLI de Azure en la máquina virtual de Ubuntu. En este artículo, asegúrese de instalar la versión 2.0.58 o posterior.

Salga de la conexión SSH.

## <a name="allow-access-from-a-virtual-network"></a>Permitir el acceso desde una red virtual

En esta sección, configurará el registro de contenedor para permitir el acceso desde una subred de una red virtual de Azure. Se proporcionan los pasos equivalentes mediante la CLI de Azure y Azure portal.

### <a name="allow-access-from-a-virtual-network---cli"></a>Permitir el acceso desde una red virtual: CLI

#### <a name="add-a-service-endpoint-to-a-subnet"></a>Agregar un extremo de servicio a una subred

Cuando se crea una máquina virtual, Azure de forma predeterminada crea una red virtual en el mismo grupo de recursos. El nombre de la red virtual se basa en el nombre de la máquina virtual. Por ejemplo, si el nombre de la máquina virtual *myDockerVM*, el nombre de red virtual predeterminado es *myDockerVMVNET*, con una subred denominada *myDockerVMSubnet*. Comprobar esto en Azure portal o mediante el [lista de red virtual de red az] [ az-network-vnet-list] comando:

```azurecli
az network vnet list --resource-group myResourceGroup --query "[].{Name: name, Subnet: subnets[0].name}"
```

Salida:

```console
[
  {
    "Name": "myDockerVMVNET",
    "Subnet": "myDockerVMSubnet"
  }
]
```

Use la [actualización de subred de red virtual de red az] [ az-network-vnet-subnet-update] comando para agregar un **Microsoft.ContainerRegistry** punto de conexión de servicio a la subred. Sustituya los nombres de la red virtual y la subred en el siguiente comando:

```azurecli
az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints Microsoft.ContainerRegistry
```

Use la [show de subred de red virtual de red az] [ az-network-vnet-subnet-show] comando para recuperar el identificador de recurso de la subred. Lo necesitará en un paso posterior para configurar una regla de acceso de red.

```azurecli
az network vnet subnet show \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --query "id"
  --output tsv
``` 

Salida:

```
/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet
```

#### <a name="change-default-network-access-to-registry"></a>Cambiar el acceso a la red de forma predeterminada al registro

De forma predeterminada, Azure container registry permite las conexiones de los hosts de cualquier red. Para limitar el acceso a una red seleccionada, cambie la acción predeterminada para denegar el acceso. Sustituya el nombre del registro en la siguiente [actualización de az acr] [ az-acr-update] comando:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="add-network-rule-to-registry"></a>Agregar regla de red al registro

Use la [Agregar regla de red az acr] [ az-acr-network-rule-add] comando para agregar una regla de red en el registro que permite el acceso desde la subred de la máquina virtual. Sustituya el nombre del registro de contenedor y el identificador de recurso de la subred en el siguiente comando: 

 ```azurecli
az acr network-rule add --name mycontainerregistry --subnet <subnet-resource-id>
```

Seguir [comprobar el acceso al registro](#verify-access-to-the-registry).

### <a name="allow-access-from-a-virtual-network---portal"></a>Permitir el acceso desde una red virtual: portal

#### <a name="add-service-endpoint-to-subnet"></a>Agregar punto de conexión de servicio a la subred

Cuando se crea una máquina virtual, Azure de forma predeterminada crea una red virtual en el mismo grupo de recursos. El nombre de la red virtual se basa en el nombre de la máquina virtual. Por ejemplo, si el nombre de la máquina virtual *myDockerVM*, el nombre de red virtual predeterminado es *myDockerVMVNET*, con una subred denominada *myDockerVMSubnet*.

Para agregar un extremo de servicio para Azure Container Registry a una subred:

1. En el cuadro de búsqueda en la parte superior de la [portal Azure][azure-portal], escriba *redes virtuales*. Cuando aparezca la opción **Redes virtuales** en los resultados de la búsqueda, selecciónela.
1. En la lista de redes virtuales, seleccione la red virtual donde se implementa la máquina virtual, como *myDockerVMVNET*.
1. En **configuración**, seleccione **subredes**.
1. Seleccione la subred donde se implementa la máquina virtual, como *myDockerVMSubnet*.
1. En **los extremos del servicio**, seleccione **Microsoft.ContainerRegistry**.
1. Seleccione **Guardar**.

![Agregar punto de conexión de servicio a la subred][acr-subnet-service-endpoint] 

#### <a name="configure-network-access-for-registry"></a>Configurar el acceso de red para el registro

De forma predeterminada, Azure container registry permite las conexiones de los hosts de cualquier red. Para limitar el acceso a la red virtual:

1. En el portal, vaya a su registro de contenedor.
1. En **configuración**, seleccione **Firewall y redes virtuales**.
1. Para denegar el acceso de forma predeterminada, elija permitir el acceso desde **Redes seleccionadas**. 
1. Seleccione **agregar red virtual existente**y seleccione la red virtual y subred que configuró con un punto de conexión de servicio. Seleccione **Agregar**.
1. Seleccione **Guardar**.

![Configuración de red virtual para el registro de contenedor][acr-vnet-portal]

Seguir [comprobar el acceso al registro](#verify-access-to-the-registry).

## <a name="allow-access-from-an-ip-address"></a>Permitir el acceso desde una dirección IP

En esta sección, configurará el registro de contenedor para permitir el acceso desde una subred de una red virtual de Azure. Se proporcionan los pasos equivalentes mediante la CLI de Azure y Azure portal.

### <a name="allow-access-from-an-ip-address---cli"></a>Permitir el acceso desde una dirección IP - CLI

#### <a name="change-default-network-access-to-registry"></a>Cambiar el acceso a la red de forma predeterminada al registro

Si aún no lo ha hecho, actualice la configuración del registro para denegar el acceso de forma predeterminada. Sustituya el nombre del registro en la siguiente [actualización de az acr] [ az-acr-update] comando:

```azurecli
az acr update --name myContainerRegistry --default-action Deny
```

#### <a name="remove-network-rule-from-registry"></a>Quitar regla de red del registro de

Si anteriormente ha agregado una regla de red para permitir el acceso desde la subred de la máquina virtual, quite el punto de conexión de servicio de la subred y la regla de red. Sustituya el nombre del registro de contenedor y el identificador de recurso de la subred que recuperó en el paso anterior en el [Quitar regla de red az acr] [ az-acr-network-rule-remove] comando: 

```azurecli
# Remove service endpoint

az network vnet subnet update \
  --name myDockerVMSubnet \
  --vnet-name myDockerVMVNET \
  --resource-group myResourceGroup \
  --service-endpoints ""

# Remove network rule

az acr network-rule remove --name mycontainerregistry --subnet <subnet-resource-id>
```

#### <a name="add-network-rule-to-registry"></a>Agregar regla de red al registro

Use la [Agregar regla de red az acr] [ az-acr-network-rule-add] comando para agregar una regla de red en el registro que permite el acceso desde la dirección IP de la máquina virtual. Sustituya el nombre del registro de contenedor y la dirección IP pública de la máquina virtual en el siguiente comando.

```azurecli
az acr network-rule add --name mycontainerregistry --ip-address <public-IP-address>
```

Seguir [comprobar el acceso al registro](#verify-access-to-the-registry).

### <a name="allow-access-from-an-ip-address---portal"></a>Permitir el acceso desde una dirección IP: portal

#### <a name="remove-existing-network-rule-from-registry"></a>Quitar regla de red existente del registro

Si anteriormente ha agregado una regla de red para permitir el acceso desde la subred de la máquina virtual, quite la regla existente. Omitir esta sección si desea tener acceso al registro de una máquina virtual diferente.

* Actualice la configuración de subred para quitar el punto de conexión de servicio de la subred para Azure Container Registry. 

  1. En el [portal Azure][azure-portal], navegue a la red virtual donde se implementa la máquina virtual.
  1. En **configuración**, seleccione **subredes**.
  1. Seleccione la subred donde se implementa la máquina virtual.
  1. En **los extremos del servicio**, quite la casilla de verificación **Microsoft.ContainerRegistry**. 
  1. Seleccione **Guardar**.

* Quitar la regla de red que permita la subred para tener acceso al registro.

  1. En el portal, vaya a su registro de contenedor.
  1. En **configuración**, seleccione **Firewall y redes virtuales**.
  1. En **redes virtuales**, seleccione el nombre de la red virtual y, a continuación, seleccione **quitar**.
  1. Seleccione **Guardar**.

#### <a name="add-network-rule-to-registry"></a>Agregar regla de red al registro

1. En el portal, vaya a su registro de contenedor.
1. En **configuración**, seleccione **Firewall y redes virtuales**.
1. Si aún no lo ha hecho, optar por permitir el acceso desde **redes seleccionadas**. 
1. En **redes virtuales**, asegúrese de que no se ha seleccionado ninguna red.
1. En **Firewall**, escriba la dirección IP pública de una máquina virtual. O bien, escriba un intervalo de direcciones en la notación CIDR que contiene la dirección IP de la máquina virtual.
1. Seleccione **Guardar**.

![Configurar la regla de firewall para el registro de contenedor][acr-vnet-firewall-portal]

Seguir [comprobar el acceso al registro](#verify-access-to-the-registry).

## <a name="verify-access-to-the-registry"></a>Comprobar el acceso al registro

Después de esperar unos minutos a actualizar la configuración, compruebe que la máquina virtual puede acceder al registro de contenedor. Realice una conexión SSH a la máquina virtual y ejecute el [el inicio de sesión de az acr] [ az-acr-login] comando para iniciar sesión en el registro. 

```bash
az acr login --name mycontainerregistry
```

Puede realizar las operaciones de registro, como ejecutar `docker pull` para extraer una imagen de ejemplo desde el registro. Sustituir un valor de imagen y etiqueta adecuado para el registro, el prefijo con el nombre de servidor de inicio de sesión del registro (todo en minúsculas):

```bash
docker pull mycontainerregistry.azurecr.io/hello-world:v1
``` 

Docker correctamente extrae la imagen a la máquina virtual.

En este ejemplo se muestra que pueden acceder al registro de contenedor privado a través de la regla de acceso de red. Sin embargo, el registro no es accesible desde un host de otro inicio de sesión que no tiene configurada una regla de acceso de red. Si intenta iniciar sesión desde otro host mediante el `az acr login` comando o `docker login` comando, el resultado es similar al siguiente:

```Console
Error response from daemon: login attempt to https://xxxxxxx.azurecr.io/v2/ failed with status: 403 Forbidden
```

## <a name="restore-default-registry-access"></a>Restaurar el acceso de registro predeterminado

Para restaurar el registro para permitir el acceso de forma predeterminada, quite todas las reglas de red que están configuradas. A continuación, establezca la acción predeterminada para permitir el acceso. Se proporcionan los pasos equivalentes mediante la CLI de Azure y Azure portal.

### <a name="restore-default-registry-access---cli"></a>Restaurar el acceso de registro predeterminado - CLI

#### <a name="remove-network-rules"></a>Quita las reglas de red

Para ver una lista de reglas de red configuradas para el registro, ejecute el siguiente [lista de reglas de red de az acr] [ az-acr-network-rule-list] comando:

```azurecli
az acr network-rule list--name mycontainerregistry 
```

Para cada regla que está configurado, ejecute el [Quitar regla de red az acr] [ az-acr-network-rule-remove] comando para quitarla. Por ejemplo: 

```azurecli
# Remove a rule that allows access for a subnet. Substitute the subnet resource ID.

az acr network-rule remove \
  --name mycontainerregistry \
  --subnet /subscriptions/ \
  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myDockerVMVNET/subnets/myDockerVMSubnet

# Remove a rule that allows access for an IP address or CIDR range such as 23.45.1.0/24.

az acr network-rule remove \
  --name mycontainerregistry \
  --ip-address 23.45.1.0/24
```

#### <a name="allow-access"></a>Permitir acceso

Sustituya el nombre del registro en la siguiente [actualización de az acr] [ az-acr-update] comando:
```azurecli
az acr update --name myContainerRegistry --default-action Allow
```

### <a name="restore-default-registry-access---portal"></a>Restauración del registro el acceso predeterminado - portal


1. En el portal, navegue hasta el registro de contenedor y seleccione **Firewall y redes virtuales**.
1. En **redes virtuales**, seleccione cada red virtual y, a continuación, seleccione **quitar**.
1. En **Firewall**, seleccione cada intervalo de direcciones y, a continuación, seleccione el icono de eliminación.
1. En **permitir el acceso desde**, seleccione **todas las redes**. 
1. Seleccione **Guardar**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha creado todos los recursos de Azure en el mismo recurso de grupo y ya no los necesita, también puede eliminar los recursos mediante una sola [eliminación del grupo az](/cli/azure/group) comando:

```azurecli
az group delete --name myResourceGroup
```

Para limpiar los recursos en el portal, navegue hasta el grupo de recursos myResourceGroup. Una vez cargado el grupo de recursos, haga clic en **eliminar grupo de recursos** para quitar el grupo de recursos y los recursos almacenados en ella.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se trataron varios recursos de red virtual y características, aunque de forma breve. La documentación de Azure Virtual Network trata estos temas ampliamente:

* [Red virtual](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network)
* [Subred](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet)
* [Puntos de conexión de servicio](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)

<!-- IMAGES -->

[acr-subnet-service-endpoint]: ./media/container-registry-vnet/acr-subnet-service-endpoint.png
[acr-vnet-portal]: ./media/container-registry-vnet/acr-vnet-portal.png
[acr-vnet-firewall-portal]: ./media/container-registry-vnet/acr-vnet-firewall-portal.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/r/microsoft/aci-helloworld/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-list]: /cli/azure/acr/repository#az-acr-repository-list
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-network-rule-add]: /cli/azure/acr/network-rule/#az-acr-network-rule-add
[az-acr-network-rule-remove]: /cli/azure/acr/network-rule/#az-acr-network-rule-remove
[az-acr-network-rule-list]: /cli/azure/acr/network-rule/#az-acr-network-rule-list
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-update]: /cli/azure/acr#az-acr-update
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-group-create]: /cli/azure/group
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-vm-create]: /cli/azure/vm#az-vm-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-subnet-update]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-update
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet/#az-network-vnet-subnet-show
[az-network-vnet-list]: /cli/azure/network/vnet/#az-network-vnet-list
[quickstart-portal]: container-registry-get-started-portal.md
[quickstart-cli]: container-registry-get-started-azure-cli.md
[azure-portal]: https://portal.azure.com
