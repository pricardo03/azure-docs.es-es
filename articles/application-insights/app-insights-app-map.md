---
title: Mapa de aplicación en Azure Application Insights | Microsoft Docs
description: Supervisión de topologías de aplicaciones complejas con el mapa de aplicación
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 9b39eef5accec4764f61ab31dd894d368242ee3d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094657"
---
# <a name="application-map-triage-distributed-applications"></a>Mapa de aplicación: evaluación de prioridades de aplicaciones distribuidas

El mapa de aplicación le ayuda a detectar los cuellos de botella en el rendimiento o las zonas activas con error en todos los componentes de la aplicación distribuida. Cada nodo del mapa representa un componente de aplicación o sus dependencias, e incluye el KPI de mantenimiento y el estado de alerta. Puede hacer clic a través de cualquier componente para ver un diagnóstico más detallado, como los eventos de Application Insights. Si su aplicación usa los servicios de Azure, también puede hacer clic por los diagnósticos de Azure, como las recomendaciones SQL Database Advisor.

## <a name="what-is-a-component"></a>¿Qué es un componente?

Los componentes son partes que se pueden implementar independientemente de su aplicación de microservicios o distribuida. Los equipos de operaciones y los desarrolladores pueden ver el código o acceder a la telemetría que generan estos componentes de la aplicación. 

* Los componentes son diferentes de las dependencias externas "observadas", como SQL, EventHub, etc. a las que su equipo u organización no pueden acceder (código o telemetría).
* Los componentes se ejecutan en cualquier número de instancias de rol, servidor o contenedor.
* Los componentes pueden ser claves de instrumentación de Application Insights independientes (incluso aunque las suscripciones sean diferentes) o diferentes roles que informan a una única clave de instrumentación de Application Insights. La experiencia de mapa de versión preliminar muestra los componentes con independencia de cómo están configurados.

## <a name="composite-application-map"></a>Mapa de aplicación compuesta

Puede ver la topología de aplicación completa a lo largo de varios niveles de componentes de aplicación relacionados. Los componentes podrían ser diferentes recursos de Application Insights o distintos roles de un único recurso. Para encontrar componentes, el mapa de aplicación sigue las llamadas de dependencia HTTP entre los servidores con el SDK de Application Insights instalado. 

Esta experiencia comienza con la detección progresiva de los componentes. La primera vez que carga el mapa de aplicación, se desencadena un conjunto de consultas para detectar los componentes relacionados con este componente. Un botón en la esquina superior izquierda se actualiza con el número de componentes de la aplicación a medida que se detectan. 

Al hacer clic en "Update map components" (Actualizar componentes del mapa), el mapa se actualiza con todos los componentes detectados hasta ese momento.

Si todos los componentes son roles dentro de un único recurso de Application Insights, este paso de detección no es necesario. La carga inicial para este tipo de aplicación tendrá todos sus componentes.

![Captura de pantalla de mapa de aplicación](media/app-insights-app-map/001.png)

Uno de los objetivos principales de la experiencia es poder visualizar topologías complejas con cientos de componentes.

Haga clic en cada componente para ver información detallada relacionada e ir a la experiencia de evaluación de prioridades de rendimiento y errores de ese componente.

![Control flotante](media/app-insights-app-map/application-map-001.png)

### <a name="investigate-failures"></a>Investigar los errores

Seleccione **Investigar los errores** para iniciar el panel de errores.

![Captura de pantalla del botón Investigar los errores](media/app-insights-app-map/investigate-failures.png)

![Captura de pantalla de la experiencia de errores](media/app-insights-app-map/failures.png)

### <a name="investigate-performance"></a>Investigar el rendimiento

Para solucionar problemas de rendimiento seleccione **Investigar el rendimiento**

![Captura de pantalla del botón Investigar el rendimiento](media/app-insights-app-map/investigate-performance.png)

![Captura de pantalla de la experiencia de rendimiento](media/app-insights-app-map/performance.png)

### <a name="go-to-details"></a>Ir a los detalles

Seleccione **Ir a los detalles** para explorar una experiencia de transacción completa que puede ofrecer las vistas realizadas en el nivel de la pila de llamadas.

![Captura de pantalla del botón Ir a los detalles](media/app-insights-app-map/go-to-details.png)

![Captura de pantalla de los detalles de una transacción completa](media/app-insights-app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Ver en Analytics

Para consultar e investigar aún más los datos de aplicaciones, haga clic **Ver en Analytics**.

![Captura de pantalla del botón Ver en Analytics](media/app-insights-app-map/view-in-analytics.png)

![Captura de pantalla de la experiencia con Analytics](media/app-insights-app-map/analytics.png)

### <a name="alerts"></a>Alertas

Para ver las alertas activas y las reglas subyacentes que provocan el desencadenamiento de estas, seleccione **alertas**.

![Captura de pantalla del botón de alertas](media/app-insights-app-map/alerts.png)

![Captura de pantalla de la experiencia con Analytics](media/app-insights-app-map/alerts-view.png)

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Comentarios
Proporcione sus comentarios mediante la opción de comentarios del portal.

![Imagen MapLink-1](./media/app-insights-app-map/13.png)

## <a name="next-steps"></a>Pasos siguientes

* [Azure Portal](https://portal.azure.com)
