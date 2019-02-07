---
title: 'Nivel de uso general: servicio Azure SQL Database | Microsoft Docs'
description: Obtenga información sobre el nivel de uso general de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 943982b056a83488426c48763deac14fd5347b8e
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509011"
---
# <a name="general-purpose-tier---azure-sql-database"></a>Nivel de uso general: Azure SQL Database

> [!NOTE]
> El nivel de uso general se denomina estándar en el modelo de compra de DTU. Para ver una comparación entre el modelo de compra basado en núcleos virtuales y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](sql-database-service-tiers.md).

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server adaptada al entorno en la nube, con el fin de garantizar una disponibilidad del 99,99 % incluso en los casos de error de la infraestructura. Hay tres modelos de arquitectura que se usan en Azure SQL Database:
- Uso general 
- Crítico para la empresa
- Hiperescala

El modelo de uso general se basa en una separación de proceso y almacenamiento. Este modelo de arquitectura se basa en la alta disponibilidad y confiabilidad de Azure Premium Storage que replica de forma transparente los archivos de base de datos y garantiza que no se pierdan los datos si se produce algún error en la infraestructura subyacente.

En la siguiente imagen se muestran cuatro nodos en el modelo de arquitectura estándar con las capas separadas de proceso y almacenamiento.

![Separación de proceso y almacenamiento](media/sql-database-managed-instance/general-purpose-service-tier.png)

En el modelo de uso general hay dos capas:

- Una capa de proceso sin estado que ejecuta el proceso `sqlserver.exe` y que solo contiene datos en caché y transitorios (por ejemplo, caché de planes, grupo de búferes o grupo de almacenes de columnas). Este nodo sin estado de SQL Server lo opera Azure Service Fabric, que inicializa el proceso, controla el estado del nodo y realiza la conmutación por error en otro lugar si es necesario.
- Una capa de datos con estado con archivos de base de datos (.mdf o .ldf) que se almacenan en Azure Premium Storage. Azure Storage garantiza que no se pierdan los datos de ningún registro que se encuentre en un archivo de base de datos. Azure Storage tiene redundancia o disponibilidad de datos integrada que garantiza la conservación de los registros de una página o archivo de registro aunque se bloquee el proceso de SQL Server.

Siempre que se actualice un sistema operativo o un motor de base de dato, si se produce un error en la infraestructura subyacente o, si se detecta algún problema crítico en el proceso de SQL Server, Azure Service Fabric moverá el proceso sin estado de SQL Server a otro nodo de proceso sin estado. Hay un conjunto de nodos libres que esperan ejecutar un nuevo servicio de proceso si se produce una conmutación por error del nodo principal, a fin de minimizar el tiempo de conmutación por error. Esto no afecta a los datos de la capa de Azure Storage y los archivos de registro o de datos se anexan al proceso de SQL Server inicializado recientemente. Este proceso garantiza una disponibilidad del 99,99 %, pero podría afectar al rendimiento de grandes cargas de trabajo que se ejecutan debido al tiempo de transición y al hecho de que el nuevo nodo de SQL Server se inicia con la caché inactiva.

## <a name="when-to-choose-this-service-tier"></a>¿Cuándo elegir este nivel de servicio?

El nivel de servicio de uso general es un nivel de servicio predeterminado e Azure SQL Database diseñado para la mayoría de las cargas de trabajo genéricas. Si necesita un motor de base de datos totalmente administrado con un Acuerdo de Nivel de Servicio de un 99,99 % de latencia de almacenamiento de entre 5 y 10 ms que coincida con IaaS de Azure SQL en la mayoría de los casos, el nivel de uso general es la opción más conveniente en su caso.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre los niveles [Crítico para la empresa](sql-database-service-tier-business-critical.md) e [Hiperescala](sql-database-service-tier-hyperscale.md).
- Obtenga información sobre [Service Fabric](../service-fabric/service-fabric-overview.md).
- Para conocer más opciones de alta disponibilidad y recuperación ante desastres, consulte [Continuidad del negocio](sql-database-business-continuity.md).
