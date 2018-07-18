---
title: ¿Qué es Anomaly Finder? - Microsoft Cognitive Services | Microsoft Docs
description: Use algoritmos avanzados en Anomaly Finder para ayudarle a identificar anomalías en los datos de serie temporal y a devolver información en Microsoft Cognitive Services.
services: cognitive-services
author: tonyxing
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 04/19/2018
ms.author: tonyxing
ms.openlocfilehash: 1080bb0ad1d901a8b9a5ace4993d4e0d46924a03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381319"
---
# <a name="what-is-anomaly-finder"></a>¿Qué es Anomaly Finder?

Anomaly Finder permite supervisar los datos con el tiempo y detectar anomalías con aprendizaje automático, que se adapta a sus datos únicos aplicando automáticamente el modelo estadístico correcto independientemente del sector, el escenario o el volumen de datos. Con una serie temporal como entrada, Anomaly Finder API devuelve si un punto de datos es una anomalía o no, determina el valor esperado y establece los límites superior e inferior de visualización. Como un servicio de AI creado previamente, Anomaly Finder no requiere ninguna experiencia de aprendizaje automático más allá de comprender cómo usar una API de RESTful. Esto hace que el desarrollo sea sencillo y versátil, ya que funciona con cualquier dado de serie temporal y también se puede integrar en sistemas de datos de streaming. Anomaly Finder abarca un intervalo amplio de casos de uso: por ejemplo, herramientas financieras para administrar fraudes, robos, mercados volubles y posibles incidentes de negocios o supervisar el tráfico de dispositivos IoT conservando el anonimato. Esta solución también se puede monetizar como parte de un servicio para que los clientes finales comprendan los cambios de datos, gastos, rentabilidad de la inversión o actividades del usuario.
Pruebe Anomaly Finder API y adquiera un conocimiento más amplio de los datos. 

Vea lo que puede compilar con esta API:

* Aprender a predecir los valores esperados en función de los datos históricos de las series temporales
* Indicar si un punto de datos es una anomalía fuera del patrón histórico
* Generar una banda para visualizar el rango del valor "normal"

![Anomaly_Finder](./media/anomaly_detection1.png) 

Figura 1: Detectar anomalías en los ingresos de ventas

![Anomaly_Finder](./media/anomaly_detection2.png)

Figura 2: Detectar cambios de patrón en las solicitudes de servicio

## <a name="requirements"></a>Requisitos

- Datos mínimos para series temporales de entrada: mínimo de trece puntos de datos para series temporales sin borrar la periodicidad y mínimo de cuatro ciclos de puntos de datos para las series temporales con una periodicidad conocida. 
- Integridad de datos: los puntos de datos de las series temporales están separados en el mismo rango y no falta ningún punto. 

## <a name="identify-anomalies"></a>Identificación de anomalías

Anomaly Detection API devuelve resultados para indicar si determinados puntos de datos son anomalías o no, y proporciona la siguiente información adicional.
* Período: la periodicidad con que se usa la API para detectar puntos de anomalías.
* WarningText: información de advertencia posible.
* ExpectedValue: valor de predicción del modelo basado en el aprendizaje.
* IsAnomaly: el resultado que indica si los puntos de datos son anomalías o no.
* IsAnomaly_Neg: el resultado que indica si los puntos de datos son anomalías en una dirección negativa (DIP).
* IsAnomaly_Pos: el resultado que indica si los puntos de datos son anomalías en una dirección positiva (picos).
* UpperMargin: la suma de ExpectedValue y UpperMargin determina el límite superior en que el punto de datos aún se considera normal.
* LowerMargin: (ExpectedValue - LowerMargin) determina el límite inferior en que el punto de datos aún se considera normal.

> [!Note]
> UpperMargin y LowerMargin pueden utilizarse para generar una banda de la serie temporal real para visualizar el rango de valores normales. 

## <a name="adjusting-lower-and-upper-bounds-in-post-processing-on-the-response"></a>Ajuste de los límites inferior y superior en el posprocesamiento de la respuesta

Anomaly Detection API devuelve un resultado predeterminado que indica si un punto de datos es una anomalía o no, y los límites inferior y superior se pueden calcular a partir de ExpectedValue y UpperMargin/LowerMargin. Los valores predeterminados deben funcionar correctamente para la mayoría de los casos. Sin embargo, algunos escenarios requieren distintos límites a los predeterminados. La práctica recomendada consiste en aplicar un coeficiente en UpperMargin o LowerMargin para ajustar los límites dinámicos.

### <a name="examples-with-1152-as-coefficiency"></a>Ejemplos con 1/1,5/2 como coeficiente

![Sensibilidad predeterminada](./media/sensitivity_1.png)

![1,5 Sensibilidad](./media/sensitivity_1.5.png)

![2 Sensibilidad](./media/sensitivity_2.png)

Solicitud con datos de ejemplo

[!INCLUDE [Request](./includes/request.md)]

Respuesta JSON de ejemplo

[!INCLUDE [Response](./includes/response.md)]
