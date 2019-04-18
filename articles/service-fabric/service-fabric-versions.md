---
title: Información sobre las versiones de clúster de Azure Service Fabric | Microsoft Docs
description: Versiones de clúster admitidas de Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chakdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/24/2018
ms.author: aljo
ms.openlocfilehash: d99000e1682b662f4bceb28096395243c894282f
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681614"
---
# <a name="supported-service-fabric-versions"></a>Versiones admitidas de Service Fabric

Asegúrese de que el clúster siempre ejecute una versión compatible de Service Fabric. Cuando anunciamos el lanzamiento de una nueva versión de Service Fabric, se marca la versión anterior para que finalice el soporte después de un mínimo de 60 días a partir de esa fecha. Las versiones nuevas se anuncian [en el blog del equipo de Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/).

Consulte los documentos siguientes para ver detalles sobre cómo hacer que el clúster ejecute una versión compatible de Service Fabric.

- [Actualizar la versión de Service Fabric en un clúster de Azure](service-fabric-cluster-upgrade.md)
- [Actualizar la versión de Service Fabric en un clúster independiente de windows server](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Versiones compatibles

En la tabla siguiente se enumera las versiones de Service Fabric que se admiten y sus fechas de finalización del soporte técnico.

| **Entorno de tiempo de ejecución de Service Fabric en el clúster** | **Se puede actualizar directamente desde la versión del clúster** |**Versiones del SDK o paquete de NuGet compatibles** | **Fecha de finalización de soporte técnico** |
| --- | --- |--- | --- |
| Todas las versiones de clústeres anteriores a 5.3.121 | 5.1.158* |Versión 2.3 o anterior |20 de enero de 2017 |
| 5.3* | 5.1.158.* |Versión 2.3 o anterior |24 de febrero de 2017 |
| 5.4.* | 5.1.158.* |Versión 2.4 o anterior |10 de mayo de 2017       |
| 5.5.* | 5.4.164.* |Versión 2.5 o anterior |10 de agosto de 2017    |
| 5.6.* | 5.4.164.* |Versión 2.6 o anterior |13 de octubre de 2017   |
| 5.7.* | 5.4.164.* |Versión 2.7 o anterior |15 de diciembre de 2017  |
| 6.0.* | 5.6.205.* |Versión menor o igual que la 2.8 |30 de marzo de 2018     |
| 6.1.* | 5.7.221.* |Versión 3.0 o anterior |15 de julio de 2018      |
| 6.2.* | 6.0.232.* |Versión 3.1 o anterior |26 de octubre de 2018   |
| 6.3.* | 6.1.480.* |Versión 3.2 o anterior |Marzo de 31,2019  |
| 6.4.* | 6.2.301.* |Versión 3.3 o anterior |Versión actual y, por lo tanto, sin fecha de finalización |

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

 En la tabla siguiente se enumera los sistemas operativos compatibles para las versiones compatibles de Service Fabric.

| **Sistema operativo** | **Versión más antigua de Fabric de servicios admitidos** |
| --- | --- |
| Windows Server 2012 R2 | Todas las versiones |
| Windows Server 2016 | Todas las versiones |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4. |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

