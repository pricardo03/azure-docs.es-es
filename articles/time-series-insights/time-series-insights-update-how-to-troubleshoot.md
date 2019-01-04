---
title: 'Versión preliminar de Azure Time Series Insights: Diagnóstico y solución de problemas | Microsoft Docs'
description: Comprenda cómo diagnosticar y solucionar problemas con la versión preliminar de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: e35439c80dea8ac47033464eeb57f7e9859fdcf5
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53275309"
---
# <a name="diagnose-and-troubleshoot"></a>Diagnóstico y solución de problemas

Este artículo resume varios problemas comunes que pueden surgir al trabajar con el entorno de la versión preliminar de Azure Time Series Insights. El artículo también describe posibles causas y soluciones para cada problema.

## <a name="problem-i-cant-find-my-environment-in-the-time-series-insights-preview-explorer"></a>Problema: No encuentro mi entorno en el explorador de la versión preliminar de Time Series Insights

Este problema puede producirse si no tiene los permisos necesarios para acceder al entorno de Time Series Insights. El usuario necesita un rol de acceso de nivel lector para ver su entorno de Time Series Insights. Para comprobar los niveles de acceso actuales y conceder acceso adicional, visite la sección de directivas de acceso de datos en el recurso de Time Series Insights en [Azure Portal](https://portal.azure.com/).

  ![Entorno][1]

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Problema: No se ve ningún dato en el explorador de la versión preliminar de Time Series Insights

Hay varias razones que pueden impedirle ver sus datos en el [explorador de la versión preliminar de Azure Time Series Insights](https://insights.timeseries.azure.com/preview).

- Puede que el origen del evento no esté recibiendo datos.

    Compruebe que el origen del evento, que es un centro de eventos o un centro de IoT, está recibiendo datos de las etiquetas o las instancias. Para comprobarlo, vaya a la página de información general de Azure Portal.

    ![Panel con las conclusiones][2]

- Los datos de origen del evento no tienen formato JSON.

    Time Series Insights solo admite datos JSON. Para ver ejemplos de JSON, consulte [Formas de JSON admitidas](./how-to-shape-query-json.md).

- A la clave de origen del evento le falta un permiso necesario.

    * Para IoT Hub, debe proporcionar la clave con el permiso de **conexión de servicio**.

    ![Configuración][3]

    * Tal como se muestra en la imagen anterior, servirían las directivas **iothubowner** o **service**, dado que ambas tienen permiso de **conexión del servicio**.
    * Para una instancia de Event Hubs, debe proporcionar la clave con el permiso de **escucha**.
  
    ![Permisos][4]

    * Como se muestra en la imagen anterior, servirían las directivas **read** o **manage**, dado que ambas tienen permiso de **escucha**.

- El grupo de consumidores proporcionado no es exclusivo de Time Series Insights.

    Durante el registro de un centro de IoT o un centro de eventos, se especifica el grupo de consumidores que se usa para leer los datos. No comparta ese grupo de consumidores. Si el grupo de consumidor es compartido, el centro de eventos subyacente desconectará automáticamente a uno de los lectores de forma aleatoria. Proporcione un grupo de consumidor exclusivo del que Time Series Insights pueda leer.

- La propiedad Time Series ID especificada en el momento del aprovisionamiento es incorrecta, falta o es nula.

    Este problema puede producirse si la propiedad Time Series ID se configura incorrectamente en el momento de aprovisionamiento del entorno. Para más información, consulte [Best practices for choosing a Time Series ID](./time-series-insights-update-how-to-id.md) (Procedimientos recomendados para elegir un identificador de Time Series). En la actualidad, no se puede actualizar un entorno de Time Series Insights existente para usar un identificador de Time Series distinto.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Problema: Se muestran algunos datos, pero faltan otros

Podría estar enviando datos sin el identificador de Time Series.

- Este problema puede producirse cuando se envían los eventos sin el campo de identificador de Time Series en la carga útil. Para más información, consulte el artículo sobre las [formas compatibles de JSON](./how-to-shape-query-json.md).

- Este problema puede producirse porque se está limitando su entorno.

    > [!NOTE]
    > En este momento, Time Series Insights admite una tasa máxima de ingesta de 6 Mbps.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Problema: Mi configuración de nombre de propiedad Timestamp del origen de eventos no funciona

Asegúrese de que el nombre y el valor se ajustan a las reglas siguientes:

* El nombre de la propiedad Timestamp distingue mayúsculas de minúsculas.
* El valor de propiedad Timestamp que procede del origen del evento, como una cadena JSON, tiene el formato `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Un ejemplo de este tipo de cadena es `“2008-04-12T12:53Z”`.

La manera más fácil de asegurarse de que el nombre de la propiedad Timestamp se captura y funciona correctamente consiste en utilizar el explorador de la versión preliminar de Time Series Insights. En el explorador de la versión preliminar de Time Series Insights, utilice el gráfico para seleccionar un período de tiempo después de que proporcionara el nombre de la propiedad Timestamp. Haga clic con el botón derecho en la selección y elija la opción de **exploración de eventos**. El primer encabezado de columna es el nombre de propiedad Timestamp. Debe tener `($ts)` junto a la palabra `Timestamp`, en lugar de:

* `(abc)`, que indica que Time Series Insights lee los valores de datos como cadenas.
* El icono de calendario, que indica que Time Series Insights lee los valores de datos como datetime.
* `#`, que indica que Time Series Insights lee los valores de datos como un entero.

Si no se especifica explícitamente la propiedad Timestamp, se utiliza como marca de tiempo predeterminado la hora de puesta en cola de un centro de IoT o centro de eventos de un evento.

## <a name="problem-i-cant-edit-or-view-my-time-series-model"></a>Problema: No puedo editar o ver mi modelo de Times Series

- Puede que esté accediendo a un entorno Time Series Insights S1 o S2.

   Los modelos de Time Series solo se admiten en entornos PAYG (pago por uso). Para más información sobre cómo acceder a su entorno S1/S2 desde el explorador de versión preliminar de Time Series Insights, consulte [Visualización de datos en el explorador](./time-series-insights-update-explorer.md).

   ![Access][5]

- Puede que no tenga permisos para ver y editar el modelo.

   Los usuarios necesitan acceso de nivel de colaborador para editar y ver su modelo de Time Series. Para comprobar los niveles de acceso actuales y conceder acceso adicional, visite la sección de directivas de acceso de datos en el recurso de Time Series Insights en Azure Portal.

## <a name="problem-all-my-instances-in-the-time-series-insights-preview-explorer-dont-have-a-parent"></a>Problema: Todas mis instancias en el explorador de versión preliminar de Time Series Insights no tienen un elemento primario

Este problema puede producirse si el entorno no tiene una jerarquía de modelo de Time Series definida. Para más información, consulte el [trabajo con modelos de Time Series](./time-series-insights-update-how-to-tsm.md).

  ![Modelos de Time Series][6]

## <a name="next-steps"></a>Pasos siguientes

- Lea el artículo sobre [trabajo con modelos de Time Series](./time-series-insights-update-how-to-tsm.md).
- Lea el artículo sobre [formas compatibles de JSON](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/environment.png
[2]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[3]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[4]: media/v2-update-diagnose-and-troubleshoot/permissions.png
[5]: media/v2-update-diagnose-and-troubleshoot/access.png
[6]: media/v2-update-diagnose-and-troubleshoot/tsm.png