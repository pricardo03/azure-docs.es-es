---
title: Planear la implementación de un clúster de Azure Service Fabric | Microsoft Docs
description: Obtenga información sobre la planeación y preparación para una implementación de clúster de Service Fabric en Azure de producción.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: aljo
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: aljo
ms.openlocfilehash: 0f3a9010805ec1a18490f6f530f60d7a3c763398
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663245"
---
# <a name="plan-and-prepare-for-a-cluster-deployment"></a>Planeación y preparación para la implementación de un clúster

Planeación y preparación para la implementación de un clúster de producción son muy importante.  Hay muchos factores a tener en cuenta.  Este artículo le guiará por los pasos de preparación de la implementación del clúster.

## <a name="read-the-best-practices-information"></a>Lea la información de los procedimientos recomendados
Para administrar clústeres y las aplicaciones de Azure Service Fabric correctamente, hay operaciones que se recomienda que realizar para optimizar la confiabilidad del entorno de producción.  Para obtener más información, lea [prácticas recomendadas de aplicación de Service Fabric y clúster](service-fabric-best-practices-overview.md).

## <a name="select-the-os-for-the-cluster"></a>Seleccione el sistema operativo para el clúster
Service Fabric permite la creación de clústeres de Service Fabric en cualquier máquina virtual o equipo con Windows Server o Linux.  Antes de implementar el clúster, debe elegir el sistema operativo:  Windows o Linux.  Cada nodo (máquina virtual) en el clúster ejecuta el mismo sistema operativo, no se pueden mezclar Windows y máquinas virtuales Linux en el mismo clúster.

## <a name="capacity-planning"></a>Planificación de capacidad
En cualquier implementación de producción, la planeación de capacidad es un paso importante. Esto es algo que hay que tener en cuenta como parte de ese proceso.

* El número inicial de los tipos de nodos del clúster 
* Las propiedades de cada tipo de nodo (tamaño, número de instancias, principales, accesible desde internet, número de máquinas virtuales, etcetera.)
* Características de confiabilidad y durabilidad del clúster

