---
title: Recomendaciones de rendimiento en Azure Database for PostgreSQL
description: En este artículo se describen las recomendaciones de rendimiento que se pueden obtener en Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 6de302dbcfa9d6d1d2b311f41b03d8e54aeb63f6
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395447"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>Recomendaciones de rendimiento en Azure Database for PostgreSQL

**Se aplica a:** Azure Database for PostgreSQL 9.6 y 10

> [!IMPORTANT]
> Recomendaciones de rendimiento está en versión preliminar pública en una cantidad de regiones limitada.

La característica Recomendaciones de rendimiento identifica los índices principales que se pueden crear en el servidor de Azure Database for PostgreSQL para mejorar el rendimiento. Para generar recomendaciones de índices, la función tiene en cuenta diversas características de la base de datos, incluido su esquema y la carga de trabajo devueltos por el Almacén de consultas. Después de implementar cualquier recomendación de rendimiento, los clientes deben probar el rendimiento para evaluar el impacto de estos cambios. 

## <a name="permissions"></a>Permisos
Permisos de **Propietario** o **Colaborador** necesarios para ejecutar el análisis de uso de la característica Recomendaciones de rendimiento.

## <a name="performance-recommendations"></a>Recomendaciones de rendimiento
La característica [Recomendaciones de rendimiento](concepts-performance-recommendations.md) analiza las cargas de trabajo en el servidor para identificar los índices con el potencial de mejorar el rendimiento.

Abra **Recomendaciones de rendimiento** desde la sección **Soporte técnico y solución de problemas** de la barra de menús en la página de Azure Portal para el servidor PostgreSQL.

![Página de inicio de Recomendaciones de rendimiento](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

Seleccione **Analizar** y elija una base de datos. De este modo comenzará el análisis. Según la carga de trabajo, puede tardar varios minutos. Una vez que se realice el análisis, habrá una notificación en el portal.

La ventana **Recomendaciones de rendimiento** mostrará una lista de recomendaciones, si se detecta alguna. Una recomendación mostrará información sobre la **Base de datos**, la **Tabla**, la **Columna** y el **Tamaño del índice** correspondientes.

![Nueva página Recomendaciones de rendimiento](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Para implementar la recomendación, copie el texto de la consulta y ejecútelo desde el cliente que prefiera.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre la [supervisión y ajuste](concepts-monitoring.md) en Azure Database for PostgreSQL.

