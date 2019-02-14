---
title: Introducción a las principales diferencias de Machine Learning Services (con R) en Azure SQL Database (versión preliminar)
description: En este tema se describen las principales diferencias entre Azure SQL Database Machine Learning Services (con R) y SQL Server Machine Learning Services.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 01/31/2019
ms.openlocfilehash: adc303e9b375aaa6f37e9e79ea5434675c75523d
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55825032"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-and-sql-server"></a>Principales diferencias entre Machine Learning Services de Azure SQL Database y SQL Server

La funcionalidad de Machine Learning Services (con R) en Azure SQL Database es similar a [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). A continuación se describen algunas de las principales diferencias entre ellas.

## <a name="language-support"></a>Compatibilidad con idiomas

SQL Server es compatible con R y Python gracias al [marco de extensibilidad](https://docs.microsoft.com/en-us/sql/advanced-analytics/concepts/extensibility-framework). SQL Database no admite ambos lenguajes. Las principales diferencias son:

- R es el único lenguaje que se admite en SQL Database. En este momento no hay ninguna compatibilidad con Python.
- La versión de R es la 3.4.4.
- No es necesario configurar `external scripts enabled` mediante `sp_configure`. Una vez que esté [registrado](sql-database-machine-learning-services-overview.md#signup), el aprendizaje automático estará habilitado para su base de datos SQL.

## <a name="package-management"></a>Administración de paquetes

La instalación y administración de paquetes de R funciona de forma distinta para SQL Database y SQL Server. Las diferencias son:

- Los paquetes de R se instalan a través de [sqlmlutils](https://github.com/Microsoft/sqlmlutils) o [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Los paquetes no pueden realizar llamadas de red salientes. Esta limitación es similar a las [reglas predeterminadas de firewall de Machine Learning Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) en SQL Server, pero no se pueden cambiar en SQL Database.
- No hay ninguna compatibilidad con los paquetes que dependen de los entornos de ejecución externos (por ejemplo, Java) o que necesitan tener acceso a las API del sistema operativo para instalarse o usarse.

## <a name="resource-governance"></a>Regulador de recursos

No es posible limitar los recursos de R a través de [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ni de grupos de recursos externos. Los recursos de R son un porcentaje de los recursos de SQL Database y dependen del nivel de servicio que elija. Para obtener más información, consulte [Azure SQL Database purchasing models](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) (Modelos de compra de Azure SQL Database).

## <a name="security-isolation"></a>Aislamiento de seguridad

En Azure SQL Database, la capa de abstracción de la plataforma de SQL (SQLPAL) proporciona aislamiento para los procesos externos. Este aislamiento proporciona una capa adicional de seguridad para ejecutar scripts de R.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la documentación sobre [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics) para obtener información general al respecto.
- Para obtener información sobre cómo usar Machine Learning Services (con R) en Azure SQL Database, consulte la [guía de inicio rápido](sql-database-connect-query-r.md).
- Obtenga más información con los [tutoriales del lenguaje R de SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials).