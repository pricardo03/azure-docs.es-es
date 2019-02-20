---
title: Pestaña Optimizar de Azure Data Factory Mapping Data Flow
description: Optimización de los flujos de datos de asignación de Azure Data Factory con la configuración de particiones de la pestaña Optimizar
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: a83e95793563faab31e6b7183d657126c13ebb54
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56212831"
---
# <a name="azure-data-factory-data-flow-transformation-optimize-tab"></a>Pestaña Optimizar de transformación de Azure Data Factory Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Cada transformación de Data Flow tiene una pestaña "Optimizar". La pestaña de optimización contiene valores opcionales para configurar esquemas de partición para flujos de datos.

<img src="media/data-flow/opt001.png" width="800">

El valor predeterminado es "usar la creación de particiones actual". La creación de particiones indica a Azure Data Factory que use el esquema de partición nativo en los flujos de datos que se ejecutan en Spark en Azure Databricks. Por lo general, este es el método recomendado.

Sin embargo, hay instancias en las que puede que desee ajustar la creación de particiones. Por ejemplo, si desea que el resultado de las transformaciones llegue a un único archivo del lago, elija "partición única" en la pestaña Optimizar para crear particiones en la transformación de receptor.

Otro caso en el que puede desear ejercer control sobre los esquemas de creación de particiones que se usan para las transformaciones de datos es cuando se busca mejorar el rendimiento. El ajuste de la creación de particiones de datos proporciona un nivel de control sobre la distribución de los datos en los nodos de proceso y las optimizaciones de la localidad de los datos que puedan tener efectos tanto positivos como negativos en el rendimiento general de los flujos de datos.

Si desea cambiar la creación de particiones en cualquier transformación, simplemente haga clic en la pestaña Optimizar y seleccione el botón de radio "Set Partitioning" (Establecer creación de particiones). Luego se le presentará una serie de opciones para la creación de particiones. El mejor método de creación de particiones que se implemente variará en función de los volúmenes de datos, claves de candidatos, valores nulos y cardinalidad. El procedimiento recomendado es empezar con la creación de particiones predeterminada y, después, probar las diferentes opciones de creación de particiones. Puede probarlo mediante la ejecución de la depuración en la canalización y, después, ver el tiempo empleado en cada agrupación de transformaciones, así como el uso de las particiones en la vista de supervisión.

<img src="media/data-flow/opt002.png" width="600">

### <a name="round-robin"></a>Round Robin

Round Robin es una partición simple que distribuye automáticamente los datos equitativamente entre las distintas particiones. Use Round Robin cuando no tenga buenos candidatos principales para implementar una estrategia de creación de particiones sólida e inteligente. Puede establecer el número de particiones físicas.

### <a name="hash"></a>Hash

Azure Data Factory generará un valor hash de las columnas para generar particiones uniformes, con el fin de que las filas con valores similares estarán en la misma partición. Si usa la opción Hash, pruebe si hay una posible distorsión de las particiones. Puede establecer el número de particiones físicas.

### <a name="dynamic-range"></a>Intervalo dinámico

Intervalo dinámico usará los intervalos dinámicos de Spark en función de las columnas o expresiones que proporcione. Puede establecer el número de particiones físicas. 

### <a name="fixed-range"></a>Intervalo fijo

Debe crear una expresión que proporcione un intervalo fijo para los valores de las columnas de datos con particiones. Para evitar la distorsión de la partición es preciso que conozca perfectamente los datos antes de usar esta opción. El valor que especifique para la expresión se usará como parte de una función de partición. Puede establecer el número de particiones físicas.

### <a name="key"></a>Clave

Si conoce bien la cardinalidad de los datos, la creación de particiones clave puede ser una buena estrategia de partición. La creación de particiones clave creará particiones para cada valor único de la columna. No puede establecer el número de particiones porque dicho número se basará en valores únicos de los datos.
