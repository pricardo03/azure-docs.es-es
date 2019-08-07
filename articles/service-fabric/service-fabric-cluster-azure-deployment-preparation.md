---
title: Planificación de una implementación de clúster de Azure Service Fabric| Microsoft Docs
description: Obtenga información sobre la planificación y preparación de la implementación de un clúster de producción de Service Fabric.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: atsenthi
ms.openlocfilehash: a130e9bc8859360704c9be1c0a7fe066d2ed4567
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600006"
---
# <a name="plan-and-prepare-for-a-cluster-deployment"></a>Planeamiento y preparación de la implementación de un clúster

La planeación y preparación para la implementación de un clúster de producción son muy importantes.  Existen muchos factores a tener en cuenta.  Este artículo le guiará por los pasos de preparación de la implementación del clúster.

## <a name="read-the-best-practices-information"></a>Lectura de información de procedimientos recomendados
Para administrar clústeres y aplicaciones de Azure Service Fabric correctamente, existen una serie de procedimientos que le recomendamos realizar para optimizar la fiabilidad de su entorno de producción.  Para obtener más información, consulte [Procedimientos recomendados para aplicaciones y clústeres de Service Fabric](service-fabric-best-practices-overview.md).

## <a name="select-the-os-for-the-cluster"></a>Selección del sistema operativo del clúster
Service Fabric permite la creación de clústeres de Service Fabric en cualquier máquina virtual o equipo con Windows Server o Linux.  Antes de implementar el clúster, debe elegir el sistema operativo:  Windows o Linux.  Todos los nodos (máquina virtual) del clúster utilizan el mismo sistema operativo. No puede combinar máquinas virtuales de Windows y Linux en el mismo clúster.

## <a name="capacity-planning"></a>Planificación de capacidad
En cualquier implementación de producción, la planeación de capacidad es un paso importante. Esto es algo que hay que tener en cuenta como parte de ese proceso.

* Número inicial de tipos de nodo para el clúster 
* Propiedades de cada tipo de nodo (tamaño, número de instancias, principal, accesible desde Internet, número de máquinas virtuales, etc.)
* Características de confiabilidad y durabilidad del clúster

