---
title: Panel de la aplicación para aplicaciones de LUIS| Microsoft Docs
description: Obtenga información sobre el panel de la aplicación, una herramienta de informes visualizada que permite supervisar las aplicaciones de un solo vistazo.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: diberry
ms.openlocfilehash: cab07a994f2bdfb7ef8491adbb482c1902e4d4cb
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031992"
---
# <a name="application-dashboard"></a>Panel de la aplicación
El panel de la aplicación permite supervisar la aplicación de un solo vistazo. El **Panel** se muestra cuando se abre una aplicación al hacer clic en el nombre de la aplicación en la página **My Apps** (Mis aplicaciones) y después en **Dashboard** (Panel) en el panel superior. 

> [!CAUTION]
> Si quiere las métricas más actualizadas para LUIS, tendrá que:
> * Usar una [clave de punto de conexión](luis-how-to-azure-subscription.md) de LUIS creada en Azure.
> * Usar la clave de punto de conexión de LUIS para todas las solicitudes de punto de conexión incluida la [API](https://aka.ms/luis-endpoint-apis) de LUIS y el bot.
> * Usar otra clave de punto de conexión para cada aplicación de LUIS. No use la misma clave de punto de conexión para todas las aplicaciones. El seguimiento de la clave de punto de conexión se realiza en el nivel de clave, no en el nivel de aplicación.  

En la página **Dashboard** (Panel) se proporciona una visión general de la aplicación de LUIS incluido el modelo actual, así como el uso del [punto de conexión](luis-glossary.md#endpoint) en el tiempo. <!--The following image shows the **Dashboard** page.-->

<!-- TBD: Get a working screen shot
![The Dashboard](./media/luis-how-to-use-dashboard/dashboard.png)
-->

<!-- TBD: IS THIS STILL TRUE?
At the top of the **Dashboard** page, a contextual notification bar constantly displays notifications to update you on the required or recommended actions appropriate for the current state of your app. It also provides useful tips and alerts as needed. A detailed description of the data reported on the **Dashboard** page follows.
-->
  
## <a name="app-status"></a>Estado de la aplicación
En el panel se muestra el estado de entrenamiento y publicación de la aplicación, incluida la fecha y hora de la última vez que se entrenó y publicó.  

![Panel: Estado de la aplicación](./media/luis-how-to-use-dashboard/app-state.png)

## <a name="model-data-statistics"></a>Estadísticas de datos del modelo
En el panel se muestra el número total de intenciones, entidades y expresiones con etiqueta que existen en la aplicación. 

![Estadísticas de datos de la aplicación](./media/luis-how-to-use-dashboard/app-model-count.png)

## <a name="endpoint-hits"></a>Visitas del punto de conexión
En el panel se muestra el total de visitas del punto de conexión que recibe la aplicación de LUIS y permite mostrar las visitas dentro del período que se especifique. El número total de visitas que se muestra es la suma de las visitas del punto de conexión que usan una [clave de punto de conexión](./luis-concept-keys.md#endpoint-key) y las que usan una [clave de creación](./luis-concept-keys.md#authoring-key).

<!-- TBD: this image is old but I don't have a new one based on usage -->
![Visitas del punto de conexión](./media/luis-how-to-use-dashboard/dashboard-endpointhits.png)

> [!NOTE] 
> El número de visitas del punto de conexión más reciente se encuentra en Azure Portal en la información general del servicio LUIS. 
 
### <a name="total-endpoint-hits"></a>Total de visitas del punto de conexión
El número total de visitas del punto de conexión que recibe la aplicación desde su creación hasta la fecha actual.

### <a name="endpoint-hits-per-period"></a>Visitas del punto de conexión por período
El número de visitas recibidas en un período anterior, mostradas por día. Los puntos entre las fechas de inicio y finalización representan los días de este período. Mantenga el puntero del mouse sobre cada punto para ver el recuento de visitas de cada día del período. 

Para seleccionar un período para verlo en el gráfico:
 
1. Haga clic en el botón **Configuración adicional** ![Botón de configuración adicional](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) para acceder a la lista de períodos. Puede seleccionar períodos que oscilan entre una semana y un año. 

    ![Visitas del punto de conexión por período](./media/luis-how-to-use-dashboard/timerange.png)

2. Seleccione un período de la lista y, después, haga clic en la flecha Atrás. ![Flecha Atrás](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) para mostrar el gráfico.

### <a name="key-usage"></a>Uso de las claves
El número de visitas que se consumen de la clave de punto de conexión de la aplicación. Para más información sobre las claves de punto de conexión, consulte [Claves en LUIS](luis-concept-keys.md). 
  
## <a name="intent-breakdown"></a>Desglose de las intenciones
En **Intent Breakdown** (Desglose de las intenciones) se muestra un desglose de las intenciones en función de las expresiones etiquetadas o las visitas del punto de conexión. En este gráfico de resumen se muestra la importancia relativa de cada intención en la aplicación. Si mantiene el puntero del mouse sobre un segmento, verá el nombre de la intención y el porcentaje que representa del número total de expresiones etiquetadas y visitas del punto de conexión. 

![Desglose de la intención](./media/luis-how-to-use-dashboard/intent-breakdown.png)

Para controlar si el desglose se basa en las expresiones etiquetadas o las visitas del punto de conexión:

1. Haga clic en el botón **Configuración adicional** ![botón de configuración adicional](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) para acceder a la lista como en la imagen siguiente:

    ![Lista de desglose de la intención](./media/luis-how-to-use-dashboard/intent-breakdown-based-on.png)
2. Seleccione un valor de la lista y, después, haga clic en la flecha Atrás. ![Flecha Atrás](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) para mostrar el gráfico.

## <a name="entity-breakdown"></a>Desglose de entidades
En el panel se muestra un desglose de las entidades en función de las expresiones etiquetadas o las visitas del punto de conexión. En este gráfico de resumen se muestra la importancia relativa de cada entidad en la aplicación. Si mantiene el puntero del mouse sobre un segmento, verá el nombre de la entidad y el porcentaje que representa del número total de expresiones etiquetadas y visitas del punto de conexión. 

![Desglose de la entidad](./media/luis-how-to-use-dashboard/entity-breakdown.png)

Para controlar si el desglose se basa en las expresiones etiquetadas o las visitas del punto de conexión:

1. Haga clic en el botón **Configuración adicional** ![botón de configuración adicional](./media/luis-how-to-use-dashboard/Dashboard-Settings-btn.png) para acceder a la lista como en la imagen siguiente:

    ![Lista de desglose de la entidad](./media/luis-how-to-use-dashboard/entity-breakdown-based-on.png)
2. Seleccione un valor de la lista y, después, haga clic en la flecha Atrás. ![Flecha Atrás](./media/luis-how-to-use-dashboard/Dashboard-backArrow.png) para mostrar el gráfico según corresponda.
