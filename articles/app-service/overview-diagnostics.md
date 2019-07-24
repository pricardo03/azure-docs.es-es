---
title: Introducción a los diagnósticos de Azure App Service | Microsoft Docs
description: Aprenda a solucionar los problemas con su aplicación con los diagnósticos de App Service.
keywords: app service, azure app service, diagnósticos, compatibilidad, aplicación web, solución de problemas, autoayuda
services: app-service
documentationcenter: ''
author: jen7714
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.custom: seodec18
ms.openlocfilehash: f2f798be85e9c3aeb8d4b54cba89d8be059427e0
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147329"
---
# <a name="azure-app-service-diagnostics-overview"></a>Introducción a los diagnósticos de Azure App Service

Cuando ejecute una aplicación web, querrá estar preparado para los posibles problemas que puedan surgir, desde errores 500 hasta que sus usuarios le comuniquen que su sitio está fuera de servicio. Los diagnósticos de App Service son una experiencia inteligente e interactiva que le ayuda a solucionar los problemas de su aplicación sin necesidad de configuración alguna. Cuando surgen problemas con la aplicación, los diagnósticos de App Service le indicarán lo que pasa y le guiarán a la información correcta de forma que pueda solucionar el problema de la manera más fácil y rápida.

Aunque esta experiencia es principalmente útil si tiene problemas con la aplicación en las últimas 24 horas, todos los gráficos de diagnóstico están siempre disponibles para su análisis.

