---
title: Configuración de redes kubenet en Azure Kubernetes Service (AKS)
description: Aprenda a configurar una red kubenet (básica) en Azure Kubernetes Service (AKS) para implementar un clúster de AKS en una red virtual existente y en una subred.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/31/2019
ms.author: iainfou
ms.reviewer: nieberts, jomore
ms.openlocfilehash: a4ed3ec823982bf3977edf9939d98419e1c4b01f
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956393"
---
# <a name="use-kubenet-networking-with-your-own-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Uso de redes kubenet con intervalos de direcciones IP propios en Azure Kubernetes Service (AKS)

De forma predeterminada, los clústeres de AKS usan [kubenet][kubenet], y una red virtual de Azure y una subred se crean automáticamente. Con *kubenet*, los nodos obtienen una dirección IP de una subred de la red virtual de Azure. Los pods reciben una dirección IP de un espacio de direcciones lógicamente distinto a la subred de red virtual de Azure de los nodos. A continuación, se configura la traducción de direcciones de red (NAT) para que los pods puedan acceder a los recursos en la red virtual de Azure. La dirección IP de origen del tráfico se somete a un proceso NAT hacia la dirección IP primaria del nodo. Este enfoque reduce enormemente el número de direcciones IP que se deben reservar en el espacio de red para su uso por parte de los pods.

Con [Azure Container Networking Interface (CNI)][cni-networking], cada pod obtiene una dirección IP de la subred, y se puede acceder directamente. Estas direcciones IP deben ser únicas en el espacio de red y deben planearse de antemano. Cada nodo tiene un parámetro de configuración para el número máximo de pods que admite. Luego, el número equivalente de direcciones IP por nodo se reserva por adelantado para ese nodo. Este enfoque requiere más planificación y a menudo lleva al agotamiento de direcciones IP o a la necesidad de volver a generar los clústeres en una subred mayor, a medida que crecen las exigencias de la aplicación.

En este artículo se muestra cómo usar las redes *kubenet* para crear y usar la subred de una red virtual con un clúster de AKS. Para más información sobre las opciones y consideraciones de red, consulte el artículo sobre los [conceptos de red para Kubernetes y AKS][aks-network-concepts].

> [!WARNING]
> Para usar grupos de nodos de Windows Server (actualmente en versión preliminar de AKS), debe usar Azure CNI. El uso de kubenet como el modelo de red no está disponible para los contenedores de Windows Server.

## <a name="before-you-begin"></a>Antes de empezar

Es preciso que esté instalada y configurada la versión 2.0.56 de la CLI de Azure u otra versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea  [Instalación de la CLI de Azure][install-azure-cli].

## <a name="overview-of-kubenet-networking-with-your-own-subnet"></a>Información general sobre redes kubenet con una subred propia

En muchos entornos, ha definido subredes y redes virtuales con intervalos de direcciones IP asignados. Estos recursos de red virtual se usan para admitir varios servicios y aplicaciones. Para proporcionar conectividad de red, los clústeres de AKS pueden usar *kubenet* (redes básicas) o Azure CNI (*redes avanzadas*).

Con *kubenet*, solo los nodos reciben una dirección IP en la subred de red virtual. Los pods no pueden comunicarse directamente entre sí. En su lugar, se usan el enrutamiento definido por el usuario (UDR) y el reenvío de IP para la conectividad entre pods a través de los nodos. También podría implementar pods detrás de un servicio que recibe una dirección IP asignada y equilibra la carga del tráfico de la aplicación. En el diagrama siguiente se muestra cómo los nodos de AKS reciben una dirección IP en la subred de red virtual, pero no los pods:

![Modelo de red kubenet con un clúster de AKS](media/use-kubenet/kubenet-overview.png)

Azure admite un máximo de 400 rutas en un UDR, por lo que no puede tener un clúster de AKS que tenga más de 400 nodos. Las características de AKS como los [nodos virtuales][virtual-nodes] o las directivas de red no son compatibles con *kubenet*.

Con *Azure CNI*, cada pod recibe una dirección IP en la subred IP y puede comunicarse directamente con otros pods y servicios. Los clústeres pueden ser tan grandes como el intervalo de direcciones IP que especifique. Sin embargo, el intervalo de direcciones IP debe planearse por adelantado, y los nodos de AKS consumen todas las direcciones IP en función del número máximo de pods que pueden admitir. Los escenarios y las características avanzadas de red como los [nodos virtuales][virtual-nodes] o las directivas de red son compatibles con *Azure CNI*.

### <a name="ip-address-availability-and-exhaustion"></a>Disponibilidad y agotamiento de las direcciones IP

Con *Azure CNI*, un problema común es que el intervalo de direcciones IP asignado es demasiado pequeño para agregar nodos adicionales cuando se escala o actualiza un clúster. Puede que el equipo de red no sea capaz de emitir un intervalo de direcciones IP lo suficientemente grande como para satisfacer las demandas esperadas de la aplicación.

