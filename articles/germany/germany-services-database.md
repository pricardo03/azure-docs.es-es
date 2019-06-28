---
title: Servicios de base de datos de Azure Alemania | Microsoft Docs
description: Proporciona una comparación de los servicios de base de datos de Azure Alemania.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.openlocfilehash: 45e058d887ae79164196ab310f93270accbfb389
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033497"
---
# <a name="azure-germany-database-services"></a>Servicios de base de datos de Azure Alemania
## <a name="sql-database"></a>SQL Database
SQL Database V12 está disponible con carácter general en Azure Alemania. Para instrucciones sobre la configuración de visibilidad de los metadatos y los procedimientos recomendados de protección, consulte el [Microsoft Security Center para el motor de base de datos SQL](/sql/relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database) y la [documentación global de SQL Database](../sql-database/index.yml).

### <a name="variations"></a>Variaciones
La dirección de SQL Database en Azure Alemania es diferente de la dirección de Azure global:

| Tipo de servicio | Azure global | Azure Alemania |
| --- | --- | --- |
| SQL Database | *.database.windows.net | .database.cloudapi.de |


## <a name="azure-cache-for-redis"></a>Azure Cache for Redis
Para más información sobre Azure Cache for Redis y cómo usarlo, consulte [Documentación global de Azure Cache for Redis](../azure-cache-for-redis/index.md).

### <a name="variations"></a>Variaciones
Las direcciones URL para acceder a Azure Cache for Redis y administrarlo en Azure Alemania son diferentes de las direcciones URL de Azure global:

| Tipo de servicio | Azure global | Azure Alemania |
| --- | --- | --- |
| Punto de conexión de caché | *.redis.cache.windows.net | *.redis.cache.cloudapi.de |
| Portal de Azure | https://portal.azure.com | https://portal.microsoftazure.de |

> [!NOTE]
> Todos los scrips y todo el código deben tener en cuenta los puntos de conexión y entornos adecuados. Para más información, consulte "Conexión a Microsoft Azure Alemania" en [Administración de Azure Cache for Redis con Azure PowerShell](../azure-cache-for-redis/cache-howto-manage-redis-cache-powershell.md).
>
>


## <a name="next-steps"></a>Pasos siguientes
Para información complementaria y actualizaciones, suscríbase al [blog de Azure Alemania](https://blogs.msdn.microsoft.com/azuregermany/).
