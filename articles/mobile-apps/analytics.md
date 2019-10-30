---
title: Comprensión del uso de las aplicaciones móviles y del comportamiento de los usuarios con Visual Studio App Center y servicios de Azure
description: Conozca los servicios de App Center que le ayudan a comprender el uso que los usuarios hacen de sus aplicaciones móviles y así poder tomar decisiones empresariales inteligentes.
author: elamalani
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 10/22/2019
ms.author: emalani
ms.openlocfilehash: d60e9437a2f33788eb183cfcad0f3a10d71fb79d
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72795180"
---
# <a name="analyze-and-understand-usage-of-mobile-applications"></a>Análisis y comprensión del uso de las aplicaciones móviles
¿Comprende bien cómo los usuarios usan las aplicaciones? ¿Cuántos usuarios activos tiene la aplicación y cómo cambia el uso con el tiempo? ¿Qué características usan y cuáles son las más utilizadas? ¿Dónde se encuentran estos usuarios? ¿Cuántos usuarios usan la versión más reciente de la aplicación? Todas estas preguntas son importantes para comprender el modo de convertir la aplicación en un éxito. Para responder a estas clases de preguntas de análisis de uso, debe recopilar datos de uso de las aplicaciones.

Cuanto más estudie los datos, más formas se podrán encontrar de mejorar la aplicación y mantener contentos a los usuarios. Es importante usar los datos para buscar información detallada sobre la que poder actuar y que los usuarios estén satisfechos.

## <a name="importance-of-analytics"></a>Importancia del análisis
- **Comprenda** a los usuarios, cómo interactúan con la aplicación y qué les hace volver para ajustar la aplicación y proporcionar excelentes experiencias que hagan crecer su negocio.
- **Realice un seguimiento** de las métricas de uso para tomar decisiones fundamentadas sobre cómo comercializar su aplicación y ofrecer un mejor servicio a su cliente.
- **Mida** el rendimiento de la aplicación.
- **Aprenda** qué partes de la aplicación generan valor y rendimiento.
- **Información detallada orientada a datos** en problemas relacionados con el abandono y la retención de clientes.

Use los servicios siguientes para habilitar el análisis de las aplicaciones móviles.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Analytics](/appcenter/analytics/) le permite aumentar su público al centrarse en lo que importa, con informes y perspectivas detallados sobre las sesiones de usuario, los dispositivos principales, las versiones del sistema operativo y el análisis del comportamiento. Cree eventos personalizados con facilidad para realizar el seguimiento de todo con análisis de aplicaciones completos.

   **Características principales**
   - **Realice un seguimiento** de los patrones de uso, la adopción de usuarios y otras métricas de participación de forma gratuita.
   - **Identifique** tendencias, el comportamiento de los usuarios y la participación mediante eventos personalizados.
   - **Métricas preparadas** e **información detallada** sobre el uso de las aplicaciones (diario, semanal, mensual), sesiones, propiedades de los dispositivos y datos demográficos de los usuarios en un **único panel**.
   - **Exportación continua de todos los datos de análisis a Azure** para conservarlos de forma ilimitada. Compatible con la exportación a Azure Blob Storage y Azure Application Insights.
   - **Integración con Azure Application Insights** para obtener información aún más detallada, como la retención, el análisis de embudo y los cohortes.
   - **Integración rápida del SDK** para empezar en cuestión de minutos.
   - **Compatibilidad con plataformas**: iOS, Android, macOS, tvOS, Xamarin, React Native, Unity y Cordova.

   **Referencias**
   - [Registro en App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
   - [Introducción a App Center Analytics](/appcenter/analytics/)

## <a name="azure-monitor"></a>Azure Monitor
Azure Monitor incluye [Application Insights](/azure/azure-monitor/app/app-insights-overview) que proporciona herramientas para recopilar y analizar la telemetría con el fin de maximizar el rendimiento y supervisar la aplicación móvil. Puede aprovechar las ventajas de Application Insights con App Center Analytics y configurar la exportación a Application Insights. Application Insights permite consultar, segmentar, filtrar y analizar la telemetría de eventos personalizada desde las aplicaciones, de forma más exhaustiva que las herramientas de análisis que App Center ofrece.

**Características principales**
   - **Consulta** de la telemetría de eventos personalizados.
   - **Filtrado** de la telemetría de eventos con funcionalidades de segmentación eficaces.
   - **Análisis** de los patrones de conversión, retención y navegación de la aplicación.
     - **Embudos** para analizar y supervisar las tasas de conversión.
     - **Retención** para analizar el grado en que la aplicación retiene a los usuarios con el paso del tiempo.
     - **Workbooks** para combinar visualizaciones y texto en un informe que se pueda compartir.
     - **Cohortes** para nombrar y guardar grupos de usuarios o eventos específicos, para poder hacer referencia a ellos fácilmente desde otras herramientas de análisis.

**Referencias**
- [Azure Portal](https://portal.azure.com/)
- [Análisis de la aplicación móvil con App Center y Application Insights](/azure/azure-monitor/learn/mobile-center-quickstart)
- [Análisis de uso con Application Insights](/azure/azure-monitor/app/usage-overview)

## <a name="azure-playfab"></a>Azure PlayFab
[Azure PlayFab](https://playfab.com/) ofrece una plataforma de back-end completa con servicios de juegos, análisis en tiempo real y LiveOps que necesita para crear juegos conectados a la nube de nivel mundial. Estos servicios reducen las barreras de lanzamiento de los desarrolladores de juegos y ofrecen soluciones de desarrollo rentables tanto para estudios grandes como pequeños que se escalan con sus juegos y les ayudan a atraer, conservar y monetizar a los jugadores. PlayFab permite a los desarrolladores usar la nube inteligente para compilar y gestionar los juegos, analizar los datos de juego y mejorar las experiencias de juego en general.

**Características principales**
   - **Supervisión** de paneles en tiempo real.
   - **Evaluación** del rendimiento del juego a través de las principales métricas.
   - **Informes generados automáticamente** para revisar los resúmenes del rendimiento diario y mensual del juego que pueden verse en el administrador de juegos y descargarse o entregarse en la bandeja de entrada diariamente.
   - **Pruebas A/B** para ejecutar experimentos y determinar el valor óptimo para una variable determinada.
   - **Segmentación** para que los jugadores puedan definir agrupaciones automatizadas de jugadores.
    
**Referencias**
- [Portal de PlayFab](https://developer.playfab.com/en-US/sign-up)
- [Analytics](/gaming/playfab/#pivot=documentation&panel=analytics)
- [Guías de inicio rápido](/gaming/playfab/#pivot=documentation&panel=quickstarts)    
