---
title: 'Recomendaciones de rendimiento en Azure Database for PostgreSQL: servidor único'
description: 'En este artículo se describe la característica de recomendaciones de rendimiento en Azure Database for PostgreSQL: servidor único.'
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 31d8c0fdf1b4df3ee00f3652c933b4b738384bea
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65068841"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql---single-server"></a>Recomendaciones de rendimiento en Azure Database for PostgreSQL: servidor único

**Se aplica a:** Azure Database for PostgreSQL: servidor único 9.6 y 10

La característica de las recomendaciones de rendimiento analiza las bases de datos para la creación de sugerencias personalizadas para mejorar el rendimiento. Para generar las recomendaciones, el análisis se examinan diversas características de base de datos, incluido el esquema. Habilitar [Query Store](concepts-query-store.md) en el servidor para poder utilizar la característica de las recomendaciones de rendimiento. Después de implementar las recomendaciones de rendimiento, debe probar el rendimiento para evaluar el impacto de estos cambios. 

## <a name="permissions"></a>Permisos
Los permisos **Propietario** o **Colaborador** necesarios para ejecutar el análisis con la característica Recomendaciones de rendimiento.

## <a name="performance-recommendations"></a>Recomendaciones de rendimiento
La característica [Recomendaciones de rendimiento](concepts-performance-recommendations.md) analiza las cargas de trabajo en el servidor para identificar los índices con el potencial de mejorar el rendimiento.

Abra **recomendaciones de rendimiento** desde el **rendimiento inteligente** sección de la barra de menús en la página de portal de Azure para el servidor de PostgreSQL.

![Página de inicio de Recomendaciones de rendimiento](./media/concepts-performance-recommendations/performance-recommendations-page.png)

Seleccione **analizar** y elija una base de datos que se iniciará el análisis. Dependiendo de la carga de trabajo, análisis th pueden tardar varios minutos en completarse. Una vez que se realice el análisis, habrá una notificación en el portal. Análisis realiza un examen en profundidad de la base de datos. Se recomienda que realizar análisis durante los períodos de poca actividad. 

El **recomendaciones** ventana mostrará una lista de recomendaciones si se detecta alguno.

![Nueva página Recomendaciones de rendimiento](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Las recomendaciones no se aplican automáticamente. Para aplicar la recomendación, copie el texto de consulta y ejecútelo desde el cliente de elección. No olvide probar y supervisar para evaluar la recomendación. 

## <a name="recommendation-types"></a>Tipos de recomendaciones

Actualmente, se admiten dos tipos de recomendaciones: *Crear índice* y *quitar índice*.

### <a name="create-index-recommendations"></a>Recomendaciones para crear índice
*Crear índice* recomendaciones sugieren nuevos índices para acelerar las consultas con más frecuencia se ejecute o que requieren mucho tiempo en la carga de trabajo. Este tipo de recomendación requiere [Query Store](concepts-query-store.md) esté habilitado. Query Store recopila información de la consulta y proporciona las estadísticas de tiempo de ejecución y la frecuencia de consulta detallada que el análisis se usa para realizar la recomendación.

### <a name="drop-index-recommendations"></a>Recomendaciones para quitar índice
Además de detectar índices que faltan,-Azure Database for PostgreSQL analiza el rendimiento de los índices existentes. Si un índice es rara vez utilizado o redundantes, el analizador recomienza descartarlo.


## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de la [supervisión y ajuste](concepts-monitoring.md) en Azure Database for PostgreSQL.

