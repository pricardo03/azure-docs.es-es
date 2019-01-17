---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: scottwhi-msft
ms.service: cognitive-services
ms.topic: include
ms.date: 04/09/2018
ms.author: scottwhi
ms.custom: include file
ms.openlocfilehash: 6016b13fe7d3e1f3b673bd2446d2f68b04878cd6
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54193479"
---
Las estadísticas de Bing proporcionan análisis para Bing Search APIs. El análisis incluye el volumen de llamadas, las cadenas de consulta principales, la distribución geográfica y mucho más. Para habilitar Estadísticas de Bing en la suscripción de pago de Bing Search, vaya al [panel de Azure](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7), seleccione la suscripción de pago y haga clic en Habilitar Estadísticas de Bing. Si habilita Estadísticas de Bing, la tasa de suscripción se incrementa ligeramente (consulte los [precios](https://aka.ms/bingstatisticspricing)).

> [!NOTE]
> Estadísticas de Bing solo está disponible con suscripciones de pago. No está disponible con suscripciones de evaluación gratuita.

> [!NOTE]
> No se puede usar ningún dato disponible a través del panel de Estadísticas de Bing para crear aplicaciones y distribuirlas a terceros.

Bing actualiza los datos de análisis cada 24 horas y mantiene un historial de 13 meses.

## <a name="accessing-your-analytics"></a>Obtención de acceso a los análisis

Para obtener acceso al panel de análisis, vaya a https://bingapistatistics.com. Asegúrese de que ha iniciado sesión con la misma cuenta de Microsoft (MSA) que utiliza para obtener una suscripción de pago.

## <a name="filtering-the-data"></a>Filtración de los datos

De forma predeterminada, los diagramas y gráficos reflejan todos los datos de las métricas a los que se tiene acceso. Para filtrar los datos que se muestran en los diagramas y gráficos, seleccione los recursos, los mercados, los puntos de conexión y el período de informe que le interesa. Los diagramas y gráficos cambian para reflejar los filtros que aplica. A continuación, se describen los filtros que puede cambiar.

- **Identificador del recurso**: identificador de recurso único que identifica la suscripción de Azure. La lista contiene varios id. si se suscribe a más de un nivel de Bing Search APIs. De forma predeterminada, se seleccionan todos los recursos.  
  
- **Mercados**: mercados de los que proceden los resultados. Por ejemplo, en-us (inglés de los Estados Unidos). De forma predeterminada, se seleccionan todos los mercados. Tenga en cuenta que el mercado en-WW es el que Bing usa si la llamada no especifica un mercado y Bing no puede determinar el mercado del usuario.  
  
- **Puntos de conexión**: puntos de conexión de Bing Search APIs. La lista contiene todos los puntos de conexión para los cuales tiene una suscripción de pago. De forma predeterminada, se seleccionan todos los puntos de conexión.  

- **Período de tiempo**: período de notificación. Puede especificar:
  - Todos&mdash;, incluye datos de hasta 13 meses  
  - Últimas 24 horas&mdash;, incluye el análisis de las últimas 24 horas.  
  - Última semana&mdash;, incluye el análisis de los siete días anteriores.  
  - Último mes&mdash;, incluye el análisis de los treinta días anteriores.  
  - Un intervalo de fechas personalizado&mdash;, incluye el análisis del intervalo de fechas especificado, si está disponible.  

  > [!NOTE]  
  > Las métricas pueden tardar hasta 24 horas en aparecer en el panel. El panel muestra la fecha y hora en la que los datos se actualizaron por última vez.  

  > [!NOTE]  
  > Las métricas están disponibles desde el momento en que se habilita el complemento de Estadísticas de Bing.

## <a name="charts-and-graphs"></a>Tablas y gráficos

El panel muestra las tablas y gráficos de las métricas disponibles para el punto de conexión seleccionado. No todas las métricas están disponibles para todos los puntos de conexión. Los diagramas y gráficos para cada punto de conexión son estáticos (no se pueden seleccionar los diagramas y gráficos que se muestran). El panel muestra solo los diagramas y gráficos para los que existen datos.

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

A continuación, se muestran las métricas posibles. Cada métrica considera las restricciones del punto de conexión.

- **Volumen de llamadas**: muestra el número de llamadas realizadas durante el período de notificación. Si el período de informe es de un día, el gráfico muestra el número de llamadas realizadas por hora. En caso contrario, el gráfico muestra el número de llamadas realizadas por día del período de informe.  
  
  > [!NOTE]
  > El volumen de llamada puede diferir de los informes de facturación, que generalmente incluye solo las llamadas correctas.

- **Principales consultas**: muestra las consultas principales y el número de repeticiones de cada consulta durante el período de notificación. Se puede configurar el número de consultas que se muestran. Por ejemplo, puede mostrar las 25, 50 o 75 consultas principales. Las consultas principales no están disponibles para los siguientes punto de conexión:  

  - /images/trending
  - /images/details
  - /images/visualsearch
  - /videos/trending
  - /videos/details
  - /news
  - /news/trendingtopics
  - /suggestions  
  
  > [!NOTE]  
  > Algunos términos de consulta se pueden suprimir para quitar información confidencial, como el correo electrónico, los números de teléfono, el número de seguridad social, etcétera.

- **Distribución geográfica**: mercados de los que proceden los resultados. Por ejemplo, en-us (inglés de los Estados Unidos). Bing usa el parámetro de consulta `mkt` para determinar el mercado, si se especifica. En caso contrario, Bing usa señales como la dirección IP del llamador para determinar el mercado.

- **Distribución de códigos de respuesta**: códigos de estado HTTP de todas las llamadas durante el período de notificación.

- **Distribución del origen de llamada**: tipos de exploradores utilizados por los usuarios. Por ejemplo, Microsoft Edge, Chrome, Safari y FireFox. Las llamadas realizadas desde fuera de un explorador, como bots, Postman o mediante cURL desde una aplicación de consola, se agrupan en Bibliotecas. El origen se determina mediante el valor del encabezado User-Agent de la solicitud. Si la solicitud no incluye el encabezado User-Agent, Bing intenta derivar el origen de otras señales.  

- **Distribución de búsqueda segura**: distribución de valores de búsqueda segura. Por ejemplo, desactivado, moderado o estricto. El parámetro de consulta `safeSearch` contiene el valor, si se especifica. En caso contrario, Bing establece el valor de forma predeterminada en moderado.  

- **Distribución de respuestas solicitadas**: Web Search API responde a la solicitud del parámetro de consulta `responseFilter`.  

- **Distribución de respuestas devueltas**: respuestas que devuelve Web Search API en los resultados.

- **Distribución del servidor de respuestas**: servidor de la aplicación que atiende las solicitudes de API. Los valores posibles son Bing.com (para el tráfico procedente de los dispositivos de escritorio y portátiles) y Bing.com-mobile (para el tráfico procedente de los dispositivos móviles). El servidor se determina mediante el valor del encabezado User-Agent de la solicitud. Si la solicitud no incluye el encabezado User-Agent, Bing intenta derivar al servidor de otras señales.

A continuación, muestra el análisis que está disponible para cada punto de conexión.

![Distribución mediante la matriz de compatibilidad de punto de conexión](./media/cognitive-services-bing-statistics/bing-statistics-matrix.PNG)
