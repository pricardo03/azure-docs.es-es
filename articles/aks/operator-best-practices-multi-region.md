---
title: Alta disponibilidad y recuperación ante desastres en Azure Kubernetes Service (AKS)
description: Aprenda las prácticas recomendadas de un operador clúster para lograr el máximo tiempo de actividad para sus aplicaciones, proporciona alta disponibilidad y preparación para la recuperación ante desastres en Azure Kubernetes Service (AKS).
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 4afc1231e6c9fa49c04c7bf6dfe26ee5eb87cc31
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475172"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Procedimientos recomendados para continuidad empresarial y recuperación ante desastres en Azure Kubernetes Service (AKS)

A medida que administra los clústeres en Azure Kubernetes Service (AKS), el tiempo de actividad de la aplicación pasa a ser importante. AKS proporciona alta disponibilidad mediante el uso de varios nodos en un conjunto de disponibilidad. Pero varios de estos nodos no protegen el sistema de un error de región. Para maximizar el tiempo de actividad, planee con antelación para mantener la continuidad del negocio y prepararse para la recuperación ante desastres.

En este artículo se centra en cómo planear la continuidad del negocio y recuperación ante desastres en AKS. Aprenderá a:

> [!div class="checklist"]
> * Planear clústeres AKS en varias regiones.
> * Enrutar el tráfico entre varios clústeres con Azure Traffic Manager.
> * Usar la replicación geográfica para los registros de la imagen de contenedor.
> * Plan para el estado de la aplicación en varios clústeres.
> * Replicar el almacenamiento en varias regiones.

## <a name="plan-for-multiregion-deployment"></a>Plan para la implementación en varias regiones

**Procedimiento recomendado**: Al implementar varios clústeres AKS, elija regiones donde hay AKS y usar regiones emparejadas.

Un clúster de AKS se implementa en una sola región. Para proteger el sistema de error de región, implementar la aplicación en varios clústeres AKS en diferentes regiones. Al planear dónde se implementará el clúster de AKS, tenga en cuenta:

