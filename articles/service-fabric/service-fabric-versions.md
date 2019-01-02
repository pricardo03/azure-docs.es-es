---
title: Información sobre las versiones de clúster de Azure Service Fabric | Microsoft Docs
description: Versiones de clúster admitidas de Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: twhitney
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/24/2018
ms.author: TylerMSFT
ms.openlocfilehash: 154869c3e6bcd44a71480a3cf7363537dddcebf9
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727688"
---
# <a name="supported-service-fabric-versions"></a>Versiones admitidas de Service Fabric

Asegúrese de que el clúster siempre ejecute una versión compatible de Service Fabric. Cuando anunciamos el lanzamiento de una nueva versión de Service Fabric, se marca la versión anterior para que finalice el soporte después de un mínimo de 60 días a partir de esa fecha. Las versiones nuevas se anuncian [en el blog del equipo de Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/).

Consulte los documentos siguientes para ver detalles sobre cómo hacer que el clúster ejecute una versión compatible de Service Fabric.

- [Actualización de la versión de Service Fabric en un clúster de Azure ](service-fabric-cluster-upgrade.md)
- [Actualización de la versión de Service Fabric en un clúster de servidores de Windows independiente ](service-fabric-cluster-upgrade-windows-server.md)

A continuación, aparece una lista con las versiones compatibles de Service Fabric y las fechas de finalización del soporte técnico.

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
| 6.3.* | 6.1.480.* |Versión 3.2 o anterior |28 de febrero de 2019  |
| 6.4.* | 6.2.301.* |Versión 3.3 o anterior |Versión actual y, por lo tanto, sin fecha de finalización |