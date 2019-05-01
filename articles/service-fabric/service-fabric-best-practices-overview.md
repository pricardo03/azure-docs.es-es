---
title: Procedimientos recomendados para aplicaciones y clústeres de Azure Service Fabric | Microsoft Docs
description: Procedimientos recomendados para la administración de aplicaciones y clústeres de Service Fabric.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: a0174a5442de7a10e45e9dc2a2f43f5f401d1a69
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60803168"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Procedimientos recomendados para aplicaciones y clústeres de Azure Service Fabric

Para administrar clústeres y aplicaciones de Azure Service Fabric correctamente, hay operaciones muy recomendables que se pueden realizar para optimizar la confiabilidad del entorno de producción. Realice las operaciones definidas en este documento y seleccione una de nuestras [plantillas de clúster de Service Fabric de los ejemplos de Azure](https://github.com/Azure-Samples/service-fabric-cluster-templates) para comenzar a diseñar la solución de producción o para modificar la plantilla existente con el fin de incorporar estos procedimientos.

## <a name="security"></a>Seguridad 

* [Procedimientos recomendados para la seguridad](service-fabric-best-practices-security.md)

## <a name="networking"></a>Redes

* [Procedimientos recomendados para las redes](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>Planeación y escalado de procesos

* [Procedimientos recomendados para el escalado de procesos](service-fabric-best-practices-capacity-scaling.md)
* [Planeamiento de la capacidad de proceso](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Infraestructura como código

* [Procedimientos recomendados para la implementación de infraestructura como código](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>Supervisión y diagnóstico

* [Procedimientos recomendados para la supervisión y diagnóstico de clústeres](service-fabric-best-practices-monitoring.md)

## <a name="checklist"></a>Lista de comprobación

Cuando haya completado todas las secciones anteriores, asegúrese de que ha integrado todos los procedimientos recomendados en la lista de comprobación de preparación para la producción:
* [Lista de comprobación sobre la preparación de producción de Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Pasos siguientes

* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-cluster-creation-via-portal.md)
* Solución de problemas: [Guía de solución de problemas de Service Fabric](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)