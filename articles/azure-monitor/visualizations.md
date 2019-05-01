---
title: Visualizar datos de Azure Monitor | Microsoft Docs
description: Proporciona un resumen de los métodos disponibles para visualizar los datos de métricas y de registro almacenados en Azure Monitor.
author: bwren
manager: carmonm
editor: ''
services: azure-monitor
documentationcenter: azure-monitor
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/24/2018
ms.author: bwren
ms.openlocfilehash: 4e9bb9bd86e22efa5dd514e742481911b09c5e17
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61088295"
---
# <a name="visualizing-data-from-azure-monitor"></a>Visualizar datos de Azure Monitor
En este artículo se proporciona un resumen de los métodos disponibles para visualizar los datos de métricas y de registro almacenados en Azure Monitor.

Visualizaciones como gráficos y diagramas pueden ayudarlo a analizar los datos de supervisión para explorar en profundidad los problemas e identificar patrones. Según la herramienta que utilice, es posible que también tenga la opción de compartir visualizaciones con otros usuarios dentro y fuera de su organización.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="azure-dashboards"></a>Paneles de Azure
Los [paneles de Azure](../azure-portal/azure-portal-dashboards.md) son la tecnología de panel principal de Azure. Son especialmente útiles para proporcionar una hoja de cristal sobre su infraestructura de Azure y servicios, lo que permite identificar rápidamente problemas importantes.

![panel](media/visualizations/dashboard.png)

### <a name="advantages"></a>Ventajas
- Integración profunda en Azure. Las visualizaciones se pueden anclar a paneles desde varias páginas de Azure, incluido el Explorador de métricas, Log Analytics y Application Insights.
- Es compatible con las métricas y los registros.
- Combinar datos de varios orígenes, como resultado de [Explorador de métricas](platform/metrics-charts.md), [registrar consultas](log-query/log-query-overview.md), y [asigna](app/app-map.md) y la disponibilidad de Application Insights.
- Opción de paneles personales o compartidos. Integrado con la [autenticación basada en roles (RBAC)](../role-based-access-control/overview.md) de Azure.
- Actualización automática. Las métricas se actualizan según el intervalo de tiempo con un mínimo de cinco minutos. Los registros se actualizan cada minuto.
- Paneles de métricas parametrizadas con marca de tiempo y parámetros personalizados.
- Opciones de diseño flexibles.
- Modo de pantalla completa.


### <a name="limitations"></a>Limitaciones
- Control limitado sobre las visualizaciones de registros sin compatibilidad con tablas de datos. El número total de series de datos se limita a 10 con otras series de datos agrupadas en _otro_ depósito.
- Los gráficos de registro no son compatibles con los parámetros personalizados.
- Los gráficos de registro se limitan a los últimos 30 días.
- Los gráficos de registro solo se pueden anclar a los paneles compartidos.
- No hay interactividad con los datos del panel.
- Exploración en profundidad contextual limitada.

## <a name="azure-monitor-views"></a>Vistas de Azure Monitor
Las [vistas de Azure Monitor](platform/view-designer.md) le permiten crear visualizaciones personalizadas con datos de registro. Las [soluciones de supervisión](insights/solutions.md) las utilizan para presentar los datos recopilados.

![Ver](media/visualizations/view.png)

### <a name="advantages"></a>Ventajas
- Visualizaciones enriquecidas para datos de registro.
- Exporte e importe las vistas para transferirlas a otros grupos de recursos y suscripciones.
- Se integran en el modelo de administración de Azure Monitor con áreas de trabajo y soluciones de supervisión.
- [Filtran](platform/view-designer-filters.md) los parámetros personalizados.
- Interactivas, admiten varios niveles de obtención de detalles (vista que explora otra vista)

### <a name="limitations"></a>Limitaciones
- Admiten registros, pero no las métricas.
- No hay vistas personales. Disponibles para todos los usuarios con acceso al área de trabajo.
- No hay actualizaciones automáticas.
- Opciones de diseño limitadas.
- No se admiten las consultas a través de varias áreas de trabajo o aplicaciones de Application Insights.
- El tamaño de respuesta y el tiempo de ejecución de las consultas se limitan a 8 MB y 110 segundos, respectivamente.


