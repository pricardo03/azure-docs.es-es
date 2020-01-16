---
title: Detección de amenazas en servicios de datos en Azure Security Center | Microsoft Docs
description: En este artículo se presentan las alertas de servicios de datos disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.topic: conceptual
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 6d6e48c84f558840b3266193c4cbb9c3576f1a58
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75665741"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Detección de amenazas en servicios de datos en Azure Security Center

 Azure Security Center analiza los registros de los servicios de almacenamiento de datos y activa alertas cuando detecta una amenaza para los recursos de datos. En este artículo se enumeran las alertas que Security Center genera para los siguientes servicios:

* [Azure SQL Database y Azure SQL Data Warehouse](#data-sql)
* [Almacenamiento de Azure](#azure-storage)
* [Azure Cosmos DB](#cosmos-db)

## SQL Database y SQL Data Warehouse <a name="data-sql"></a>

Advanced Threat Protection para Azure SQL Database detecta actividades anómalas que indican intentos inusuales y potencialmente dañinos de acceso o ataque a las bases de datos.

Verá alertas cuando haya actividades sospechosas en las bases de datos, posibles vulnerabilidades o ataques por inyección de código SQL, así como patrones anómalos de acceso y consulta a las bases de datos.

Advanced Threat Protection para Azure SQL Database y SQL forma parte del paquete unificado [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security) para funcionalidades de seguridad SQL avanzadas, que abarca instancias de Azure SQL Database, instancias administradas de Azure SQL Database, bases de datos de Azure SQL Data Warehouse y servidores SQL Server en Azure Virtual Machines.

Para más información, consulte:

* [Cómo habilitar Advanced Threat Protection para Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)
* [Cómo habilitar Advanced Threat Protection para servidores SQL Server en Azure Virtual Machines](security-center-iaas-advanced-data.md)
* [La lista de alertas de protección contra amenazas para SQL Database y SQL Data Warehouse](alerts-reference.md#alerts-sql-db-and-warehouse)

## Azure Storage <a name="azure-storage"></a>

Advanced Threat Protection para Storage (disponible actualmente solo para Blob Storage) detecta los intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de almacenamiento o de vulnerarlas. Esta capa de protección le permite afrontar las amenazas sin necesidad de ser un experto en seguridad y le ayuda a administrar sus sistemas de supervisión de la seguridad.

>[!NOTE]
>Advanced Threat Protection para Storage no está disponible actualmente en Azure Government ni en regiones de nube soberana.

Para más información, consulte:

* [Cómo habilitar Advanced Threat Protection para Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection)
* [La lista de alertas de protección contra amenazas para Azure Storage](alerts-reference.md#alerts-azurestorage)

## Azure Cosmos DB<a name="cosmos-db"></a>

Las alertas de Azure Cosmos DB las generan los intentos inusuales y potencialmente dañinos de acceso o aprovechamiento de cuentas de Azure Cosmos DB.

Para más información, consulte:

* [Advanced Threat Protection para Azure Cosmos DB (versión preliminar)](../cosmos-db/cosmos-db-advanced-threat-protection.md)
* [La lista de alertas de protección contra amenazas para Azure Cosmos DB (versión preliminar)](alerts-reference.md#alerts-azurecosmos)
