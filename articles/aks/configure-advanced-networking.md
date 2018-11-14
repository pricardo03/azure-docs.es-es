---
title: Configurar redes avanzadas en Azure Kubernetes Service (AKS)
description: Aprenda a configurar una red avanzada en Azure Kubernetes Service (AKS), incluida la implementación de un clúster de AKS en una red virtual existente y una subred.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/11/2018
ms.author: iainfou
ms.openlocfilehash: 289aa893a0ffa598d5b9fae67a81e9bf0c9782f7
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014404"
---
# <a name="configure-advanced-networking-in-azure-kubernetes-service-aks"></a>Configurar redes avanzadas en Azure Kubernetes Service (AKS)

De forma predeterminada, los clústeres de AKS usan redes *básicas*, que crean y configuran una red virtual y una subred para su uso con el clúster. Para un mayor control de estas opciones de redes, como los intervalos IP, puede usar redes *avanzadas* en su lugar. Con las redes avanzadas, también puede crear un clúster de AKS en una red virtual existente y una subred. Esta red virtual existente suele proporcionar conectividad a una red local mediante Azure ExpressRoute o VPN de sitio a sitio.

En este artículo se muestra cómo usar las redes avanzadas para crear y usar una red virtual con un clúster de AKS. Para obtener información más general sobre redes, consulte el artículo sobre los [conceptos de red para Kubernetes y AKS][aks-network-concepts].

## <a name="prerequisites"></a>Requisitos previos