### <a name="select-the-initial-number-of-node-types"></a>Seleccione el número inicial de los tipos de nodo
En primer lugar, debe decidir para qué va a servir el clúster que va a crear. ¿Qué tipos de aplicaciones planea que se van a implementar en este clúster? ¿Tiene la aplicación varios servicios y alguno de ellos ha de ser público o accesible desde Internet? ¿Tienen los servicios (que componen la aplicación) distintos requisitos de infraestructura, como, por ejemplo, una RAM mayor o un número más alto de ciclos de CPU? Un clúster de Service Fabric puede constar de más de un tipo de nodo: un tipo de nodo principal y uno o varios tipos de nodo no principal. Cada tipo de nodo se asigna a un conjunto de escalado de máquinas virtuales. Cada tipo de nodo se puede escalar o reducir verticalmente de forma independiente. Cada uno tiene diferentes conjuntos de puertos abiertos y puede tener distintas métricas de capacidad. [Propiedades de nodo y restricciones de colocación] [ placementconstraints] puede configurarse para restringir los servicios específicos a los tipos de nodo específico.  Para obtener más información, lea [el número de tipos de nodo del clúster debe empezar con](service-fabric-cluster-capacity.md#the-number-of-node-types-your-cluster-needs-to-start-out-with).

### <a name="select-node-properties-for-each-node-type"></a>Seleccione las propiedades de nodo para cada tipo de nodo
Tipos de nodo definen el VM SKU, número y las propiedades de las máquinas virtuales del conjunto de escalado asociado.

El tamaño mínimo de máquinas virtuales para cada tipo de nodo viene determinada por la [nivel de durabilidad] [ durability] que elija para el tipo de nodo.

El número mínimo de máquinas virtuales para el tipo de nodo principal viene determinada por la [nivel de confiabilidad] [ reliability] elija.

Vea las recomendaciones mínimas para [tipos de nodo principal](service-fabric-cluster-capacity.md#primary-node-type---capacity-guidance), [cargas de trabajo con estado en tipos de nodo no principal](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateful-workloads), y [cargas de trabajo sin estado en tipos de nodo no principal](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateless-workloads). 

Nada más que el número mínimo de nodos deben basarse en el número de réplicas de la aplicación o de servicios que desea ejecutar en este tipo de nodo.  [Planeamiento de capacidad para aplicaciones de Service Fabric](service-fabric-capacity-planning.md) ayuda a calcular los recursos que necesita para ejecutar sus aplicaciones. Siempre puede escalar el clúster o hacia abajo más adelante para ajustar para cambiar la carga de trabajo de aplicación. 

### <a name="select-the-durability-and-reliability-levels-for-the-cluster"></a>Seleccione los niveles de confiabilidad y durabilidad del clúster
El nivel de durabilidad se usa para indicar al sistema los privilegios que tienen las máquinas virtuales con la infraestructura subyacente de Azure. En el tipo de nodo principal, este privilegio permite que Service Fabric pause cualquier solicitud de la infraestructura de nivel de máquina virtual (por ejemplo, reinicio de máquina virtual, restablecimiento de la imagen inicial de máquina virtual o migración de máquina virtual) que afecte a los requisitos de quórum de los servicios del sistema y los servicios con estado. En los tipos de nodo distintos del principal, este privilegio permite que Service Fabric pause cualquier solicitud de la infraestructura de nivel de máquina virtual (por ejemplo, el reinicio de máquina virtual, el restablecimiento de la imagen inicial de máquina virtual o la migración de máquinas virtuales) que afecte a los requisitos de cuórum de los servicios con estado.  Para las ventajas de los diferentes niveles y recomendaciones sobre qué nivel para usar y cuándo, vea [las características de durabilidad del clúster][durability].

El nivel de confiabilidad se usa para establecer el número de réplicas de los servicios del sistema que se desea ejecutar en el tipo de nodo principal de este clúster. Cuanto mayor sea el número de réplicas, más confiables son los servicios del sistema en el clúster.  Para las ventajas de los diferentes niveles y recomendaciones sobre qué nivel para usar y cuándo, vea [las características de confiabilidad del clúster][reliability]. 

## <a name="enable-reverse-proxy-andor-dns"></a>Habilitar proxy inverso o DNS
Los servicios que se conectan entre sí dentro de un clúster pueden acceder, por lo general, directamente a los puntos de conexión de otros servicios ya que los nodos de un clúster se encuentran en la misma red local. Para facilitar la conexión entre servicios, Service Fabric proporciona servicios adicionales: Un [servicio DNS](service-fabric-dnsservice.md) y un [servicio de proxy inverso](service-fabric-reverseproxy.md).  Al implementar un clúster, se pueden habilitar ambos servicios.

Ya que muchos servicios, servicios sobre todo en contenedores, puede tener un nombre de dirección URL existente, poder resolverlo mediante el estándar DNS protocolo (en lugar del protocolo de servicio de nomenclatura) es práctico, especialmente en escenarios "lift and shift" de aplicación. Esto es exactamente lo que hace el servicio DNS. Permite asignar nombres DNS a un nombre de servicio y, por tanto, resolver direcciones IP del punto de conexión.

El proxy inverso aborda servicios en el clúster que exponen los puntos de conexión HTTP (incluido HTTPS). El proxy inverso simplifica en gran medida una llamada a otros servicios, ya que proporciona un formato URI específico.  El proxy inverso también controla la resolución, conexión y vuelva a intentar los pasos necesarios para que un servicio para comunicarse con otro.

## <a name="prepare-for-disaster-recovery"></a>Preparación para la recuperación ante desastres
Una parte fundamental de la entrega de alta disponibilidad es garantizar que los servicios sobreviven a cualquier tipo de error. Esto es especialmente importante para los errores imprevistos y que se encuentran fuera de control. [Prepararse para la recuperación ante desastres](service-fabric-disaster-recovery.md) se describen algunos modos de error comunes que podrían ser desastres si no hubieran modelado y administrado correctamente. También se describen las mitigaciones y acciones emprender si un desastre.

## <a name="production-readiness-checklist"></a>Lista de comprobación sobre la preparación de producción
¿La aplicación y el clúster están preparados para asumir el tráfico de producción? Antes de implementar el clúster en producción, recorrer el [lista de comprobación de preparación para la producción](service-fabric-production-readiness-checklist.md). Mantenga su aplicación y el clúster que se ejecuta sin problemas al repasar los elementos de esta lista de comprobación. Se recomienda encarecidamente a todos estos elementos para desactivarse antes de pasar a producción.

## <a name="next-steps"></a>Pasos siguientes
* [Crear un clúster de Service Fabric que ejecute Windows](service-fabric-best-practices-overview.md)
* [Crear un clúster de Service Fabric con Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md)

[placementconstraints]: service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints
[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster