---
title: Introducción a Azure Database Migration Service | Microsoft Docs
description: Una introducción a Azure Database Migration Service, que proporciona migraciones completas desde muchos orígenes de base de datos hasta las plataformas de datos de Azure.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: douglasl
ms.service: database-migration
ms.workload: data-services
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 053e571b6285cd405ea17f43fec1d3ea99732070
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235588"
---
# <a name="what-is-the-azure-database-migration-service"></a>¿Qué es Azure Database Migration Service?
Azure Database Migration Service es un servicio totalmente administrado diseñado para permitir migraciones completas desde varios orígenes de base de datos hasta las plataformas de datos de Azure con un tiempo de inactividad mínimo (migraciones en línea).

## <a name="migrate-databases-to-azure-with-familiar-tools"></a>Migración de bases de datos a Azure con herramientas familiares
Azure Database Migration Service integra una parte de la funcionalidad de nuestras herramientas y servicios existentes. Así, ofrece a los clientes una solución completa y de alta disponibilidad. El servicio utiliza [Data Migration Assistant](https://aka.ms/dma) para generar informes de evaluación que proporcionen recomendaciones que le guíen a través de los cambios necesarios antes de realizar una migración. Es decisión suya aplicar los remedios necesarios. Cuando esté listo para comenzar el proceso de migración, Azure Database Migration Service realizará todos los pasos necesarios. Ya puede iniciar sus proyectos de migración y olvidarse de ellos con la tranquilidad de saber que el proceso utiliza los procedimientos recomendados que determina Microsoft.

> [!NOTE]
> El uso de Azure Database Migration Service para realizar una migración en línea requiere la creación de una instancia basada en el plan de tarifa Crítico para la empresa (versión preliminar).

## <a name="regional-availability"></a>Disponibilidad regional
Azure Database Migration Service está disponible actualmente en las siguientes regiones:

![Disponibilidad regional de Azure Database Migration Service](media\overview\dms-regional-availability1.png)

Para obtener la información más actualizada sobre la disponibilidad regional de Azure Database Migration Service, en el sitio de infraestructura global de Azure, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/).

## <a name="next-steps"></a>Pasos siguientes
- [Creación de una instancia de Azure Database Migration Service mediante Azure Portal](quickstart-create-data-migration-service-portal.md).
- [Migración de SQL Server a Azure SQL Database](tutorial-sql-server-to-azure-sql.md).
- [Información general sobre los requisitos previos para usar Azure Database Migration Service](pre-reqs.md).
- [Preguntas más frecuentes sobre el uso de Azure Database Migration Service](faq.md).