* [**Disponibilidad por región AKS**](https://docs.microsoft.com/azure/aks/quotas-skus-regions#region-availability): Elija regiones cerca de los usuarios. AKS continuamente se expande hacia nuevas regiones.
* [**Regiones emparejadas de Azure**](https://docs.microsoft.com/azure/best-practices-availability-paired-regions): Para su área geográfica, elija dos regiones que estén emparejadas entre sí. Regiones emparejadas coordinar las actualizaciones de plataforma y dar prioridad a los esfuerzos de recuperación cuando sea necesario.
* **Disponibilidad del servicio**: Decidir si las regiones emparejadas deben ser "n" y en caliente, activa/activa o activo o inactivo. ¿Desea ejecutar ambas regiones al mismo tiempo, con una región *listo* para empezar a obtener tráfico? O bien, ¿desea que una región de tienen tiempo para que esté listo para atender el tráfico?

Disponibilidad por región AKS y regiones emparejadas son una consideración común. Implemente los clústeres de AKS en regiones emparejadas diseñadas para administrar la recuperación ante desastres de las regiones de forma conjunta. Por ejemplo, AKS está disponible en este de EE.UU. y oeste de Estados Unidos. Estas regiones están emparejadas. Elija ambas regiones al crear una estrategia de BC/DR de AKS.

Al implementar la aplicación, agregue otro paso a la canalización de CI/CD para implementar en estas varios clústeres AKS. Si no actualiza las canalizaciones de implementación, las aplicaciones pueden implementarse en solo uno de sus regiones y los clústeres AKS. El tráfico de cliente que se dirige a una región secundaria no recibirá las actualizaciones más recientes de código.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Uso de Azure Traffic Manager para enrutar el tráfico

**Procedimiento recomendado**: Azure Traffic Manager puede dirigir a los clientes a su instancia de clúster y la aplicación más cercana de AKS. El mejor rendimiento y redundancia, dirigir todo el tráfico de aplicación a través de Traffic Manager antes de enviarlos a su clúster AKS.

Si tiene varios clústeres AKS en regiones diferentes, utilice el Administrador de tráfico para controlar cómo fluye el tráfico a las aplicaciones que se ejecutan en cada clúster. [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) es un equilibrador de carga de tráfico basado en DNS que puede distribuir el tráfico de red entre regiones. Use Traffic Manager para enrutar usuarios basado en tiempo de respuesta del clúster o en la ubicación geográfica.

![AKS con Traffic Manager](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Los clientes que tienen normalmente un único clúster AKS conectan con el nombre de dirección IP o DNS del servicio de una aplicación determinada. En una implementación multicluster, los clientes deben conectarse a un nombre DNS de Traffic Manager que apunte a los servicios en cada clúster de AKS. Estos servicios se definen mediante el uso de puntos de conexión de Traffic Manager. Cada punto de conexión es la *IP del equilibrador de carga del servicio*. Use esta configuración para dirigir el tráfico de red desde el punto de conexión de Traffic Manager en una región para el punto de conexión en una región distinta.

![Geográfico de enrutamiento a través de Traffic Manager](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Traffic Manager realiza las búsquedas de DNS y devuelve el punto de conexión más adecuado de un usuario. Perfiles anidados pueden dar prioridad a una ubicación principal. Por ejemplo, los usuarios generalmente deben conectarse a su región geográfica más cercana. Si esa región tiene un problema, Traffic Manager dirige en su lugar, los usuarios en una región secundaria. Este enfoque garantiza que los clientes pueden conectar a una instancia de la aplicación incluso si su región geográfica más cercana está disponible.

Para obtener información sobre cómo configurar los puntos de conexión y el enrutamiento, consulte [configurar el método de enrutamiento de tráfico geográfico con Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door-service"></a>Enrutamiento de aplicación de capa 7 con el servicio de puerta de entrada de Azure

Traffic Manager usa DNS (nivel 3) para el tráfico de forma. [Servicio de puerta delantera Azure](https://docs.microsoft.com/azure/frontdoor/front-door-overview) proporciona una opción de enrutamiento de HTTP/HTTPS (capa 7). Algunas características adicionales del servicio de puerta de entrada de Azure son SSL terminación, dominio personalizado, firewall de aplicaciones web, URL Rewrite y afinidad de sesión. Revise los requisitos del tráfico de su aplicación para saber qué solución es la más conveniente.

## <a name="enable-geo-replication-for-container-images"></a>Habilitar la replicación geográfica para las imágenes de contenedor

**Procedimiento recomendado**: Store las imágenes de contenedor en Azure Container Registry y replicación geográfica del registro para cada región AKS.

Para implementar y ejecutar sus aplicaciones en AKS, necesita una manera de almacenar y extraer las imágenes de contenedor. Container Registry se integra con AKS, por lo que puede almacenar de forma segura sus imágenes de contenedor o los gráficos de Helm. Container Registry admite replicación geográfica con varios maestros para replicar automáticamente las imágenes en las regiones de Azure en todo el mundo. 

Para mejorar el rendimiento y la disponibilidad, utilice la replicación geográfica de Container Registry para crear un registro en cada región donde haya un clúster de AKS. Cada clúster de AKS, a continuación, extrae imágenes de contenedor desde el registro de contenedor local de la misma región:

![Contenedor del registro la replicación geográfica para las imágenes de contenedor](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Cuando se usa la replicación geográfica de Container Registry para extraer imágenes desde la misma región, los resultados son:

* **Con mayor rapidez**: Extraiga imágenes desde las conexiones de red de alta velocidad y baja latencia en la misma región de Azure.
* **Más confiable**: Si una región no está disponible, el clúster de AKS extrae las imágenes de un registro de contenedor disponible.
* **Más barato**: No se genera ningún cargo por salida de red entre los centros de datos.

Replicación geográfica es una característica de *Premium* registros de contenedor SKU. Para obtener información sobre cómo configurar la replicación geográfica, consulte [replicación geográfica de Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).

## <a name="remove-service-state-from-inside-containers"></a>Quitar el estado de servicio de dentro de los contenedores

**Procedimiento recomendado**: Siempre que sea posible, no almacene el estado del servicio dentro del contenedor. En su lugar, use una plataforma de Azure como servicio (PaaS) que admite la replicación en varias regiones.

*Estado del servicio* hace referencia a los datos en memoria o en el disco que requiere un servicio de función. El estado incluye las variables de miembro y las estructuras de datos que el servicio lee y escribe. Dependiendo de cómo está diseñado el servicio, el estado también puede incluir archivos u otros recursos que se almacenan en el disco. Por ejemplo, el estado puede incluir los archivos de una base de datos que se utiliza para almacenar los registros de transacciones y datos.

Estado puede externalizar o colocado con el código que manipula el estado. Normalmente, externalizar estado mediante el uso de una base de datos u otro almacén de datos que se ejecuta en máquinas diferentes a través de la red o que se ejecuta fuera de proceso en el mismo equipo.

Contenedores y microservicios sean más resistentes cuando los procesos que se ejecutan dentro de ellos no conservan el estado. Porque las aplicaciones contienen casi siempre algún estado, usar una solución de PaaS como base de datos de Azure para MySQL, Azure Database for PostgreSQL o Azure SQL Database.

Para compilar aplicaciones portátiles, consulte las siguientes directrices:

* [La metodología de aplicación 12-factor](https://12factor.net/)
* [Ejecutar una aplicación web en varias regiones de Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Crear un plan de migración de almacenamiento

**Procedimiento recomendado**: Si usa Azure Storage, preparar y probar cómo migrar el almacenamiento de la región primaria a la región de copia de seguridad.

Sus aplicaciones pueden usar el almacenamiento de Azure para sus datos. Dado que las aplicaciones se reparten entre varios clústeres AKS en distintas regiones, debe mantener el almacenamiento sincronizado. Existen dos formas habituales de replicar el almacenamiento:

* Replicación asincrónica basada en la infraestructura
* Replicación asincrónica basada en la aplicación

### <a name="infrastructure-based-asynchronous-replication"></a>Replicación asincrónica basada en la infraestructura

Sus aplicaciones podrían requerir un almacenamiento persistente, incluso después de elimina un pod. En Kubernetes, puede usar los volúmenes persistentes para conservar el almacenamiento de datos. Volúmenes persistentes están montados en una máquina virtual del nodo y, a continuación, se expone a los pods. Volúmenes persistentes siguen pods incluso si los pods se mueven a otro nodo dentro del mismo clúster.

La estrategia de replicación que use depende de la solución de almacenamiento. Soluciones de almacenamiento comunes como [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [Ceph](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [torre](https://rook.io/docs/rook/master/disaster-recovery.html), y [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps) proporcionan sus propias instrucciones sobre la recuperación ante desastres y replicación.

La estrategia típica consiste en proporcionar un punto común de almacenamiento donde las aplicaciones puedan escribir sus datos. Estos datos se replican entre regiones y se accede a ellos de forma local.

![Replicación asincrónica basada en la infraestructura](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Si usa Azure Managed Disks, puede elegir la replicación y las soluciones de recuperación ante desastres como los siguientes:

* [Velero en Azure](https://github.com/heptio/velero/blob/master/site/docs/master/azure-config.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Replicación asincrónica basada en la aplicación

Kubernetes no proporciona actualmente ninguna implementación nativa para la replicación asincrónica basada en la aplicación. Dado que los contenedores y Kubernetes están acoplados holgadamente, debería funcionar cualquier enfoque tradicional de aplicación o lenguaje. Normalmente, las propias aplicaciones replican las solicitudes de almacenamiento, a continuación, se escriben en el almacenamiento de datos subyacente de cada clúster.

![Replicación asincrónica basada en la aplicación](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Pasos siguientes

En este artículo se centra en la continuidad del negocio y consideraciones sobre la recuperación ante desastres para los clústeres AKS. Para obtener más información acerca de las operaciones de clúster de AKS, consulte estos artículos sobre procedimientos recomendados:

* [Aislamiento de la arquitectura multiempresa y clúster][aks-best-practices-cluster-isolation]
* [Basic Kubernetes scheduler features][aks-best-practices-scheduler] (Características básicas de programadores de Kubernetes)

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
