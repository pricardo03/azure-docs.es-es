---
title: Ejecutar la actividad de flujo de datos en Azure Data Factory | Microsoft Docs
description: Cómo ejecutar los datos fluye desde dentro de una canalización de factoría de datos.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: makromer
ms.openlocfilehash: b0a6c6feae11f8daeed54c5e763dbff3aa711652
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66153484"
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

### <a name="run-on"></a>Ejecutar el

Elija el entorno de proceso para esta ejecución del flujo de datos. El valor predeterminado es la resolución automática de Azure predeterminada Integration Runtime. Esta opción ejecutará el flujo de datos en el entorno de Spark en la misma región que la factoría de datos. El tipo de cálculo será un clúster de trabajo, lo que significa que el entorno de proceso tardará varios minutos en Inicio.

### <a name="debugging-pipelines-with-data-flows"></a>Depuración de canalizaciones con flujos de datos

![Depurar botón](media/data-flow/debugbutton.png "botón Depurar")

Utilizar la depuración de flujo de datos para utilizar un clúster calienten para probar los flujos de datos interactivamente en una ejecución de depuración de canalización. Use la opción de la canalización de depuración para probar los flujos de datos dentro de una canalización.

### <a name="run-on"></a>Ejecutar el

Este es un campo obligatorio que define qué tiempo de ejecución de integración que se usará para la ejecución de la actividad de flujo de datos. De forma predeterminada, la factoría de datos usará el tiempo de ejecución de integración de Azure de resolución automática de forma predeterminada. Sin embargo, puede crear sus propios tiempos de ejecución de integración de Azure que definir regiones específicas, TTL, recuentos básicos y tipo de proceso para la ejecución de actividad de flujo de datos.

La configuración predeterminada para las ejecuciones de flujo de datos es de 8 núcleos de proceso general con un TTL de 60 minutos.

Tiene control sobre el entorno de ejecución de Spark para sus actividades de flujo de datos. En el [en tiempo de ejecución de integración de Azure](concepts-integration-runtime.md) es la configuración para establecer el tipo de proceso (uso general, con optimización para memoria y proceso optimizado), número de núcleos de trabajo y tiempo de vida para que coincida con el motor de ejecución con el proceso de flujo de datos requisitos. Además, establecer el TTL, podrá mantener un clúster activo que está inmediatamente disponible para las ejecuciones de trabajos.

![En tiempo de ejecución de integración de Azure](media/data-flow/ir-new.png "en tiempo de ejecución de integración de Azure")

### <a name="staging-area"></a>Área de ensayo

Si se hunde los datos en Azure Data Warehouse, debe elegir una ubicación de ensayo para la carga por lotes de Polybase.

## <a name="parameterized-datasets"></a>Conjuntos de datos con parámetros

Si usa conjuntos de datos con parámetros, asegúrese de establecer los valores de parámetro.

![Ejecutar los parámetros de flujo de datos](media/data-flow/params.png "parámetros")

### <a name="debugging-parameterized-data-flows"></a>Depuración de flujos de datos con parámetros

Solo se pueden depurar flujos de datos con conjuntos de datos con parámetros de la canalización de depuración ejecutar mediante la actividad de flujo de datos de ejecución. Actualmente, las sesiones de depuración interactiva en el flujo de datos de ADF no funcionan con conjuntos de datos con parámetros. Depuración de ejecuciones de canalización funcionará con parámetros.

Una buena práctica consiste en compilar el flujo de datos con un conjunto de datos estático para que tengan la propagación de columna de metadatos completos disponible en tiempo de diseño. A continuación, reemplace el conjunto de datos estático con un conjunto de datos con parámetros dinámico al poner en operación su canalización de flujo de datos.

## <a name="next-steps"></a>Pasos siguientes
Consulte otras actividades de flujo de control compatibles con Data Factory: 

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
