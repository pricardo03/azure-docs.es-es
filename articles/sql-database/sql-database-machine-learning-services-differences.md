---
title: Diferencias principales para Azure SQL Database Machine Learning Services (versión preliminar)
description: En este tema se describen las principales diferencias entre Azure SQL Database Machine Learning Services (con R) y SQL Server Machine Learning Services.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: 57ea52c179376e8378680f436d396ffaf9357f68
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57771857"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-preview-and-sql-server"></a>Diferencias clave entre los servicios de Machine Learning en Azure SQL Database (versión preliminar) y SQL Server

La funcionalidad de Azure SQL Database Machine Learning Services (con R) (versión preliminar) es similar a [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). A continuación se muestran algunas diferencias clave.

> [!IMPORTANT]
> Servicios de Azure SQL Database Machine Learning está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="language-support"></a>Compatibilidad con idiomas

SQL Server es compatible con R y Python gracias al [marco de extensibilidad](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework). SQL Database no admite ambos lenguajes. Las principales diferencias son:

- R es el único lenguaje que se admite en SQL Database. En este momento no hay ninguna compatibilidad con Python.
- La versión de R es la 3.4.4.
- No es necesario configurar `external scripts enabled` mediante `sp_configure`. Una vez que esté [registrado](sql-database-machine-learning-services-overview.md#signup), el aprendizaje automático estará habilitado para su base de datos SQL.

## <a name="package-management"></a>Administración de paquetes

La instalación y administración de paquetes de R funciona de forma distinta para SQL Database y SQL Server. Las diferencias son:

- Los paquetes de R se instalan a través de [sqlmlutils](https://github.com/Microsoft/sqlmlutils) o [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Los paquetes no pueden realizar llamadas de red salientes. Esta limitación es similar a la [predeterminado de las reglas de firewall de Machine Learning Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) en SQL Server, pero no se puede cambiar en la base de datos SQL.
- No hay ninguna compatibilidad con los paquetes que dependen de los entornos de ejecución externos (por ejemplo, Java) o que necesitan tener acceso a las API del sistema operativo para instalarse o usarse.

## <a name="resource-governance"></a>Gobernanza de recursos

No es posible limitar los recursos de R a través de [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) ni de grupos de recursos externos.

Durante la versión preliminar pública, recursos de R se establecen en un máximo de 20% de los recursos de base de datos SQL y dependen en qué nivel de servicio elija. Para obtener más información, consulte [Azure SQL Database purchasing models](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) (Modelos de compra de Azure SQL Database).

### <a name="insufficient-memory-error"></a>Error de memoria insuficiente

Si hay suficiente memoria disponible para R, obtendrá un mensaje de error. Mensajes de error comunes son:

- No se puede comunicar con el tiempo de ejecución de script 'R' para el Id. de solicitud: ***. Compruebe los requisitos de tiempo de ejecución 'R'
- Se ha producido un error de script 'R' durante la ejecución de 'sp_execute_external_script' con HRESULT 0 x 80004004. ... se produjo un error de script externo: ".. no se pudo asignar memoria (0 Mb) en función de C 'R_AllocStringBuffer' "
- Se produjo un error de script externo: Error: no se puede asignar el vector de tamaño.

Uso depende de la cantidad de memoria se utiliza en los scripts de R y el número de consultas en paralelo que se está ejecutando. Si recibe los errores anteriores, puede escalar la base de datos a un mayor nivel de servicio para resolver este problema.

## <a name="next-steps"></a>Pasos siguientes

- Consulte la documentación sobre [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics) para obtener información general al respecto.
- Para obtener información sobre cómo usar Machine Learning Services (con R) en Azure SQL Database, consulte la [guía de inicio rápido](sql-database-connect-query-r.md).
- Obtenga más información con los [tutoriales del lenguaje R de SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials).