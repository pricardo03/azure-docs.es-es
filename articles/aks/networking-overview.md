---
title: Configuración de red en Azure Kubernetes Service (AKS)
description: Obtenga información acerca de la configuración de red básica y avanzada en Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: marsma
ms.openlocfilehash: 1d7855ff840fc1dd68effb19c43c3a691bd15d62
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39714679"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuración de red en Azure Kubernetes Service (AKS)

Cuando se crea un clúster de Azure Kubernetes Service (AKS), puede seleccionar entre dos opciones de redes: **Básica** o **Avanzada**.

## <a name="basic-networking"></a>Red básica

La opción de redes **Básica** es la configuración predeterminada para la creación del clúster de AKS. La configuración de red del clúster, y sus pods, se administra completamente en Azure y es adecuada para implementaciones que no requieren la configuración de red virtual personalizada. Cuando selecciona la red Básica, no tiene control sobre la configuración de red, como las subredes o los intervalos de direcciones IP asignados al clúster.

Los nodos de un clúster de AKS configurado para usar la red Básica utilizan el complemento de Kubernetes [kubenet][kubenet].

## <a name="advanced-networking"></a>Redes avanzadas

La configuración de redes **Avanzada** coloca sus pods en una red virtual de Azure (VNet) configurada por el usuario, proporcionando a los pods conectividad automática con los recursos de la red virtual e integración con el amplio conjunto de funcionalidades que ofrecen las redes virtuales. Las redes avanzadas están disponibles al implementar clústeres de AKS con [Azure Portal][portal], con la CLI de Azure o con una plantilla de Resource Manager.

Los nodos de un clúster de AKS configurado para el uso de red Avanzada utilizan el complemento de Kubernetes [Azure Container Networking Interface (CNI)][cni-networking].

![Diagrama que muestra dos nodos con puentes que conectan cada uno a una única red virtual de Azure][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Características de redes avanzadas

Las redes avanzadas proporcionan las siguientes ventajas:

* Implementar el clúster de AKS en una red virtual existente o crear una nueva red virtual y subred para el clúster.
* Cada pod del clúster tiene asignada una dirección IP en la red virtual y puede comunicarse directamente con los otros pods del clúster y otros nodos de la red virtual.
* Un pod se puede conectar con otros servicios de una red virtual emparejada y con redes locales a través de ExpressRoute y conexiones VPN de sitio a sitio (S2S). Los pods también son accesibles desde el entorno local.
* Exponer un servicio de Kubernetes de forma externa o de forma interna mediante Azure Load Balancer. También es una característica de la red Básica.
* Los pods de una subred que tienen puntos de conexión de servicio habilitados pueden conectarse de forma segura a los servicios de Azure, como Azure Storage o SQL Database.
* Usar rutas definidas por el usuario (UDR) para redirigir el tráfico desde los pods a una aplicación virtual de red.
* Los pods pueden acceder a recursos públicos de Internet. También es una característica de la red Básica.

## <a name="advanced-networking-prerequisites"></a>Requisitos previos de redes avanzadas

* La red virtual del clúster AKS debe permitir la conectividad saliente de internet.
* No cree más de un clúster AKS en la misma subred.
* Los clústeres AKS no pueden usar `169.254.0.0/16`, `172.30.0.0/16` o `172.31.0.0/16` para el intervalo de direcciones del servicio de Kubernetes.
* La entidad de servicio usada por el clúster de AKS debe tener al menos permisos de [colaborador de la red](../role-based-access-control/built-in-roles.md#network-contributor) en la subred de la red virtual. Si quiere definir un [rol personalizado](../role-based-access-control/custom-roles.md) en lugar de usar el rol integrado de colaborador de red, se requieren los permisos siguientes:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Planeamiento de direccionamiento IP del clúster

Los clústeres configurados con redes avanzadas requieren planeamiento adicional. El tamaño de la red virtual y su subred debe ajustarse al número de pods que tiene previsto ejecutar, así como al número de nodos del clúster.

Se asignan direcciones IP para los pods y los nodos del clúster desde la subred especificada dentro de la red virtual. Cada nodo se configura con una dirección IP principal, que es la dirección IP del nodo, y 30 direcciones IP adicionales preconfiguradas por Azure CNI que se asignan a los pods programados para el nodo. Cuando se escala horizontalmente el clúster, cada nodo del mismo modo se configura de modo similar con direcciones IP de la subred.

El plan de direcciones IP de un clúster AKS consta de una red virtual, al menos una subred para los nodos y pods, y un intervalo de direcciones del servicio de Kubernetes.

| Intervalo de direcciones / recurso de Azure | Límites y tamaño |
| --------- | ------------- |
| Virtual network | La red virtual de Azure puede ser tan grande como /8, pero solo puede tener 16 000 direcciones IP configuradas. |
| Subred | Debe ser lo suficientemente grande como para dar cabida a los nodos, los pods y a todos los recursos de Kubernetes y de Azure que se pueden aprovisionar en el clúster. Por ejemplo, si implementa una instancia de Azure Load Balancer interna, sus direcciones IP de front-end se asignan desde la subred del clúster, no las direcciones IP públicas. <p/>Para calcular el tamaño *mínimo* de la subred: `(number of nodes) + (number of nodes * pods per node)` <p/>Ejemplo de un clúster de 50 nudos: `(50) + (50 * 30) = 1,550` (/21 o superior) |
| Intervalo de direcciones del servicio de Kubernetes | Este intervalo no lo debe usar ningún elemento de red de esta red virtual o que esté conectado a ella. El CIDR de la dirección del servicio debe ser menor que /12. |
| Dirección IP del servicio DNS de Kubernetes | Dirección IP del intervalo de direcciones del servicio de Kubernetes que se usará en la detección de servicios de clúster (kube-dns). |
| Dirección de puente de Docker | Dirección IP (en notación CIDR) que se usa como la dirección IP del puente de Docker en los nodos. El valor predeterminado es 172.17.0.1/16. |

Cada red virtual aprovisionada para su uso con el complemento Azure CNI está limitada a **16 000 direcciones IP configuradas**.

## <a name="maximum-pods-per-node"></a>Pods máximos por nodo

El número máximo predeterminado de pods por nodo en un clúster de AKS varía entre las redes básicas y las redes avanzadas y el método de implementación del clúster.

### <a name="default-maximum"></a>Valor máximo predeterminado

* Redes básicas: **110 pods por nodo**
* Redes avanzadas **30 pods por nodo**

### <a name="configure-maximum"></a>Configuración del valor máximo

Según el método de la implementación, es posible que pueda modificar el número máximo de pods por nodo en un clúster de AKS.

* **CLI de Azure**: especifique el argumento `--max-pods` cuando implemente un clúster con el comando [az aks create][az-aks-create].
* **Plantilla de Resource Manager**: especifique la propiedad `maxPods` del objeto [ManagedClusterAgentPoolProfile] cuando implemente un clúster con una plantilla de Resource Manager.
* **Azure Portal**: no puede modificar el número máximo de pods por nodo cuando implemente un clúster con Azure Portal. Los clústeres de redes avanzadas están limitados a 30 pods por nodo cuando se implementan en Azure Portal.

## <a name="deployment-parameters"></a>Parámetros de implementación

Cuando crea un clúster de AKS, los parámetros siguientes son configurables para redes avanzadas:

**Red virtual**: la red virtual en la que desea implementar el clúster de Kubernetes. Si desea crear una red virtual nueva para el clúster, seleccione *Crear nueva* y siga los pasos descritos en la sección *Creación de red virtual*. La red virtual está limitada a 16 000 direcciones IP configuradas.

**Subred**: la subred dentro de la red virtual en la que desea implementar el clúster. Si desea crear una nueva subred en la red virtual para el clúster, seleccione *Crear nueva* y siga los pasos descritos en la sección *Creación de subred*.

**Intervalo de direcciones de servicio de Kubernetes**: este es el conjunto de direcciones IP virtuales que Kubernetes asigna a [servicios][services] en el clúster. Puede usar cualquier intervalo de direcciones privado que cumpla los requisitos siguientes:

* No debe estar dentro del intervalo de direcciones IP de la red virtual del clúster.
* No debe superponerse con otras redes virtuales con las que la red virtual del clúster esté emparejada.
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

* *¿Cómo se pueden configurar propiedades adicionales para la subred que he creado durante la creación del clúster de AKS? Por ejemplo, los puntos de conexión de servicio.*

  La lista completa de propiedades de la red virtual y las subredes que se crean durante la creación del clúster de AKS puede configurarse en la página de configuración de red virtual estándar en Azure Portal.

* *¿Puedo usar una subred diferente dentro de mi red virtual de clúster como* **intervalo de direcciones de servicio de Kubernetes**?

  Aunque no se recomienda, esta configuración es posible. El intervalo de direcciones de servicio es un conjunto de direcciones IP virtuales (VIP) que Kubernetes asigna a los servicios del clúster. Las redes de Azure no tiene ninguna visibilidad sobre el intervalo de direcciones IP de servicio del clúster de Kubernetes. Debido a ello, es posible crear posteriormente una nueva subred en la red virtual del clúster que se superponga con este intervalo. Si se produce una superposición de este tipo, Kubernetes podría asignar a un servicio una dirección IP que ya esté en uso por otro recurso de la subred, lo que provocaría un comportamiento impredecible o errores. Al tener la seguridad de usar un intervalo de direcciones que se encuentra fuera de la red virtual del clúster, puede evitar este riesgo de superposición.

## <a name="next-steps"></a>Pasos siguientes

### <a name="networking-in-aks"></a>Redes en AKS

Más información acerca de las redes en AKS en los siguientes artículos:

[Uso de una dirección IP estática con el equilibrador de carga de Azure Kubernetes Service (AKS)](static-ip.md)

[Entrada HTTPS en Azure Container Service (AKS)](ingress.md)

[Uso de un equilibrador de carga interno con Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>ACS Engine

[Azure Container Service Engine (ACS Engine)][acs-engine] es un proyecto de código abierto que genera plantillas de Azure Resource Manager que puede usar para implementar clústeres habilitados para Docker en Azure. Se pueden implementar los orquestadores Kubernetes, DC/OS, Swarm Mode y Swarm con ACS Engine.

Los clústeres de Kubernetes creados con ACS Engine admiten los complementos [kubenet][kubenet] y [Azure CNI][cni-networking]. Por lo tanto, los escenarios de redes básico y avanzado son compatibles con ACS Engine.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: aks-ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