El diagnóstico de App Service funciona no solo para la aplicación en Windows, sino también para las aplicaciones en [Linux/contenedores](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro), el [entono de App Service](https://docs.microsoft.com/azure/app-service/environment/intro) y [Azure functions](https://docs.microsoft.com/azure/azure-functions/functions-overview).

## <a name="open-app-service-diagnostics"></a>Apertura de los diagnósticos de App Service

Para acceder al diagnóstico de App Service, vaya a la aplicación web de App Service o a App Service Environment en [Azure Portal](https://portal.azure.com). En el panel izquierdo, haga clic en **Diagnosticar y solucionar problemas**.

Para Azure Functions, vaya a la aplicación de la función y, en el panel de navegación superior, haga clic en **Características de la plataforma** y seleccione **Diagnosticar y solucionar problemas** en la sección **Administración de recursos**.

En la página principal de diagnóstico de App Service, puede elegir la categoría que mejor describa el problema con la aplicación. Para ello, use las palabras clave de cada icono de la página principal. Además, en esta página encontrará las **Herramientas de diagnóstico** para aplicaciones de Windows. Consulte [Herramientas de diagnóstico (solo para la aplicación de Windows)](#diagnostic-tools-only-for-windows-app).

![Página principal](./media/app-service-diagnostics/app-service-diagnostics-homepage-1.png)

## <a name="interactive-interface"></a>Interfaz interactiva

Una vez que seleccione una categoría de la página principal que se corresponda mejor con el problema de la aplicación, la interfaz interactiva de diagnósticos de App Service (Genie) le guiará a través del diagnóstico y la resolución del problema con la aplicación. Puede usar los accesos directos de iconos que proporciona Genie para ver el informe de diagnóstico completo de la categoría del problema que le interesa. Los accesos directos de iconos proporcionan una manera directa de acceder a las métricas de diagnóstico.

![Accesos directos de iconos](./media/app-service-diagnostics/tile-shortcuts-2.png)

Después de hacer clic en estos iconos, podrá ver una lista de temas relacionados con el problema descrito en el icono. En estos temas se proporcionan fragmentos de información importante del informe completo. Puede hacer clic en cualquiera de estos temas para profundizar en los problemas. Además, puede hacer clic en **Ver el informe completo** para explorar todos los temas en una sola página.

![Temas](./media/app-service-diagnostics/application-logs-insights-3.png)

![Ver el informe completo](./media/app-service-diagnostics/view-full-report-4.png)

## <a name="diagnostic-report"></a>Informe de diagnóstico

Si decide hacer clic en un tema para profundizar en el problema, obtendrá más detalles sobre el tema, a menudo complementados con gráficos y marcas. El informe de diagnóstico puede ser una herramienta eficaz para identificar el problema con la aplicación.

![Informe de diagnóstico](./media/app-service-diagnostics/full-diagnostic-report-5.png)

## <a name="health-checkup"></a>Comprobación de mantenimiento

Si no sabe cuál es el problema con la aplicación o no sabe por dónde empezar a solucionarlo, un buen lugar para comenzar es la comprobación de mantenimiento. La comprobación de mantenimiento analiza las aplicaciones para ofrecerle una visión general rápida e interactiva de lo que funciona bien y mal y dónde mirar para investigar el problema. Su interfaz inteligente e interactiva le proporciona una guía por el proceso de solución de problemas. La comprobación de mantenimiento está integrada con la experiencia de Genie para aplicaciones de Windows y el informe de diagnóstico de aplicaciones web fuera de servicio para aplicaciones de Linux.

### <a name="health-checkup-graphs"></a>Gráficos de comprobación de mantenimiento

Hay cuatro gráficos diferentes en la comprobación de mantenimiento.

- **Solicitudes y errores:** gráfico en el que se muestra el número de solicitudes realizadas durante las últimas 24 horas, junto con errores del servidor HTTP.
- **Rendimiento de la aplicación:** gráfico en el que se muestra el tiempo de respuesta durante las últimas 24 horas para diversos grupos de percentiles.
- **Uso de CPU:** gráfico en el que se muestra el porcentaje general de uso de la CPU por instancia durante las últimas 24 horas.  
- **Uso de la memoria:** gráfico en el que se muestra el porcentaje general de uso de la memoria física por instancia durante las últimas 24 horas.

![Comprobación de mantenimiento](./media/app-service-diagnostics/health-checkup-6.png)

### <a name="investigate-application-code-issues-only-for-windows-app"></a>Investigación de los problemas de código de aplicación (solo para la aplicación de Windows)

Dado que muchos problemas de la aplicación están relacionados con el código de esta, el diagnóstico de App Service se integra con [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) para resaltar las excepciones y los problemas de dependencia para ponerlos en correlación con el tiempo de inactividad seleccionado. Application Insights deben estar habilitado por separado.

![Application Insights](./media/app-service-diagnostics/application-insights-7.png)

Para ver las excepciones y las dependencias de Application Insights, seleccione los métodos abreviados de icono **Aplicación web fuera de servicio** o **Aplicación web lenta**.

### <a name="troubleshooting-steps-only-for-windows-app"></a>Pasos para solucionar problemas (solo para la aplicación de Windows)

Si se detecta un problema de una categoría determinada en las últimas 24 horas, puede ver el informe de diagnóstico completo y los diagnósticos de App Service pueden pedirle que vea más consejos de solución de problemas para guiarle mejor en su solución.

![Application Insights y Solución de problemas y siguientes pasos](./media/app-service-diagnostics/troubleshooting-and-next-steps-8.png)

## <a name="diagnostic-tools-only-for-windows-app"></a>Herramientas de diagnóstico (solo para la aplicación de Windows)

Las Herramientas de diagnóstico incluyen herramientas de diagnóstico más avanzadas que le ayudan a investigar los problemas del código de la aplicación, la lentitud, las cadenas de conexión, así como herramientas proactivas que le permiten mitigar los problemas relacionados con el uso de la CPU, las solicitudes y la memoria.

### <a name="proactive-cpu-monitoring"></a>Supervisión de CPU proactiva

La supervisión de CPU proactiva proporciona una manera fácil y proactiva de realizar una acción cuando la aplicación o el proceso secundario de la aplicación consumen recursos elevados de la CPU. Puede establecer sus propias reglas de umbral de la CPU para mitigar temporalmente un problema de uso elevado de la CPU hasta que se detecte la causa real del problema.

![Supervisión de CPU proactiva](./media/app-service-diagnostics/proactive-cpu-monitoring-9.png)

### <a name="proactive-auto-healing"></a>Recuperación automática proactiva

Al igual que la supervisión de CPU proactiva, la recuperación automática proactiva ofrece un enfoque sencillo y proactivo para mitigar un comportamiento inesperado de la aplicación. Puede establecer sus propias reglas basadas en número de solicitudes, una solicitud lenta, el límite de memoria y el código de estado HTTP para desencadenar acciones de mitigación. Esta herramienta puede usarse para mitigar temporalmente un comportamiento inesperado hasta que se detecte la causa real del problema. Para obtener más información sobre la recuperación automática proactiva, visite [Announcing the new auto healing experience in app service diagnostics](https://azure.github.io/AppService/2018/09/10/Announcing-the-New-Auto-Healing-Experience-in-App-Service-Diagnostics.html) (Anuncio de la nueva experiencia de recuperación automática en los diagnósticos de App Service).

![Recuperación automática proactiva](./media/app-service-diagnostics/proactive-auto-healing-10.png)

## <a name="change-analysis-only-for-windows-app"></a>Análisis de cambios (solo para la aplicación de Windows)

En un entorno de desarrollo muy dinámico, a veces puede ser difícil llevar un seguimiento de todos los cambios que se realizan en la aplicación, por no hablar de detectar un cambio que ha provocado un comportamiento incorrecto. El análisis de cambios puede ayudarle a restringir los cambios que se han realizado en la aplicación para facilitar la experiencia de solución de problemas. El análisis de cambios se encuentra en **Cambios en la aplicación** y está insertado también en informes de diagnóstico como **Bloqueos de aplicación**, de modo que pueda usarlo simultáneamente con otras métricas.

El análisis de cambios debe habilitarse antes de usar la característica. Para obtener más información sobre el análisis de cambios, visite [Announcing the new change analysis experience in App Service Diagnostics](https://azure.github.io/AppService/2019/05/07/Announcing-the-new-change-analysis-experience-in-App-Service-Diagnostics-Analysis.html) (Anuncio de la nueva experiencia de análisis de cambios en los diagnósticos de App Service).

![Página predeterminada del análisis de cambios](./media/app-service-diagnostics/change-analysis-default-page-11.png)

![Vista diferente](./media/app-service-diagnostics/diff-view-12.png)