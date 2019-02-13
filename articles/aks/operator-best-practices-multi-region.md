---
title: 'Procedimientos recomendados de operador: alta disponibilidad y recuperación ante desastres en Azure Kubernetes Services (AKS)'
description: Conozca los procedimientos recomendados del operador del clúster para conseguir el máximo tiempo de actividad de las aplicaciones, a fin de ofrecer una alta disponibilidad y prepararse para situaciones de recuperación ante desastres en Azure Kubernetes Service (AKS).
services: container-service
author: lastcoolnameleft
ms.service: container-service
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: lastcoolnameleft
ms.openlocfilehash: 9958f5f0f1435af231c1426a249c745f4a2352c5
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55816618"
---
# <a name="best-practices-for-business-continuity-and-disaster-recovery-in-azure-kubernetes-service-aks"></a>Procedimientos recomendados para continuidad empresarial y recuperación ante desastres en Azure Kubernetes Service (AKS)

A medida que administra los clústeres en Azure Kubernetes Service (AKS), el tiempo de actividad de la aplicación pasa a ser importante. AKS proporciona alta disponibilidad mediante el uso de varios nodos en un conjunto de disponibilidad. Todos estos nodos no ofrecen protección frente a errores en una región. Para maximizar el tiempo de actividad, implemente algunas características de continuidad empresarial y recuperación ante desastres.

El artículo de procedimientos recomendados se centra en las consideraciones que facilitan el planeamiento de continuidad empresarial y recuperación ante desastres en AKS. Aprenderá a:

> [!div class="checklist"]
* Planear clústeres de AKS en varias regiones
* Enrutar el tráfico entre varios clústeres con Azure Traffic Manager
* Usar la replicación geográfica para los registros de imágenes de contenedor
* Planear el estado de la aplicación entre varios clústeres
* Replicar el almacenamiento en varias regiones

## <a name="plan-for-multi-region-deployment"></a>Planeamiento de la implementación en varias regiones

**Guía de procedimientos recomendados**: al implementar varios clústeres de AKS, elija las regiones donde AKS está disponible y use regiones emparejadas.

Un clúster de AKS se implementa en una sola región. Para protegerse frente a errores en una región, implemente la aplicación en varios clústeres de AKS en diferentes regiones. Cuando planee en qué regiones implementar el clúster de AKS, debe aplicar las siguientes consideraciones:

* [Disponibilidad en regiones de AKS](https://docs.microsoft.com/azure/aks/container-service-quotas#region-availability)
  * Elija regiones cerca de los usuarios. AKS se expande continuamente a nuevas regiones.
* [Regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)
  * Para su área geográfica, elija dos regiones que estén emparejadas entre sí. Estas regiones coordinan las actualizaciones de la plataforma y dan prioridad a los esfuerzos de recuperación cuando resulta necesario.
* Nivel de disponibilidad de servicio (frecuente/frecuente, frecuente/flexible, frecuente/en frío)
  * Desea ejecutar ambas regiones al mismo tiempo, con una región *preparada* para empezar a atender el tráfico o una región que necesita tiempo para prepararse para atender el tráfico.

La disponibilidad por región de AKS y las regiones emparejadas son una consideración conjunta. Implemente los clústeres de AKS en regiones emparejadas diseñadas para administrar la recuperación ante desastres de las regiones de forma conjunta. Por ejemplo, AKS está disponible en *Este de EE. UU.* y *Oeste de EE. UU.*. Estas regiones también están emparejadas. Estas dos regiones serían recomendables al crear una estrategia de continuidad empresarial y recuperación ante desastres en AKS.

Al implementar la aplicación, también debe agregar otro paso a la canalización de CI/CD para implementarlo en estos varios clústeres de AKS. Si no actualiza las canalizaciones de la implementación, puede que las implementaciones de la aplicación solo se implementen en una de las regiones y los clústeres de AKS. El tráfico de cliente que se dirige a una región secundaria no recibirá las actualizaciones más recientes de código.

## <a name="use-azure-traffic-manager-to-route-traffic"></a>Uso de Azure Traffic Manager para enrutar el tráfico

**Guía de procedimientos recomendados**: Azure Traffic Manager puede remitir a los clientes a la instancia de la aplicación y los clústeres de AKS más próximos. Para conseguir los máximos niveles de rendimiento y redundancia, dirija todo el tráfico de la aplicación a través de Traffic Manager antes de que pase al clúster de AKS.

Con varios clústeres de AKS en distintas regiones, necesita controlar cómo se dirige el tráfico a las aplicaciones que se ejecutan en cada clúster. [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/) es un equilibrador de carga de tráfico basado en DNS que puede distribuir el tráfico de red entre regiones. Puede enrutar a los usuarios en función del tiempo de respuesta del clúster o de la ubicación geográfica.

![AKS con Azure Traffic Manager](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

Con un único clúster de AKS, los clientes suelen conectarse a la *IP de servicio* o al nombre DNS de una aplicación determinada. En una implementación de varios clústeres, los clientes deben conectarse a un nombre DNS de Traffic Manager que apunte a los servicios en cada clúster de AKS. Estos servicios se definen mediante los puntos de conexión de Traffic Manager. Cada punto de conexión es la *IP del equilibrador de carga del servicio*. Esta configuración le permite dirigir el tráfico de red desde el punto de conexión de Traffic Manager de una región al punto de conexión de una región distinta.

![Enrutamiento geográfico con Azure Traffic Manager](media/operator-best-practices-bc-dr/traffic-manager-geographic-routing.png)

Traffic Manager se utiliza para realizar las búsquedas DNS y devolver el punto de conexión más adecuado para un usuario. Se pueden utilizar perfiles anidados, con una prioridad determinada para una ubicación principal. Por ejemplo, un usuario principalmente debe conectarse a su región geográfica más cercana. Si esa región tiene un problema, Traffic Manager, en su lugar, dirige el tráfico a una región secundaria. Este enfoque garantiza que los clientes puedan conectarse siempre a la instancia de una aplicación, incluso aunque su región geográfica más próxima no esté disponible.

Para obtener información sobre cómo configurar estos puntos de conexión y el enrutamiento, consulte [Configuración del método de enrutamiento de tráfico geográfico con Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-geographic-routing-method).

### <a name="layer-7-application-routing-with-azure-front-door"></a>Enrutamiento de aplicación de capa 7 con Azure Front Door

Azure Traffic Manager usa DNS (nivel 3) para dar forma al tráfico. [Azure Front Door (versión preliminar)](https://docs.microsoft.com/azure/frontdoor/front-door-overview) proporciona una opción de enrutamiento HTTP/HTTPS (capa 7). Las características adicionales de Front Door incluyen terminación SSL, dominio personalizado, firewall de aplicaciones web, Reescritura de dirección URL y afinidad de sesión.

Revise los requisitos del tráfico de su aplicación para saber qué solución es la más conveniente.

## <a name="enable-geo-replication-for-container-images"></a>Habilitar la replicación geográfica para las imágenes de contenedor

**Guía de procedimientos recomendados**: almacene las imágenes de contenedor en Azure Container Registry y replique geográficamente el registro en cada región de AKS.

Para implementar y ejecutar sus aplicaciones en AKS, necesita una manera de almacenar y extraer las imágenes de contenedor. Azure Container Registry (ACR) puede integrarse con AKS para almacenar de forma segura sus imágenes de contenedor o los gráficos de Helm. ACR admite la replicación geográfica de arquitectura multimaestro para replicar automáticamente las imágenes en las regiones de Azure de todo el mundo. Para mejorar el rendimiento y la disponibilidad, utilice la replicación geográfica de ACR para crear un registro en cada región donde haya un clúster de AKS. Cada clúster de AKS, a continuación, extrae imágenes de contenedor del registro de ACR local en la misma región:

![Replicación geográfica de Azure Container Registry para imágenes de contenedor](media/operator-best-practices-bc-dr/acr-geo-replication.png)

Las ventajas del uso de la replicación geográfica de ACR incluyen:

* **La extracción de imágenes de la misma región es más rápida**. Puede extraer imágenes de conexiones de red de alta velocidad y baja latencia dentro de la misma región de Azure.
* **La extracción de imágenes de la misma región es más confiable**. Si una región no está disponible, el clúster de AKS extrae la imagen desde un registro de ACR diferente que sigue estando disponible.
* **La extracción de imágenes de la misma región es más económica**. No se genera ningún cargo por salida de red entre los centros de datos.

La replicación geográfica es una característica de los registros de ACR de SKU *Premium*. Para obtener instrucciones sobre cómo configurar la replicación, vea [Replicación geográfica en Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).

## <a name="remove-service-state-from-inside-containers"></a>Quitar el estado de servicio de dentro de los contenedores

**Guía de procedimientos recomendados**: cuando sea posible, no almacene el estado de servicio dentro del contenedor. En su lugar, use los servicios PaaS de Azure que admiten la replicación en varias regiones.

Estado de servicio hace referencia a los datos en memoria o en disco que el servicio necesita para funcionar. El estado incluye las variables de miembro y las estructuras de datos que el servicio lee y escribe. Dependiendo del diseño del servicio, el estado también puede incluir archivos u otros recursos que se almacenan en discos. Por ejemplo, los archivos que usaría una base de datos para almacenar los registros de transacciones y de datos.

El estado se puede externalizar o colocar con el código que está manipulando el estado. La externalización de estado normalmente se realiza mediante una base de datos u otro almacén de datos que se ejecutan en distintas máquinas de la red o fuera del proceso de la misma máquina.

Los contenedores y microservicios son más resistentes si los procesos que se ejecutan dentro de ellos no conservan el estado. Dado que las aplicaciones contienen casi siempre algún estado, use una solución de plataforma como servicio, como Azure Database for MySQL/Postgres o Azure SQL.

Para más información sobre cómo compilar aplicaciones que sean más portables, vea las siguientes directrices:

* [La metodología de las aplicaciones con doce factores](https://12factor.net/)
* [Ejecución de una aplicación web en varias regiones de Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/multi-region)

## <a name="create-a-storage-migration-plan"></a>Crear un plan de migración de almacenamiento

**Guía de procedimientos recomendados**: si usa Azure Storage, prepare y pruebe cómo migrar el almacenamiento desde la región principal a la región de copia de seguridad.

Las aplicaciones pueden utilizar Azure Storage para sus datos. Como las aplicaciones se extienden entre varios clústeres de AKS en distintas regiones, debe mantener sincronizado el almacenamiento. Dos formas comunes de replicar el almacenamiento incluyen los siguientes enfoques:

* Replicación asincrónica basada en la aplicación
* Replicación asincrónica basada en la infraestructura

### <a name="infrastructure-based-asynchronous-replication"></a>Replicación asincrónica basada en la infraestructura

Las aplicaciones pueden requerir almacenamiento persistente, incluso después de eliminar un pod. En Kubernetes, puede usar los volúmenes persistentes para conservar el almacenamiento de datos. Estos volúmenes persistentes están montados en la máquina virtual del nodo y, a continuación, se exponen a los pods. Los volúmenes persistentes realizan el seguimiento de pods, aunque estos se muevan a un nodo diferente dentro del mismo clúster.

Las estrategias de replicación pueden variar en función de la solución de almacenamiento utilizada. Las soluciones de almacenamiento comunes, como [Gluster](https://docs.gluster.org/en/latest/Administrator%20Guide/Geo%20Replication/), [CEPH](http://docs.ceph.com/docs/master/cephfs/disaster-recovery/), [Rook](https://rook.io/docs/rook/master/disaster-recovery.html) y [Portworx](https://docs.portworx.com/scheduler/kubernetes/going-production-with-k8s.html#disaster-recovery-with-cloudsnaps), tienen sus propios procedimientos.

El enfoque central es un punto de almacenamiento común para que las aplicaciones escriban sus datos. Estos datos se replican entre regiones y se accede a ellos de forma local.

![Replicación asincrónica basada en la infraestructura](media/operator-best-practices-bc-dr/aks-infra-based-async-repl.png)

Si usa Azure Managed Disks, las soluciones disponibles de replicación y recuperación ante desastres incluyen el uso de uno de los siguientes enfoques:

* [Ark en Azure](https://github.com/heptio/ark/blob/master/docs/azure-config.md)
* [Azure Site Recovery](https://azure.microsoft.com/blog/asr-managed-disks-between-azure-regions/)

### <a name="application-based-asynchronous-replication"></a>Replicación asincrónica basada en la aplicación

Actualmente no hay ninguna implementación nativa de Kubernetes para la replicación asincrónica basada en la aplicación. Con la naturaleza de conexión no directa de los contenedor y Kubernetes, cualquier aplicación o enfoque de lenguaje tradicional debe funcionar. El enfoque central es que las propias aplicaciones repliquen las solicitudes de almacenamiento que posteriormente se escriben en cada almacenamiento de datos subyacente del clúster.

![Replicación asincrónica basada en la aplicación](media/operator-best-practices-bc-dr/aks-app-based-async-repl.png)

## <a name="next-steps"></a>Pasos siguientes

Este artículo se centra en las consideraciones sobre continuidad empresarial y recuperación ante desastres en los clústeres de AKS. Para obtener más información acerca de las operaciones de clúster en AKS, consulte los siguientes procedimientos recomendados:

* [Multi-tenancy and cluster isolation][aks-best-practices-cluster-isolation] (Aislamiento multiempresa y de clúster)
* [Basic Kubernetes scheduler features][aks-best-practices-scheduler] (Características básicas de programadores de Kubernetes)

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