* La red virtual del clúster AKS debe permitir la conectividad saliente de Internet.
* No cree más de un clúster AKS en la misma subred.
* Los clústeres AKS no pueden usar `169.254.0.0/16`, `172.30.0.0/16` o `172.31.0.0/16` para el intervalo de direcciones del servicio de Kubernetes.
* La entidad de servicio usada por el clúster de AKS debe tener al menos permisos de [colaborador de la red](../role-based-access-control/built-in-roles.md#network-contributor) en la subred de la red virtual. Si quiere definir un [rol personalizado](../role-based-access-control/custom-roles.md) en lugar de usar el rol integrado de colaborador de red, se requieren los permisos siguientes:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Planeamiento de direccionamiento IP del clúster

Los clústeres configurados con redes avanzadas requieren planeamiento adicional. Los tamaños de red virtual y de subred deben ajustarse al número de pods que tiene previsto ejecutar, así como al número de nodos del clúster.

Se asignan direcciones IP para los pods y los nodos del clúster desde la subred especificada dentro de la red virtual. Cada nodo se configura con una dirección IP principal. De forma predeterminada, Azure CNI configura previamente treinta direcciones IP adicionales que se asignan a los pods programados en el nodo. Cuando se escala horizontalmente el clúster, cada nodo del mismo modo se configura de modo similar con direcciones IP de la subred. También puede ver el [número máximo de pods por nodo](#maximum-pods-per-node).

> [!IMPORTANT]
> El número de direcciones IP necesarias debe incluir consideraciones sobre las operaciones de actualización y escalabilidad. Si establece el intervalo de direcciones IP para que solo se admita un número fijo de nodos, no se podrá actualizar o escalar el clúster.
>
> - Al **actualizar** el clúster de AKS, se implementa un nuevo nodo en el clúster. Los servicios y las cargas de trabajo comienzan a ejecutarse en el nuevo nodo y el nodo antiguo se quita del clúster. Para realizar este proceso de actualización gradual es necesario que haya disponible un bloque adicional de direcciones IP. El recuento de nodos es entonces `n + 1`.
>
> - Al **escalar** un clúster de AKS, se implementa un nuevo nodo en el clúster. Los servicios y las cargas de trabajo comienzan a ejecutarse en el nuevo nodo. El intervalo de direcciones IP debe tener en cuenta cómo podría escalar verticalmente el número de nodos y los pods que el clúster puede admitir. También se debe incluir un nodo adicional para las operaciones de actualización. El recuento de nodos es entonces `n + number-of-additional-scaled-nodes-you-anticipate + 1`.

Si espera que los nodos ejecuten el número máximo de pods, y destruye e implementa pods con regularidad, también debe contar con algunas direcciones IP adicionales por nodo. Estas direcciones IP adicionales tienen en cuenta que la eliminación de un servicio y la implementación de la dirección IP liberada para un nuevo servicio pueden tardar unos segundos, y adquieren la dirección.

El plan de direcciones IP de un clúster AKS consta de una red virtual, al menos una subred para los nodos y pods, y un intervalo de direcciones del servicio de Kubernetes.

| Intervalo de direcciones / recurso de Azure | Límites y tamaño |
| --------- | ------------- |
| Virtual network | El tamaño de Azure Virtual Network puede ser de /8, pero se limita a 65 536 direcciones IP configuradas. |
| Subred | Debe ser lo suficientemente grande como para dar cabida a los nodos, los pods y a todos los recursos de Kubernetes y de Azure que se pueden aprovisionar en el clúster. Por ejemplo, si implementa una instancia de Azure Load Balancer interna, sus direcciones IP de front-end se asignan desde la subred del clúster, no las direcciones IP públicas. El tamaño de la subred también debe tener en cuenta las operaciones de actualización o las necesidades futuras de escalabilidad.<p />Para calcular el tamaño *mínimo* de subred, incluido un nodo adicional para operaciones de actualización: `(number of nodes + 1) + ((number of nodes + 1) * maximum pods per node that you configure)`<p/>Ejemplo de un clúster de 50 nudos: `(51) + (51  * 30 (default)) = 1,581` (/21 o superior)<p/>Ejemplo de un clúster de 50 nodos que también incluye aprovisionamiento para escalar verticalmente 10 nodos adicionales: `(61) + (61 * 30 (default)) = 2,440` (/20 o superior)<p>Si no especifica un número máximo de pods por nodo al crear el clúster, el número máximo de pods por nodo se establece en *30*. El número mínimo de direcciones IP requeridas se basa en ese valor. Si se calculan los requisitos mínimos de dirección IP según otro valor máximo distinto, vea [cómo configurar el número máximo de pods por nodo](#configure-maximum---new-clusters) para establecer este valor cuando se implementa el clúster. |
| Intervalo de direcciones del servicio de Kubernetes | Este intervalo no lo debe usar ningún elemento de red de esta red virtual o que esté conectado a ella. El CIDR de la dirección del servicio debe ser menor que /12. |
| Dirección IP del servicio DNS de Kubernetes | Dirección IP del intervalo de direcciones del servicio de Kubernetes que se usará en la detección de servicios de clúster (kube-dns). |
| Dirección de puente de Docker | Dirección IP (en notación CIDR) que se usa como la dirección IP del puente de Docker en los nodos. El valor predeterminado es 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Pods máximos por nodo

El número máximo de pods por nodo en un clúster de AKS es 110. El número máximo *predeterminado* de pods por nodo varía entre las redes básicas y las redes avanzadas y el método de implementación del clúster.

| Método de implementación | Predeterminado básico | Predeterminado avanzado | Configurable en la implementación |
| -- | :--: | :--: | -- |
| Azure CLI | 110 | 30 | Sí (hasta 110) |
| Plantilla de Resource Manager | 110 | 30 | Sí (hasta 110) |
| Portal | 110 | 30 | Sin  |

### <a name="configure-maximum---new-clusters"></a>Configurar máximo: nuevos clústeres

Puede configurar el número máximo de pods por nodo *solo en tiempo de implementación del clúster*. Si realiza la implementación con la CLI de Azure o con una plantilla de Resource Manager, puede establecer un valor máximo de pods por nodo de hasta 110.

* **CLI de Azure**: especifique el argumento `--max-pods` cuando implemente un clúster con el comando [az aks create][az-aks-create]. El valor máximo es 110.
* **Plantilla de Resource Manager**: especifique la propiedad `maxPods` del objeto [ManagedClusterAgentPoolProfile] cuando implemente un clúster con una plantilla de Resource Manager. El valor máximo es 110.
* **Azure Portal**: no puede modificar el número máximo de pods por nodo cuando implemente un clúster con Azure Portal. Los clústeres de redes avanzadas están limitados a 30 pods por nodo cuando se implementan mediante Azure Portal.

### <a name="configure-maximum---existing-clusters"></a>Configurar máximo: clústeres existentes

No se puede cambiar el número máximo de pods por nodo en un clúster de AKS existente. Solo puede ajustar el número cuando se implementa inicialmente el clúster.

## <a name="deployment-parameters"></a>Parámetros de implementación

Cuando crea un clúster de AKS, los parámetros siguientes son configurables para redes avanzadas:

**Red virtual**: la red virtual en la que desea implementar el clúster de Kubernetes. Si desea crear una red virtual nueva para el clúster, seleccione *Crear nueva* y siga los pasos descritos en la sección *Creación de red virtual*. Para información acerca de límites y cuotas para Azure Virtual Network, vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Subred**: la subred dentro de la red virtual en la que desea implementar el clúster. Si desea crear una nueva subred en la red virtual para el clúster, seleccione *Crear nueva* y siga los pasos descritos en la sección *Creación de subred*. Para la conectividad híbrida, el intervalo de direcciones no debe solaparse con ninguna otra red virtual de su entorno.

**Intervalo de direcciones de servicio de Kubernetes**: este es el conjunto de direcciones IP virtuales que Kubernetes asigna a [servicios][services] en el clúster. Puede usar cualquier intervalo de direcciones privado que cumpla los requisitos siguientes:

* No debe estar dentro del intervalo de direcciones IP de la red virtual del clúster.
* No deben superponerse con ninguna otra red virtual del mismo nivel que la red virtual del clúster.
* No debe superponerse con ninguna dirección IP local.
* No debe estar dentro de los intervalos `169.254.0.0/16`, `172.30.0.0/16` o `172.31.0.0/16`

Aunque es técnicamente posible especificar un intervalo de direcciones de servicio en la misma red virtual que el clúster, no se recomienda. Puede producirse un comportamiento impredecible si se usan intervalos IP superpuestos. Para más información, consulte la sección [P+F](#frequently-asked-questions) de este artículo. Para más información sobre los servicios de Kubernetes, consulte [Servicios][services] en la documentación de Kubernetes.

**Dirección IP del servicio DNS de Kubernetes**: la dirección IP del servicio DNS del clúster. Esta dirección debe estar dentro del  *intervalo de direcciones del servicio Kubernetes*.

**Dirección del puente de Docker**: la dirección IP y la máscara de red que se van a asignar al puente de Docker. Esta dirección IP no debe estar dentro del intervalo de direcciones IP de la red virtual del clúster.

## <a name="configure-networking---cli"></a>Configuración de redes: CLI

Al crear un clúster de AKS con la CLI de Azure, también puede configurar redes avanzadas. Use los siguientes comandos para crear un nuevo clúster de AKS con las características de red avanzadas habilitadas.

En primer lugar, obtenga el identificador de recursos de subred para la subred existente a la que se unirá el clúster de AKS:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Use el comando [crear az aks][az-aks-create] con el argumento `--network-plugin azure` para crear un clúster con redes avanzadas. Actualice el valor `--vnet-subnet-id` con el identificador de subred recopilado en el paso anterior:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Configuración de redes: portal

La siguiente captura de pantalla de Azure Portal muestra un ejemplo de la configuración de estos valores durante la creación del clúster de AKS:

![Configuración de redes avanzada en Azure Portal][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Las siguientes preguntas y respuestas se aplican a la configuración de red **Avanzada**.

* *¿Puedo implementar máquinas virtuales en la subred del clúster?*

  No. No se admite la implementación de máquinas virtuales en la subred usada por el clúster Kubernetes. Las máquinas virtuales se pueden implementar en la misma red virtual, pero en una subred diferente.

* *¿Puedo configurar las directivas de red por pod?*

  No. Las directivas de redes por pod no se admiten actualmente.

* *¿Es configurable el número máximo de pods que se puede implementar en un nodo ?*

  Sí, al implementar un clúster con la CLI de Azure o una plantilla de Resource Manager. Consulte [Pods máximos por nodo](#maximum-pods-per-node).

  No se puede cambiar el número máximo de pods por nodo en un clúster existente.

* *¿Cómo se pueden configurar propiedades adicionales para la subred que he creado durante la creación del clúster de AKS? Por ejemplo, los puntos de conexión de servicio.*

  La lista completa de propiedades de la red virtual y las subredes que se crean durante la creación del clúster de AKS puede configurarse en la página de configuración de red virtual estándar en Azure Portal.

* *¿Puedo usar una subred diferente dentro de mi red virtual de clúster como* **intervalo de direcciones de servicio de Kubernetes**?

  Aunque no se recomienda, esta configuración es posible. El intervalo de direcciones de servicio es un conjunto de direcciones IP virtuales (VIP) que Kubernetes asigna a los servicios del clúster. Las redes de Azure no tiene ninguna visibilidad sobre el intervalo de direcciones IP de servicio del clúster de Kubernetes. Debido a ello, es posible crear posteriormente una nueva subred en la red virtual del clúster que se superponga con el intervalo de direcciones de servicio. Si se produce una superposición de este tipo, Kubernetes podría asignar a un servicio una dirección IP que ya esté en uso por otro recurso de la subred, lo que provocaría un comportamiento impredecible o errores. Al tener la seguridad de usar un intervalo de direcciones que se encuentra fuera de la red virtual del clúster puede evitar este riesgo de superposición.

## <a name="next-steps"></a>Pasos siguientes

### <a name="networking-in-aks"></a>Redes en AKS

Más información acerca de las redes en AKS en los siguientes artículos:

- [Uso de una dirección IP estática con el equilibrador de carga de Azure Kubernetes Service (AKS)](static-ip.md)
- [Uso de un equilibrador de carga interno con Azure Container Service (AKS)](internal-lb.md)

- [Crear un controlador de entrada básico con conectividad de red externa][aks-ingress-basic]
- [Habilitar el complemento de enrutamiento de aplicación HTTP][aks-http-app-routing]
- [Crear un controlador de entrada que usa una red privada interna y una dirección IP][aks-ingress-internal]
- [Crear un controlador de entrada con una dirección IP pública dinámica y configurar Let's Encrypt para generar certificados TLS de forma automática][aks-ingress-tls]
- [Crear un controlador de entrada con una dirección IP pública estática y configurar Let's Encrypt para generar certificados TLS de forma automática][aks-ingress-static-tls]

### <a name="acs-engine"></a>ACS Engine

[Azure Container Service Engine (ACS Engine)][acs-engine] es un proyecto de código abierto que genera plantillas de Azure Resource Manager que puede usar para implementar clústeres habilitados para Docker en Azure. Se pueden implementar los orquestadores Kubernetes, DC/OS, Swarm Mode y Swarm con ACS Engine.

Los clústeres de Kubernetes creados con ACS Engine admiten los complementos [kubenet][kubenet] y [Azure CNI][cni-networking]. Por lo tanto, los escenarios de redes básico y avanzado son compatibles con ACS Engine.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md
