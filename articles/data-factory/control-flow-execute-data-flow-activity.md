---
title: Ejecutar actividades del flujo de datos en Azure Data Factory | Microsoft Docs
description: Cómo ejecutar flujos de datos desde una canalización de factoría de datos.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: 24b27c16573a35b1d8749d7ff381fbef970f4bd0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67471655"
---
# <a name="execute-data-flow-activity-in-azure-data-factory"></a>Ejecutar la actividad de flujo de datos en Azure Data Factory
Use la actividad para ejecutar flujos de datos y ejecute el flujo de datos de ADF en las ejecuciones de depuración de canalizaciones (sandbox) y en las ejecuciones de canalizaciones desencadenadas.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="syntax"></a>Sintaxis

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "dataflow1",
         "type": "DataFlowReference"
      },
        "compute": {
          "computeType": "General",
          "coreCount": 8,
      }
}

```

## <a name="type-properties"></a>Propiedades de tipo

* ```dataflow``` es el nombre de la entidad de flujo de datos que quiere ejecutar.
* ```compute``` describe el entorno de ejecución de Spark.
* ```coreCount``` es el número de núcleos que se asignarán a esta actividad de ejecución del flujo de datos.

![Ejecutar Data Flow](media/data-flow/activity-data-flow.png "Ejecutar Data Flow")

### <a name="debugging-pipelines-with-data-flows"></a>Depuración de canalizaciones con flujos de datos.

![Botón de depuración](media/data-flow/debugbutton.png "Botón de depuración")

Use la depuración de Data Flow para usar un clúster activado previamente y así poder probar sus flujos de datos de forma interactiva en una ejecución de depuración de canalizaciones. Use la opción para depurar canalizaciones para probar los flujos de datos de una canalización.

### <a name="run-on"></a>Ejecutar en

Este es un campo obligatorio que define qué instancia de Integration Runtime se debe usar para la ejecución de actividades de Data Flow. De forma predeterminada, Data Factory usará el entorno de ejecución de integración de resolución automática de Azure. Sin embargo, puede crear sus propios entornos de ejecución de integración de Azure que definan regiones específicas, el tipo de proceso, recuentos de núcleos y elementos TTL para la ejecución de actividades del flujo de datos.

La configuración predeterminada de las ejecuciones de Data Flow es de 8 núcleos de proceso general con un TTL de 60 minutos.

Elija el entorno de proceso de esta ejecución para su flujo de datos. El valor predeterminado es el entorno de ejecución de la instancia de Integration Runtime automática de Azure. Esta opción ejecutará el flujo de datos en el entorno de Spark que está en la misma región que su factoría de datos. El tipo de proceso será un clúster de tareas, lo que significa que el entorno de proceso tardará varios minutos en iniciarse.

Tendrá control sobre el entorno de ejecución de Spark para sus actividades de Data Flow. En el [entorno de ejecución de integración de Azure](concepts-integration-runtime.md) están las opciones para configurar el tipo de proceso (propósito general, optimizada para memoria y optimizado para proceso), el 0número de núcleos de trabajo y el tiempo de vida para hacer coincidir el motor de ejecución con los requisitos de proceso de Data Flow. Además, la configuración de TTL le permitirá mantener un clúster activado previamente que esté disponible de inmediato para la ejecución de trabajos.

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!NOTE]
> La selección del entorno de ejecución de integración en la actividad de Data Flow solo se aplica a las *ejecuciones activadas* de su canalización. La depuración de la canalización con instancias de Data Flow que cuenten con opciones de depuración se ejecutará en el clúster de Spark predeterminado de 8 núcleos.

### <a name="staging-area"></a>Área de almacenamiento provisional

Si está recibiendo sus datos en Azure Data Warehouse, debe elegir una ubicación de almacenamiento provisional para la carga por lotes de Polybase. La configuración del almacenamiento provisional solo se aplica a las cargas de trabajo de Azure Data Warehouse.

## <a name="parameterized-datasets"></a>Conjuntos de datos con parámetros

Si usa conjuntos de datos con parámetros, asegúrese de establecer los valores del parámetro.

![Ejecutar parámetros de Data Flow](media/data-flow/params.png " Parámetros")

## <a name="parameterized-data-flows"></a>Flujos de datos con parámetros

Si tiene parámetros en el flujo de datos, deberá establecer los valores dinámicos de los parámetros del flujo de datos aquí, en la sección Parámetros de la actividad para ejecutar Data Flow. Puede usar el lenguaje de expresiones de canalización de ADF (solo para los tipos de parámetro de cadena) o el lenguaje de expresión de Data Flow para establecer los valores de los parámetros con expresiones dinámicas o valores estáticos literales.

![Ejecutar el ejemplo de parámetro de Data Flow](media/data-flow/parameter-example.png "Ejemplo de parámetro")

### <a name="debugging-data-flows-with-parameters"></a>Depuración de flujos de datos con parámetros.

En este momento solo puede depurar flujos de datos con parámetros mediante la ejecución para depurar canalizaciones, si usa la actividad de flujo de datos de ejecución. Las sesiones de depuración interactivas en Data Flow de ADF estarán disponibles próximamente. Sin embargo, las ejecuciones de canalizaciones y las de depuración funcionarán con parámetros.

Una opción útil es aumentar el flujo de datos con contenido estático para obtener una propagación de columna de metadatos completa en el tiempo de diseño de la resolución de problemas. A continuación, reemplace el conjunto de datos estáticos con un conjunto de datos dinámico con parámetros cuando ponga en funcionamiento su flujo de datos.

## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory: 

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
