---
title: Admite las versiones de clúster en Azure Service Fabric | Microsoft Docs
description: Obtenga información sobre las versiones de clúster en Azure Service Fabric.
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
ms.openlocfilehash: 75e95737eecb9407a80103d1cad00d4987fe7091
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60716088"
---
# <a name="supported-service-fabric-versions"></a>Versiones admitidas de Service Fabric

Asegúrese de que el clúster siempre ejecuta una versión compatible de Azure Service Fabric. Finaliza un mínimo de 60 días después de que anunciamos el lanzamiento de una nueva versión de Service Fabric, compatibilidad con la versión anterior. Encontrará los anuncios de nuevas versiones en el [blog del equipo de Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/).

Consulte los siguientes documentos para obtener más información sobre cómo mantener el clúster que ejecuta una versión compatible de Service Fabric:

- [Actualización de un clúster de Azure Service Fabric](service-fabric-cluster-upgrade.md)
- [Actualizar la versión de Service Fabric que se ejecuta en el clúster de Windows Server independiente](service-fabric-cluster-upgrade-windows-server.md)

## <a name="supported-versions"></a>Versiones compatibles

En la tabla siguiente se enumera las versiones de Service Fabric y sus fechas de finalización del soporte técnico.

| En tiempo de ejecución de Service Fabric del clúster | Puede actualizar directamente desde la versión del clúster |Versión compatible del paquete SDK o NuGet | Finalización del soporte técnico |
| --- | --- |--- | --- |
| Todas las versiones de clúster anteriores a 5.3.121 | 5.1.158.* |Versión 2.3 o anterior |20 de enero de 2017 |
| 5.3* | 5.1.158.* |Versión 2.3 o anterior |24 de febrero de 2017 |
| 5.4.* | 5.1.158.* |Versión 2.4 o anterior |10 de mayo de 2017       |
| 5.5.* | 5.4.164.* |Versión 2.5 o anterior |10 de agosto de 2017    |
| 5.6.* | 5.4.164.* |Versión 2.6 o anterior |13 de octubre de 2017   |
| 5.7.* | 5.4.164.* |Versión 2.7 o anterior |15 de diciembre de 2017  |
| 6.0.* | 5.6.205.* |Versión menor o igual que la 2.8 |30 de marzo de 2018     |
| 6.1.* | 5.7.221.* |Versión 3.0 o anterior |15 de julio de 2018      |
| 6.2.* | 6.0.232.* |Versión 3.1 o anterior |26 de octubre de 2018   |
| 6.3.* | 6.1.480.* |Versión 3.2 o anterior |31 de marzo de 2019  |
| 6.4.* | 6.2.301.* |Versión 3.3 o anterior |La versión actual, por lo tanto, sin fecha de finalización |

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

En la tabla siguiente se enumera los sistemas operativos compatibles para las versiones compatibles de Service Fabric.

| Sistema operativo | Primera versión compatible de Service Fabric |
| --- | --- |
| Windows Server 2012 R2 | Todas las versiones |
| Windows Server 2016 | Todas las versiones |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4. |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