### <a name="select-the-initial-number-of-node-types"></a>Selección del número inicial de tipos de nodo
En primer lugar, debe decidir para qué va a servir el clúster que va a crear. ¿Qué tipos de aplicaciones planea que se van a implementar en este clúster? ¿Tiene la aplicación varios servicios y alguno de ellos ha de ser público o accesible desde Internet? ¿Tienen los servicios (que componen la aplicación) distintos requisitos de infraestructura, como, por ejemplo, una RAM mayor o un número más alto de ciclos de CPU? Un clúster de Service Fabric puede constar de más de un tipo de nodo: un tipo de nodo principal o más tipos de nodo no principales. Cada tipo de nodo se asigna a un conjunto de escalado de máquinas virtuales. Cada tipo de nodo se puede escalar o reducir verticalmente de forma independiente. Cada uno tiene diferentes conjuntos de puertos abiertos y puede tener distintas métricas de capacidad. Se pueden configurar [restricciones de ubicación y propiedades de nodo][placementconstraints] para limitar servicios específicos para tipos de nodo determinados.  Para obtener más información, consulte [Número de tipos de nodo necesarios para el inicio del clúster](service-fabric-cluster-capacity.md#the-number-of-node-types-your-cluster-needs-to-start-out-with).

### <a name="select-node-properties-for-each-node-type"></a>Selección de propiedades de nodo para cada tipo de nodo
Los tipos de nodo definen la SKU de máquina virtual el número y las propiedades de las máquinas virtuales en el conjunto de escalado asociado.

El tamaño mínimo de las máquinas virtuales para cada tipo de nodo está determinado por el [nivel de durabilidad][durability] que elija para el tipo de nodo.

El número mínimo de máquinas virtuales del tipo de nodo principal se determina mediante el [nivel de confiabilidad][reliability] que se elija.

Consulte las recomendaciones mínimas para [tipos de nodo principales](service-fabric-cluster-capacity.md#primary-node-type---capacity-guidance), [cargas de trabajo con estado en tipos de nodo no principales](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateful-workloads) y [cargas de trabajo sin estado en tipos de nodo no principales](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateless-workloads). 

Los números de nodos superiores al mínimo deben basarse en el número de réplicas de la aplicación o los servicios que desee ejecutar en ese tipo de nodo.  [Planeamiento de capacidad para aplicaciones de Service Fabric](service-fabric-capacity-planning.md) le ayuda a estimar los recursos que necesita para ejecutar las aplicaciones. Siempre puede escalar o reducir verticalmente el clúster más tarde para ajustarlo a una carga de trabajo de aplicaciones cambiante. 

### <a name="select-the-durability-and-reliability-levels-for-the-cluster"></a>Selección de los niveles de durabilidad y confiabilidad para el clúster
El nivel de durabilidad se usa para indicar al sistema los privilegios que tienen las máquinas virtuales con la infraestructura subyacente de Azure. En el tipo de nodo principal, este privilegio permite que Service Fabric pause cualquier solicitud de la infraestructura de nivel de máquina virtual (por ejemplo, reinicio de máquina virtual, restablecimiento de la imagen inicial de máquina virtual o migración de máquina virtual) que afecte a los requisitos de quórum de los servicios del sistema y los servicios con estado. En los tipos de nodo distintos del principal, este privilegio permite que Service Fabric pause cualquier solicitud de la infraestructura de nivel de máquina virtual (por ejemplo, el reinicio de máquina virtual, el restablecimiento de la imagen inicial de máquina virtual o la migración de máquinas virtuales) que afecte a los requisitos de cuórum de los servicios con estado.  Para conocer las ventajas de los diferentes niveles y obtener recomendaciones sobre qué nivel usar y cuándo, vea [Características de durabilidad del clúster][durability].

El nivel de confiabilidad se usa para establecer el número de réplicas de los servicios del sistema que se desea ejecutar en el tipo de nodo principal de este clúster. Cuanto mayor sea el número de réplicas, más confiables son los servicios del sistema en el clúster.  Para conocer las ventajas de los diferentes niveles y obtener recomendaciones sobre qué nivel usar y cuándo, vea [Características de confiabilidad del clúster][reliability]. 

## <a name="enable-reverse-proxy-andor-dns"></a>Habilitación del proxy inverso o DNS
Los servicios que se conectan entre sí dentro de un clúster pueden acceder, por lo general, directamente a los puntos de conexión de otros servicios ya que los nodos de un clúster se encuentran en la misma red local. Para facilitar la conexión entre servicios, Service Fabric ofrece servicios adicionales: Un [servicio DNS](service-fabric-dnsservice.md) y un [servicio de proxy inverso](service-fabric-reverseproxy.md).  Se pueden habilitar ambos servicios cuando se implementa un clúster.

Debido a que muchos servicios, sobre todo los servicios en contenedores, pueden tener un nombre de dirección URL existente, es conveniente tener la capacidad de resolverlo mediante el protocolo DNS estándar (en lugar del protocolo Naming Service), en particular en escenarios "lift-and-shift" de la aplicación. Esto es exactamente lo que hace el servicio DNS. Permite asignar nombres DNS a un nombre de servicio y, por tanto, resolver direcciones IP del punto de conexión.

Los servicios de direcciones de proxy inverso del clúster que expone puntos de conexión HTTP (incluidos HTTPS). El proxy inverso simplifica ampliamente la llamada a otros servicios mediante un formato de URI específico.  El proxy inverso también administra los pasos de resolución, conexión y reintento necesarios para que un servicio se comunique con otro.

## <a name="prepare-for-disaster-recovery"></a>Preparación para la recuperación ante desastres
Una parte fundamental de la entrega de alta disponibilidad es garantizar que los servicios sobreviven a cualquier tipo de error. Esto es especialmente importante para los errores imprevistos y que se encuentran fuera de control. [Preparación para la recuperación ante desastres](service-fabric-disaster-recovery.md) describe algunos modos de error frecuentes que podrían convertirse en desastres si no se modelan y administran correctamente. También se explican las mitigaciones y las acciones que deben realizarse en caso de desastre.

## <a name="production-readiness-checklist"></a>Lista de comprobación sobre la preparación de producción
¿La aplicación y el clúster están preparados para asumir el tráfico de producción? Antes de realizar la implementación del clúster en la producción, ejecute la [lista de comprobación sobre la preparación de producción](service-fabric-production-readiness-checklist.md). Revise los elementos de la siguiente lista de comprobación para que la aplicación y el clúster se ejecuten sin problemas. Se recomienda encarecidamente comprobar todos estos elementos antes de pasar a la producción.

## <a name="next-steps"></a>Pasos siguientes
* [Creación de un clúster de Service Fabric que ejecuta Windows](service-fabric-best-practices-overview.md)
* [Creación de un clúster de Service Fabric que ejecuta Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md)

[placementconstraints]: service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints
[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster