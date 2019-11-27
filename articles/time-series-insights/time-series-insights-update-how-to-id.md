---
title: 'Procedimientos recomendados para elegir un identificador de serie temporal: Azure Time Series Insights | Microsoft Docs'
description: Conozca los procedimientos recomendados para elegir un identificador de serie temporal en la versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/22/2019
ms.custom: seodec18
ms.openlocfilehash: cf826c47c61e3714a05dd81fe3eea4e6ee0b03f4
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74012493"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Procedimientos recomendados al elegir un id. de serie temporal

En este artículo se resume la importancia del identificador de serie temporal en el entorno de la versión preliminar de Azure Time Series Insights y los procedimiento recomendados para elegir uno.

## <a name="choose-a-time-series-id"></a>Elección de un identificador de Time Series

Elegir un id. de serie temporal es como elegir una clave de partición para una base de datos. Se debe seleccionar cuando se crea un entorno de versión preliminar de Time Series Insights. Es una propiedad *inmutable*. En otras palabras, después de crear un entorno de versión preliminar de Time Series Insights con un identificador de serie temporal, no se puede cambiar en dicho entorno. 

> [!IMPORTANT]
> El identificador de serie temporal distingue mayúsculas de minúsculas.

Es fundamental seleccionar un identificador de serie temporal adecuado. Estos son algunos de los procedimientos recomendado que puede seguir:

* Elija una clave de partición con muchos valores distintos (por ejemplo, centenares o miles). En muchos casos, puede ser el identificador de dispositivo, identificador de sensor o identificador de etiqueta de JSON.
* El id. de serie temporal debe ser único en el nivel de nodo hoja de su [Modelo de serie temporal](./time-series-insights-update-tsm.md).
* Si el origen del evento es un centro de IoT, lo más probable es que el identificador de serie temporal sea *iothub-connection-device-id*.
* El límite de caracteres de la cadena de nombre de la propiedad del identificador de serie temporal es 128. En el caso del valor de la propiedad del identificador de serie temporal, el límite de caracteres es 1.024.
* Si falta un valor de propiedad único para el identificador de serie temporal, se trata como un valor NULL y sigue la misma regla de la restricción de unicidad.
* Puede seleccionar hasta *tres* propiedades clave como identificador de serie temporal. Su combinación será una clave compuesta que representa el identificador de serie temporal.  

  > [!NOTE]
  > Las tres propiedades clave deben ser cadenas.
  > Tendría que realizar consultas en esta clave compuesta, en lugar de hacerlo propiedad a propiedad.

En los siguientes escenarios se describe cómo seleccionar más de una propiedad clave como identificador de serie temporal.  

### <a name="example-1-time-series-id-with-a-unique-key"></a>Ejemplo 1: identificador de serie temporal con una clave única

* Tiene flotas de recursos heredadas. Cada una tiene una clave única.
* Cada flota se identifica de forma única mediante la propiedad **deviceId**. En el caso de otra flota, la propiedad única es **objectId**. Ninguna de las flotas contiene la propiedad única de la otra. En este ejemplo, debe seleccionar dos claves, **deviceId** y **objectId**, como claves únicas.
* Aceptamos valores NULL y la falta de la presencia de una propiedad en la carga del evento cuenta como un valor NULL. Esta es también la forma adecuada de controlar el envío de datos a dos orígenes de eventos diferentes, donde los datos de cada origen de eventos tienen un identificador de serie temporal único.

### <a name="example-2-time-series-id-with-a-composite-key"></a>Ejemplo 2: identificador de serie temporal con una clave compuesta

* Necesita que varias propiedades sean únicas dentro del mismo tipo de activos. 
* Es fabricante de edificios inteligentes e implementa sensores en todas las habitaciones. En cada una de ellas, lo habitual es que tenga los mismos valores para **sensorId**. Algunos ejemplos son **sensor1**, **sensor2**y **sensor3**.
* Los números de las plantas y de las estancias del edificio se solapan en la propiedad **flrRm**. Estos números tienen valores como **1a**, **2b** y **3a**.
* Tiene una propiedad, **location**, que contiene valores como **Redmond**, **Barcelona** y **Tokio**. Para que los valores sean únicos, designe las tres propiedades siguientes como sus claves del identificador de serie temporal: **sensorId**, **flrRm** y **location**.

Ejemplo de evento sin procesar:

```JSON
{
  "sensorId": "sensor1",
  "flrRm": "1a",
  "location": "Redmond",
  "temperature": 78
}
```

En Azure Portal, esta clave compuesta se puede especificar así: 

`[{"name":"sensorId","type":"String"},{"name":"flrRm","type":"String"},{"name":"location","type":"string"}]`

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el [modelado de datos](./time-series-insights-update-tsm.md).

* Planee el [entorno de la versión preliminar de Azure Time Series Insights](./time-series-insights-update-plan.md).