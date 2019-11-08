---
title: Sincronización de datos desde Azure SQL Database Edge mediante SQL Data Sync | Microsoft Docs
description: Más información sobre la sincronización de datos entre Azure SQL Database Edge con Azure SQL Data Sync
keywords: sql database edge,sync data from sql database edge, sql database edge data sync
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 857cee30ac4c1002fb7ca57d6be5fa461a14e9ee
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73509199"
---
# <a name="tutorial-sync-data-from-sql-database-edge-to-azure-sql-database-using-sql-data-sync"></a>Tutorial: Sincronización de datos desde SQL Database Edge a Azure SQL Database con SQL Data Sync

En este tutorial, aprenderá a usar un *grupo de sincronización* de SQL Data Sync para sincronizar incrementalmente los datos de Azure SQL Database Edge con Azure SQL Database. SQL Data Sync es un servicio basado en Azure SQL Database que permite sincronizar los datos seleccionados de manera bidireccional entre varias bases de datos SQL e instancias de SQL Server. Para más información sobre Azure SQL Data Sync, consulte [Azure SQL Data Sync](../sql-database/sql-database-sync-data.md).

Como Azure SQL Database Edge se basa en las versiones más recientes del [Motor de base de datos de Microsoft SQL Server](/sql/sql-server/sql-server-technical-documentation/), cualquier mecanismo de sincronización de datos que sea aplicable a una instancia de SQL Server local también se puede usar para sincronizar datos con la instancia de SQL Dabatase Edge que se ejecute en un dispositivo perimetral.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se requiere un equipo Windows configurado con el [Azure SQL Data Sync Agent](../sql-database/sql-database-data-sync-agent.md).

## <a name="before-you-begin"></a>Antes de empezar

* Cree una base de datos de Azure SQL Database. Para información sobre cómo crear una base de datos de Azure SQL Database mediante Azure Portal, consulte [Creación de una base de datos única en Azure SQL Database](../sql-database/sql-database-single-database-get-started.md?tabs=azure-portal).

* Cree las tablas y otros objetos necesarios en la implementación de Azure SQL Database.

* Cree las tablas y objetos necesarios en la implementación de Azure SQL Database Edge. Para más información, vea [Uso de paquetes DAC de SQL Database con SQL Database Edge](stream-analytics.md).

* Registre la instancia de Azure SQL Database Edge con Azure SQL Data Sync Agent. Para más información, consulte [Incorporación de una base de datos local de SQL Server](../sql-database/sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="sync-data-between-an-azure-sql-database-and-sql-database-edge"></a>Sincronización de datos entre una base de datos de Azure SQL Database y SQL Database Edge

La configuración de la sincronización entre una base de datos de Azure SQL Database y una instancia de SQL Database Edge mediante SQL Data Sync implica tres pasos clave.  

1. Use Azure Portal para crear un grupo de sincronización. Para crear el grupo de sincronización, consulte [Creación de un grupo de sincronización con Azure Portal](../sql-database/sql-database-get-started-sql-data-sync.md#create-sync-group). Se puede usar la misma base de datos *Centro* para crear varios grupos de sincronización diferentes y sincronizar datos de instancias distintas de SQL Database Edge con una o varias bases de datos SQL Database en Azure.

2. Agregue miembros de Sync al grupo de sincronización. Para agregar miembros al grupo de sincronización, remítase a [Incorporación de miembros al grupo de sincronización de SQL Data Sync](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

3. Configure el grupo de sincronización para seleccionar las tablas que formarán parte de esta sincronización. Para configurar el grupo de sincronización, consulte [Configuración del grupo de sincronización](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

Después de completar los pasos anteriores, tiene un grupo de sincronización que incluye una base de datos de Azure SQL Database y una instancia de SQL Database Edge.

Para más información sobre SQL Data Sync, vea los siguientes artículos:

* [Data Sync Agent para Azure SQL Data Sync](../sql-database/sql-database-data-sync-agent.md)

* [Procedimientos recomendados](../sql-database/sql-database-best-practices-data-sync.md) y [Solución de problemas de Azure SQL Data Sync](../sql-database/sql-database-troubleshoot-data-sync.md)

* [Monitor SQL Data Sync with Azure Monitor logs](../sql-database/sql-database-sync-monitor-oms.md) (Supervisión de SQL Data Sync con registros de Azure Monitor)

* [Actualización del esquema de sincronización con Transact-SQL](../sql-database/sql-database-update-sync-schema.md) o [PowerShell](../sql-database/scripts/sql-database-sync-update-schema.md)

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Powershell para sincronizar entre Azure SQL Database y Azure SQL Database Edge](../sql-database/scripts/sql-database-sync-data-between-azure-onprem.md). En el tutorial, reemplace los detalles de la base de datos *OnPremiseServer* con los detalles de Azure SQL Database Edge.
