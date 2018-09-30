---
title: Tutorial sobre la supervisión y ajuste en Azure Database for PostgreSQL
description: Este tutorial trata sobre la supervisión y el ajuste en Azure Database for PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: c21236026af960af0697978e49a0075fc9cf77ef
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960301"
---
# <a name="tutorial-monitor-and-tune-azure-database-for-postgresql"></a>Tutorial: Supervisión y ajuste en Azure Database for PostgreSQL

Azure Database for PostgreSQL tiene características que le ayudarán a comprender y mejorar el rendimiento del servidor. En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Habilitar las consultas y la recopilación de estadísticas de espera
> * Obtener acceso a los datos recopilados y usarlos
> * Ver el rendimiento de las consultas y estadísticas de espera a lo largo del tiempo
> * Analizar una base de datos para obtener recomendaciones de rendimiento
> * Aplicación de recomendaciones de rendimiento

## <a name="before-you-begin"></a>Antes de empezar
Necesita un servidor de Azure Database for PostgreSQL con PostgreSQL versión 9.6 o 10. Puede seguir los pasos del [tutorial de creación](tutorial-design-database-using-azure-portal.md) para crear un servidor.

> [!IMPORTANT]
> **Almacén de consultas**, **Información de rendimiento de consultas** y **Recomendaciones de rendimiento** están en la versión preliminar pública.

## <a name="enabling-data-collection"></a>Habilitación de la recopilación de datos
El [Almacén de consultas](concepts-query-store.md) captura un historial de las consultas y las estadísticas de espera en el servidor y lo almacena en la base de datos **azure_sys** en el servidor. Es una característica opcional. Para habilitarla:

1. Abra Azure Portal.

2. Seleccione su servidor de Azure Database for PostgreSQL.

3. Seleccione **Parámetros del servidor** que se encuentra en la sección **Configuración** del menú de la izquierda.

4. Establezca **pg_qs.query_capture_mode** en **TOP** para comenzar a recopilar los datos de rendimiento de las consultas. Establezca **pgms_wait_sampling.query_capture_mode** en **ALL** (TODAS) para comenzar a recopilar las estadísticas de espera. Haga clic en Save (Guardar).
   
   ![Parámetros del servidor de Almacén de consultas](./media/tutorial-performance-intelligence/query-store-parameters.png)

5. Deje hasta 20 minutos para el primer lote de datos que se va a conservar en la base de datos **azure_sys**.


## <a name="performance-insights"></a>Información detallada del rendimiento
La vista [Información de rendimiento de consultas](concepts-query-performance-insight.md) en Azure Portal mostrará la información clave del Almacén de consultas. 

1. En la página del portal del servidor Azure Database for PostgreSQL, seleccione **Información de rendimiento de consultas**, en la sección **Soporte técnico y solución de problemas** del menú de la izquierda.

2. La pestaña **Consultas de larga ejecución** muestra las 5 consultas principales por promedio de duración por ejecución, agregadas en intervalos de 15 minutos. 
   
   ![Página de inicio de Información de rendimiento de consultas](./media/tutorial-performance-intelligence/query-performance-insight-landing-page.png)

   Puede ver más consultas; para ello, selecciónelas en la lista desplegable **Número de consultas**. Cuando haga esto, los colores del gráfico pueden cambiar para un identificador de consulta específico.

3. Puede hacer clic y arrastrar en el gráfico para restringir a un período de tiempo específico.

4. Use los iconos para alejar y acercar a fin de ver un período de tiempo mayor o menor, respectivamente.

5. Consulte la tabla debajo del gráfico para obtener más información acerca de las consultas de larga ejecución en ese período.

6. Seleccione la pestaña **Estadísticas de espera** para ver las visualizaciones correspondientes de las esperas en el servidor.
   
   ![Estadística de esperas de Información de rendimiento de consultas](./media/tutorial-performance-intelligence/query-performance-insight-wait-statistics.png)

### <a name="permissions"></a>Permisos
Los permisos **Propietario** o **Colaborador** necesarios para ver el texto de las consultas en Información de rendimiento de consultas. El **lector** puede ver las tablas y gráficos, pero no el texto de las consultas.


## <a name="performance-recommendations"></a>Recomendaciones de rendimiento
La característica de [Recomendaciones de rendimiento](concepts-performance-recommendations.md) analiza las cargas de trabajo en el servidor para identificar los índices con el potencial de mejorar el rendimiento.

1. Abra **Recomendaciones de rendimiento** desde la sección **Soporte técnico y solución de problemas** de la barra de menús en la página de Azure Portal para el servidor PostgreSQL.
   
   ![Página de inicio de Recomendaciones de rendimiento](./media/tutorial-performance-intelligence/performance-recommendations-landing-page.png)

2. Seleccione **Analizar** y elija una base de datos. De este modo comenzará el análisis.

3. Según la carga de trabajo, puede tardar varios minutos. Una vez que se realice el análisis, habrá una notificación en el portal.

4. La ventana **Recomendaciones de rendimiento** mostrará una lista de recomendaciones, si se detecta alguna. 

5. Una recomendación mostrará información sobre la **Base de datos**, **Tabla**, **Columna** y **Tamaño del índice** correspondientes.

   ![Resultado de Recomendaciones de rendimiento](./media/tutorial-performance-intelligence/performance-recommendations-result.png)

6. Para implementar la recomendación, copie el texto de la consulta y ejecútelo desde el cliente que prefiera.

### <a name="permissions"></a>Permisos
Los permisos **Propietario** o **Colaborador** necesarios para ejecutar el análisis con la característica Recomendaciones de rendimiento.

## <a name="next-steps"></a>Pasos siguientes
- Más información acerca de la [supervisión y ajuste](concepts-monitoring.md) en Azure Database for PostgreSQL.