Como compromiso, puede crear un clúster de AKS que use *kubenet* y conectarse a una subred de red virtual existente. Este enfoque permite que los nodos reciban direcciones IP definidas, sin necesidad de reservar un gran número de direcciones IP por adelantado para todos los pods posibles que se podrían ejecutar en el clúster.

Con *kubenet*, puede usar un intervalo de direcciones IP mucho más pequeño y tener la capacidad de satisfacer una elevada demanda de los clústeres y la aplicación. Por ejemplo, incluso con un intervalo de direcciones IP de */27*, podría ejecutar un clúster de 20-25 nodos con espacio suficiente para escalar o actualizar. Este tamaño de clúster admitiría hasta *2200-2750* pods (con una capacidad máxima predeterminada de 110 pods por nodo).

Los cálculos básicos siguientes comparan la diferencia entre los modelos de red:

- **kubenet**: un intervalo sencillo de direcciones IP de */24* puede admitir hasta *251* en el clúster (cada subred de red virtual de Azure reserva las tres primeras direcciones IP para operaciones de administración).
  - Este número de nodos podría admitir hasta *27610* pods (con una capacidad máxima predeterminada de 110 pods por nodo con *kubenet*).
- **Azure CNI**: ese mismo intervalo de subred básico de */24* solo podría admitir un máximo de *8* nodos en el clúster.
  - Este número de nodos podría admitir solo hasta *240* pods (con una capacidad máxima predeterminada de 30 pods por nodo con *Azure CNI*).

> [!NOTE]
> Estos valores máximos no tienen en cuenta las operaciones de actualización o escalado. En la práctica, no puede ejecutar el número máximo de nodos que el intervalo de direcciones IP de la subred admite. Debe dejar algunas direcciones IP disponibles para usarlas durante el escalado de las operaciones de actualización.

### <a name="virtual-network-peering-and-expressroute-connections"></a>Emparejamiento de redes virtuales y conexiones de ExpressRoute

Para proporcionar conectividad local, los dos enfoques de red *kubenet* y *Azure-CNI* pueden usar el [emparejamiento de redes virtuales de Azure][vnet-peering] o las [conexiones ExpressRoute][express-route]. Planee los intervalos de direcciones IP detenidamente para evitar la superposición y un enrutamiento de tráfico incorrecto. Por ejemplo, muchas redes locales usan un intervalo de direcciones *10.0.0.0/8* que se anuncia a través de la conexión ExpressRoute. Se recomienda crear los clústeres de AKS de las subredes de red virtual de Azure fuera de este intervalo de direcciones, como *172.26.0.0/16*.

### <a name="choose-a-network-model-to-use"></a>Selección de un modelo de red para usarlo

Elegir qué complemento de red utilizar para el clúster de AKS suele ser una cuestión de equilibrar las necesidades de flexibilidad y configuración avanzada. Las consideraciones siguientes ayudan a indicar cuándo puede resultar más conveniente cada modelo de red.

Use *kubenet* cuando:

- Tenga un espacio de direcciones IP limitado.
- La mayor parte de la comunicación de los pods se realice dentro del clúster.
- No necesite características avanzadas como los nodos virtuales o la directiva de red.

Use *Azure CNI* cuando:

- Tenga espacio de direcciones IP disponible.
- La mayor parte de la comunicación de los pods se realice con recursos fuera del clúster.
- No desee administrar los UDR.
- Necesite características avanzadas como los nodos virtuales o la directiva de red.

> [!NOTE]
> Kuberouter permite habilitar la directiva de red cuando se usa kubenet y puede instalarse como un daemonset en un clúster de AKS. Ten en cuenta kube-enrutador está aún en versión beta y no se admite es ofrecido por Microsoft para el proyecto.

## <a name="create-a-virtual-network-and-subnet"></a>Creación de una red virtual y una subred

Para empezar a usar *kubenet* y su propia subred de red virtual, primero cree un grupo de recursos mediante el comando [az group create][az-group-create]. En el ejemplo siguiente, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Si no tiene una subred y red virtual existentes para usarlas, cree estos recursos de red con el comando [az network vnet create][az-network-vnet-create]. En el ejemplo siguiente, la red virtual se denomina *myVnet* y tiene el prefijo de dirección de *10.0.0.0/8*. Se crea una subred llamada *myAKSSubnet* con el prefijo de dirección *10.240.0.0/16*.

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myAKSVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16
```

## <a name="create-a-service-principal-and-assign-permissions"></a>Creación de una entidad de servicio y asignación de permisos

Para permitir que un clúster de AKS interactúe con otros recursos de Azure, se usa una entidad de servicio de Azure Active Directory. La entidad de servicio debe tener permisos para administrar la red virtual y la subred que los nodos de AKS utilizan. Para crear una entidad de servicio, use el comando [az ad sp create-for-rbac][az-ad-sp-create-for-rbac]:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

En la salida de ejemplo siguiente se muestran la contraseña y el identificador de aplicación de la entidad de servicio. Estos valores se usan en pasos adicionales para asignar un rol a la entidad de servicio y después crear el clúster de AKS:

```console
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "476b3636-5eda-4c0e-9751-849e70b5cfad",
  "displayName": "azure-cli-2019-01-09-22-29-24",
  "name": "http://azure-cli-2019-01-09-22-29-24",
  "password": "a1024cd7-af7b-469f-8fd7-b293ecbb174e",
  "tenant": "72f998bf-85f1-41cf-92ab-2e7cd014db46"
}
```

Para asignar las delegaciones correctas en los pasos restantes, use los comandos [az network vnet show][az-network-vnet-show] y [az network vnet subnet show][az-network-vnet-subnet-show] para obtener los identificadores de recursos necesarios. Estos identificadores de recursos se almacenan como variables y se hace referencia a ellos en los pasos restantes:

```azurecli-interactive
VNET_ID=$(az network vnet show --resource-group myResourceGroup --name myAKSVnet --query id -o tsv)
SUBNET_ID=$(az network vnet subnet show --resource-group myResourceGroup --vnet-name myAKSVnet --name myAKSSubnet --query id -o tsv)
```

Ahora, asigne la entidad de servicio para los permisos de *colaborador* del clúster de AKS en la red virtual mediante el comando [az role assignment create][az-role-assignment-create]. Proporcionar su propia  *\<appId >* tal como se muestra en la salida del comando anterior para crear la entidad de servicio:

```azurecli-interactive
az role assignment create --assignee <appId> --scope $VNET_ID --role Contributor
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Creación de un clúster de AKS en la red virtual

Ya ha creado una red virtual y una subred y también ha creado y asignado permisos para que una entidad de servicio utilice dichos recursos de red. Ahora cree un clúster de AKS en la red virtual y la subred con el comando [az aks create][az-aks-create]. Definir su propia entidad de servicio  *\<appId >* y  *\<contraseña >*, tal y como se muestra en la salida del comando anterior para crear la entidad de servicio.

Los siguientes intervalos de direcciones IP también se definen como parte del proceso de creación del clúster:

* *--service-cidr* se usa para asignar servicios internos en una dirección IP del clúster de AKS. Este intervalo de direcciones IP debe ser un espacio de direcciones que no se use en ninguna otra parte del entorno de red. Esto incluye cualquier rango de red local si conecta, o pretende conectar, las redes virtuales de Azure mediante ExpressRoute o con una conexión VPN de sitio a sitio.

* La dirección *--dns-service-ip* debe ser la dirección *.10* del intervalo de direcciones IP del servicio.

* *--pod-cidr* debe ser un espacio de direcciones grande que no se use en ninguna otra parte del entorno de red. Esto incluye cualquier rango de red local si conecta, o pretende conectar, las redes virtuales de Azure mediante ExpressRoute o con una conexión VPN de sitio a sitio.
    * Este intervalo de direcciones debe ser lo suficientemente grande como para alojar el número de nodos hasta el que pretende escalar verticalmente. No puede cambiar este intervalo de direcciones una vez que el clúster se haya implementado, en caso de que necesite más direcciones para nodos adicionales.
    * El intervalo de direcciones IP para pods se usa para asignar un espacio de direcciones de */24* a cada nodo del clúster. En el ejemplo siguiente, *--pod-cidr* de *192.168.0.0/16* asigna el primer nodo *192.168.0.0/24*, el segundo nodo *192.168.1.0/24* y el tercer nodo *192.168.2.0/24*.
    * A medida que el clúster se escala o actualiza, la plataforma de Azure continúa asignando un intervalo de direcciones IP para pods a cada nuevo nodo.
    
* El *--dirección del puente de docker* permite que los nodos de AKS se comuniquen con la plataforma de administración subyacente. Esta dirección IP no debe estar dentro del rango de direcciones IP de red virtual del clúster, y no debe superponerse con otros rangos de direcciones en uso en la red.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --network-plugin kubenet \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --pod-cidr 192.168.0.0/16 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal <appId> \
    --client-secret <password>
```

Al crear un clúster de AKS, también se crean un grupo de seguridad de red y una tabla de ruta. Estos recursos de red administrados por el plano de control AKS. El grupo de seguridad de red se asocia automáticamente con la NIC virtuales en los nodos. La tabla de enrutamiento se asocia automáticamente con la subred de red virtual. Reglas del grupo de seguridad de red y las tablas de rutas y se actualizan automáticamente al crear y expone los servicios.

## <a name="next-steps"></a>Pasos siguientes

Con un clúster de AKS implementado en la subred de red virtual existente, ahora puede usar el clúster de forma habitual. Empiece a [crear aplicaciones con Azure Dev Spaces][dev-spaces] o a [utilizar Draft][use-draft] o [implemente aplicaciones con Helm][use-helm].

<!-- LINKS - External -->
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-network-concepts]: concepts-network.md
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[use-helm]: kubernetes-helm.md
[use-draft]: kubernetes-draft.md
[virtual-nodes]: virtual-nodes-cli.md
[vnet-peering]: ../virtual-network/virtual-network-peering-overview.md
[express-route]: ../expressroute/expressroute-introduction.md
