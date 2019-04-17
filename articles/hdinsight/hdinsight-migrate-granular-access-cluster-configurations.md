---
title: Cambios en el clúster acceso a configuración - Azure HDInsight
description: Obtenga información sobre los cambios en el acceso a campos de configuración de clúster confidenciales.
author: tylerfox
ms.author: tyfox
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: 85a6737ee111975d4c7ecf078673280127bfd0fd
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59610248"
---
# <a name="changes-to-cluster-configuration-access"></a>Cambios realizados en el acceso de configuración de clúster

La versión más reciente del SDK de HDInsight de Azure aporta algunos cambios importantes para admitir el acceso más específico basado en roles para obtener información confidencial. Como parte de ellos cambia, algunos **acción puede ser necesaria** si usa uno de los métodos afectados.

## <a name="sdk-for-net-500"></a>SDK para .NET 5.0.0

- [`ConfigurationOperationsExtensions.Get`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.get?view=azure-dotnet) le **ya no se devuelven los parámetros confidenciales** como claves de almacenamiento (sitio principal) o las credenciales de HTTP (puerta de enlace).
    - Para recuperar todas las configuraciones, incluidos los parámetros confidenciales, utilice `ConfigurationOperationsExtensions.List` a partir de ahora.  Tenga en cuenta que los usuarios con el rol "Lector" no podrá usar este método. Esto permite un control granular sobre el que los usuarios pueden acceder a información confidencial para un clúster. 
    - Para recuperar solo las credenciales de puerta de enlace HTTP, utilice `ClusterOperationsExtensions.GetGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.Update`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.update?view=azure-dotnet) Ahora está en desuso y se ha reemplazado por `ClusterOperationsExtensions.UpdateGatewaySettings`. 
- [`ConfigurationsOperationsExtensions.EnableHttp`](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.enablehttp?view=azure-dotnet) y [ `DisableHttp` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.hdinsight.configurationsoperationsextensions.disablehttp?view=azure-dotnet) están en desuso. HTTP ahora siempre está habilitado, por lo que estos métodos ya no son necesarios.