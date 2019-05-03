---
title: 'Planeación de los eventos de mantenimiento de Azure: Azure SQL Database | Microsoft Docs'
description: Aprenda a prepararse para los eventos de mantenimiento planeado en su base de datos de Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
manager: craigg
ms.date: 01/30/2019
ms.openlocfilehash: 928338a911efae051df7164239dbd19f9317338a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584614"
---
# <a name="planning-for-azure-maintenance-events-in-azure-sql-database"></a>Planeación de los eventos de mantenimiento en Azure SQL Database

Aprenda a prepararse para los eventos de mantenimiento planeado en su base de datos de Azure SQL.

## <a name="what-is-a-planned-maintenance-event"></a>Qué es un evento de mantenimiento planeado

Para cada base de datos, Azure SQL Database mantiene un cuórum de réplicas de base de datos en el que una de ellas es la principal. En todo momento una réplica principal debe realizar mantenimiento en línea, mientras que al menos una réplica secundaria debe estar en buen estado. Durante el mantenimiento planeado, los miembros del cuórum de la base de datos se quedarán sin conexión una a la vez, con la intención de que haya una réplica principal respondiendo y al menos una réplica secundaria en línea, de forma que no haya tiempo de inactividad en el cliente. Cuando sea necesario que la réplica principal esté sin conexión, se producirá un proceso de reconfiguración o conmutación por error en el que una réplica secundaria se convertirá en la nueva réplica principal.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Qué esperar durante un evento de mantenimiento planeado

Las reconfiguraciones o conmutaciones por error normalmente se completan en 30 segundos; el promedio es de 8 segundos. Si ya está conectada, la aplicación debe volver a conectarse a la nueva copia correcta de la réplica principal de la base de datos. Si se intenta establecer una nueva conexión mientras la base de datos está realizando una reconfiguración antes de que la nueva réplica esté en línea, se devolverá el error 40613 (base de datos no disponible): "La base de datos '{nombre de la base de datos}' del servidor '{nombre del servidor}' no está disponible actualmente. Vuelva a intentar la conexión más tarde". Si la base de datos tiene una consulta de larga duración, esta consulta se interrumpirá durante la reconfiguración y deberá reiniciarse.

## <a name="retry-logic"></a>Lógica de reintento

Todas las aplicaciones cliente de producción que se conecten a un servicio de base de datos en la nube deben implementar una [lógica de reintento](sql-database-connectivity-issues.md#retry-logic-for-transient-errors) de conexión sólida. Así, ayudarán a mitigar estas situaciones y, por lo general, harán que el usuario final no tenga noticias de los errores.

## <a name="frequency"></a>Frecuencia

En promedio, se producen 1,7 eventos de mantenimiento planeado cada mes.

## <a name="resource-health"></a>Estado de los recursos

Si la base de datos SQL está experimentando errores de inicio de sesión, compruebe la ventana de [Resource Health](../service-health/resource-health-overview.md#getting-started) en [Azure Portal](https://portal.azure.com) para conocer el estado actual. La sección del historial de estado contiene el motivo del tiempo de inactividad de cada evento (si está disponible).


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información acerca de [Resource Health](sql-database-resource-health.md) para SQL Database.
- Para obtener más información acerca de la lógica de reintentos, consulte [Lógica de reintento para errores transitorios](sql-database-connectivity-issues.md#retry-logic-for-transient-errors).
