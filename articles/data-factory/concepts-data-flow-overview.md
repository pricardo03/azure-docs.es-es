---
title: Información general de la asignación de Data Flow en Azure Data Factory
description: Explicación general de la asignación de flujos de datos en Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 6f4c124c59584c8538d85ac61650661ae559a77b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70123918"
---
# <a name="what-are-mapping-data-flows"></a>¿Qué es la asignación de instancias de Data Flow?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La asignación de instancias de Data Flow es una transformación de datos diseñada visualmente en Azure Data Factory. Las instancias de Data Flow permiten a los ingenieros de datos desarrollar una lógica de transformación de datos gráfica sin tener que escribir código. Los flujos de datos resultantes se ejecutan como actividades en las canalizaciones de Azure Data Factory mediante clústeres de Azure Databricks de escalabilidad horizontal.

La intención de Data Flow de Azure Data Factory es proporcionar una experiencia totalmente visual sin necesidad de codificación. Sus flujos de datos se ejecutarán en su propio clúster de ejecución para realizar el procesamiento de datos de escalabilidad horizontal. Asimismo, Azure Data Factory controla toda la traducción de código, la optimización de rutas de acceso y la ejecución de trabajos de flujo de datos.

Puede comenzar creando flujos de datos en el modo de depuración, para así poder validar la lógica de transformación de forma interactiva. A continuación, agregue una actividad de Data Flow a la canalización para ejecutar y probar el flujo de datos en la depuración de la canalización, o use "Desencadenar ahora" en la canalización para probar el flujo de datos de una actividad de canalización.

A continuación, debe programar y supervisar sus actividades de flujo de datos mediante las canalizaciones de Azure Data Factory que ejecutan la actividad de Data Flow.

La alternancia del modo de depuración en la superficie de diseño de Data Flow permite la compilación interactiva de las transformaciones de datos. El modo de depuración proporciona un entorno de preparación de datos y de vista previa de los datos para la construcción del flujo de datos.

## <a name="begin-building-your-data-flow-logical-graph"></a>Creación del grafo lógico de flujo de datos

Comience a crear flujos de datos con el signo + en los recursos de Factory para crear un nuevo flujo de datos.

![nuevo flujo de datos](media/data-flow/newdataflow2.png "new data flow")

Empiece por configurar la transformación de origen y, a continuación, agregue la transformación de datos a cada paso posterior mediante el signo +. Al crear el grafo lógico, puede cambiar entre los modos de gráfico y configuración mediante el botón para mostrar el grafo y para ocultarlo.

![Mostrar grafo](media/data-flow/showg.png "Show graph")

## <a name="configure-transformation-logic"></a>Configuración de la lógica de transformación

![Ocultar grafo]Hide Graph(media/data-flow/hideg.png "")

Si oculta el grafo, podrá navegar por los nodos de transformación lateralmente.

![Navegar](media/data-flow/showhide.png "navigate")

## <a name="next-steps"></a>Pasos siguientes

* [Comienzo con una transformación de origen](data-flow-source.md)
