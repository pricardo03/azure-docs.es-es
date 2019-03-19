---
title: Ejecutar la actividad de flujo de datos en Azure Data Factory | Microsoft Docs
description: La actividad de flujo de datos execute ejecuta flujos de datos.
services: data-factory
documentationcenter: ''
author: kromerm
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: 856f4bd9c2b04ff10ed598c5e641955e1de99398
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792419"
---
# <a name="execute-data-flow-activity-in-azure-data-factory"></a>Ejecutar la actividad de flujo de datos en Azure Data Factory
Utilice la actividad de flujo de datos de execute para ejecutar el flujo de datos ADF en ejecuciones de canalización depuración (sandbox) y en las ejecuciones de canalización desencadenada.

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

* ```dataflow``` es el nombre de la entidad de flujo de datos que desea ejecutar
* ```compute``` Describe el entorno de ejecución de Spark
* ```coreCount``` es el número de núcleos para asignar a esta ejecución de la actividad del flujo de datos

![Ejecutar el flujo de datos](media/data-flow/activity-data-flow.png "ejecutar el flujo de datos")

### <a name="run-on"></a>Ejecutar en

Elija el entorno de proceso para esta ejecución del flujo de datos. El valor predeterminado es la resolución automática de Azure predeterminada Integration Runtime. Esta opción ejecutará el flujo de datos en el entorno de Spark en la misma región que la factoría de datos. El tipo de cálculo será un clúster de trabajo, lo que significa que el entorno de proceso tardará varios minutos en Inicio.

### <a name="debugging-pipelines-with-data-flows"></a>Depuración de canalizaciones con flujos de datos

![Depurar botón](media/data-flow/debugbutton.png "botón Depurar")

Utilizar la depuración de flujo de datos para utilizar un clúster calienten para probar los flujos de datos interactivamente en una ejecución de depuración de canalización. Use la opción de depuración de canalización para probar los flujos de datos dentro de una canalización.

### <a name="compute-type"></a>Tipo de proceso

Puede elegir el uso General, de proceso optimizado o con optimización para memoria, dependiendo de los requisitos del flujo de datos.

### <a name="core-count"></a>Núm. de núcleos

Elija el número de núcleos que desea asignar al trabajo. Para los trabajos más pequeños, menos núcleos funcionan mejor.

### <a name="staging-area"></a>Área de ensayo

Si se hunde los datos en Azure Data Warehouse, debe elegir una ubicación de ensayo para la carga por lotes de Polybase.

## <a name="parameterized-datasets"></a>Conjuntos de datos con parámetros

Si usa conjuntos de datos con parámetros, asegúrese de establecer los valores de parámetro.

![Ejecutar los parámetros de flujo de datos](media/data-flow/params.png "parámetros")

### <a name="debugging-parameterized-data-flows"></a>Depuración de flujos de datos con parámetros

Solo se pueden depurar flujos de datos con conjuntos de datos con parámetros de la canalización de depuración ejecutar mediante la actividad de flujo de datos de ejecución. Actualmente, las sesiones de depuración interactiva en el flujo de datos de ADF no funcionan con conjuntos de datos con parámetros. Depuración de ejecuciones de canalización funcionará con parámetros.

## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory: 

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
