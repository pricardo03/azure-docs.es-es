---
title: 'Inicio rápido: explorador de Azure Time Series Insights | Microsoft Docs'
description: Esta guía de inicio rápido muestra cómo comenzar a trabajar con el explorador de Azure Time Series Insights en el explorador web para visualizar grandes volúmenes de datos de IoT. Recorra las características clave en un entorno de demostración.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: be663520c9c1afd11ace57cd9dcb8ffb8219b86b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696953"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Guía de inicio rápido: Exploración de Azure Time Series Insights

Se proporciona un explorador de inicio rápido para comenzar a usar Azure Time Series Insights en un entorno de demostración gratuito. Con él, aprenderá a usar el explorador web para visualizar grandes volúmenes de datos de IoT y recorrerá las características clave existentes en la versión de disponibilidad general.

Azure Time Series Insights es un servicio de análisis, almacenamiento y administración totalmente administrado que facilita el análisis y la exploración de miles de millones de eventos de IoT de forma simultánea. Ofrece una visión global de los datos que permite validar rápidamente la solución de IoT y evitar el costoso tiempo de inactividad de los dispositivos críticos. Con Azure Time Series Insights, puede detectar las tendencias ocultas, descubrir anomalías y realizar análisis de causa principal casi en tiempo real.

Para mayor flexibilidad, Azure Time Series Insights puede agregarse a una aplicación existente previamente mediante sus eficaces [API de REST](./time-series-insights-update-tsq.md) y [SDK de cliente](./tutorial-create-tsi-sample-spa.md). Las API permiten almacenar y consultar los datos de serie temporal y consumirlos en una aplicación cliente de su elección. También tiene la opción de usar el SDK de cliente para agregar los componentes de la interfaz de usuario a la aplicación existente.

El explorador de Time Series Insights es una visita guiada de las características existentes en la versión de disponibilidad general.

## <a name="prepare-the-demo-environment"></a>Preparación del entorno de demostración

1. Cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) si no tiene ninguna aún.

1. En el explorador, navegue hasta la [demostración de la disponibilidad general](https://insights.timeseries.azure.com/demo).

1. Si se le solicita, inicie sesión en el explorador de Time Series Insights con las credenciales de la cuenta de Azure.

1. Aparece una página con un recorrido rápido de Time Series Insights. Haga clic en **Siguiente** para comenzar el recorrido rápido.

   [![Haga clic en Siguiente](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox).

## <a name="explore-the-demo-environment"></a>Exploración del entorno de demostración

1. Aparece el **panel de selección de hora**. Use este panel para seleccionar un período de tiempo que se va a visualizar.

   [![Panel de selección de hora](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Haga clic y arrastre en la región y, luego, haga clic en el botón **Buscar**.

   [![Selección de un período de tiempo](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   Time Series Insights muestra una visualización de gráfico para el período de tiempo que especificó. Con el gráfico de líneas, puede hacer varias acciones, como filtrar, anclar, ordenar y apilar.

   Para volver al **panel de selección de hora**, haga clic en la flecha hacia abajo como se muestra a continuación:

   [![Gráfico](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Haga clic en **Agregar** en el **panel de términos** para agregar un término de búsqueda nuevo.

   [![Agregar elemento](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. En el gráfico, puede seleccionar una región, hacer clic con el botón derecho en la región y seleccionar **Explorar eventos**.

   [![Explorar eventos](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   Se muestra una cuadrícula de los datos sin procesar de la región que está explorando:

   [![Vista de cuadrícula](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Seleccionar y filtrar datos

1. Edite los términos para cambiar los valores del gráfico y agregue otro término para correlacionar distintos tipos de valores:

   [![Agregar un término](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. Escriba un término de filtro en el cuadro **Filtrar series...** para el filtrado de series improvisado. En esta guía de inicio rápido, escriba **Station5** para correlacionar la temperatura y la presión de esa estación.

   [![Filtrar series](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Cuando termine la guía de inicio rápido, podrá experimentar con el conjunto de datos de ejemplo para crear visualizaciones distintas.

## <a name="next-steps"></a>Pasos siguientes

Está listo para crear su propio entorno de Time Series Insights:
> [!div class="nextstepaction"]
> [Planeamiento del entorno de Time Series Insights](time-series-insights-environment-planning.md)