## <a name="workbooks"></a>Workbooks
Los [libros](../azure-monitor/app/usage-workbooks.md) son documentos interactivos que proporcionan información detallada sobre los datos, la investigación y colaboración en el equipo. Algunos ejemplos de libros útiles son las guías de resolución de problemas y los análisis posteriores a incidentes.

![Libro](media/visualizations/workbook.png)

### <a name="advantages"></a>Ventajas
- Es compatible con las métricas y los registros.
- Admite parámetros que habilitan los informes interactivos en que, al seleccionar un elemento en una tabla, se actualizarán dinámicamente los gráficos y las visualizaciones asociados.
- Flujo similar al de los documentos.
- Opción de libros personales o compartidos.
- Experiencia de creación sencilla y colaborativa.
- Las plantillas admiten la galería de plantillas públicas basadas en GitHub.

### <a name="limitations"></a>Limitaciones
- No hay actualizaciones automáticas.
- No hay ningún diseño denso, como paneles, que haga que los libros tengan menos utilidad que un solo panel de vidrio. Diseñado especialmente para proporcionar información más detallada.


## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) es particularmente útil para crear informes y paneles centrados en los negocios, así como informes de análisis de tendencias de KPI a largo plazo. Puede [importar los resultados de una consulta de registro](platform/powerbi.md) en un conjunto de datos de Power BI para poder beneficiarse de sus características, como la combinación de datos de diferentes orígenes y el uso compartido de informes en la web y los dispositivos móviles.

![Power BI](media/visualizations/power-bi.png)

### <a name="advantages"></a>Ventajas
- Visualizaciones enriquecidas.
- Amplia interactividad, incluidas las funciones de acercamiento y filtrado cruzado.
- Fácil de compartir en toda la organización.
- Integración con otros datos desde varios orígenes de datos.
- Mejor rendimiento con los resultados almacenados en caché en un cubo.


### <a name="limitations"></a>Limitaciones
- Admiten registros, pero no las métricas.
- No hay integración de Azure. No puede administrar los paneles y los modelos a través de Azure Resource Manager.
- Los resultados de las consultas deben importarse en el modelo de Power BI para que se configuren. Limitación de tamaño de los resultados y de actualización.
- La actualización de datos se limita a ocho veces al día.


## <a name="grafana"></a>Grafana
[Grafana](https://grafana.com/) es una plataforma abierta que sobresale en paneles operativos. Resulta especialmente útil para detectar, aislar y priorizar los incidentes operativos. Puede agregar el [complemento de origen de datos de Azure Monitor en Grafana](platform/grafana-plugin.md) a su suscripción de Azure para visualizar los datos de métricas de Azure.

![Grafana](media/visualizations/grafana.png)

### <a name="advantages"></a>Ventajas
- Visualizaciones enriquecidas.
- Ecosistema enriquecido de orígenes de datos.
- Interactividad de datos que incluye la función de acercamiento.
- Es compatible con parámetros.

### <a name="limitations"></a>Limitaciones
- No hay integración de Azure. No puede administrar los paneles y los modelos a través de Azure Resource Manager.
- Costo para admitir una infraestructura Grafana adicional o costo adicional para Grafana Cloud.


## <a name="build-your-own-custom-application"></a>Compilar su propia aplicación personalizada
Puede tener acceso a datos de métricas y de registro en Azure Monitor mediante la API con cualquier cliente REST, lo que le permite crear sus propios sitios web y aplicaciones personalizados.

### <a name="advantages"></a>Ventajas
- Flexibilidad completa en cuanto a interfaz de usuario, visualización, interactividad y características.
- Combine las métricas y los datos de registro con otros orígenes de datos.

### <a name="disadvantages"></a>Desventajas
- Se requiere un trabajo de ingeniería importante.


## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre los [datos que Azure Monitor recopila](platform/data-platform.md).
- Obtenga información sobre los [paneles de Azure](../azure-portal/azure-portal-dashboards.md).
- Obtenga información sobre las [vistas de Azure Monitor](platform/view-designer.md).
- Obtenga información sobre [libros](../azure-monitor/app/usage-workbooks.md).
- Obtenga información sobre cómo [importar datos de registro en Power BI](../azure-monitor/platform/powerbi.md).
- Obtenga información sobre el [complemento de origen de datos de Azure Monitor en Grafana](../azure-monitor/platform/grafana-plugin.md).

