---
title: Actividad de Data Flow en Azure Data Factory
description: Cómo ejecutar flujos de datos desde una canalización de factoría de datos.
services: data-factory
documentationcenter: ''
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.author: makromer
ms.date: 10/07/2019
ms.openlocfilehash: 5623907346ee3882ad53a27695336ba4bc449db8
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73679941"
---
# <a name="data-flow-activity-in-azure-data-factory"></a>Actividad de Data Flow en Azure Data Factory

Utilice la actividad de Data Flow para transformar y trasladar datos a través de flujos de datos de asignación. Si no está familiarizado con los flujos de datos, consulte esta [introducción a los flujos de datos de asignación](concepts-data-flow-overview.md).

## <a name="syntax"></a>Sintaxis

```json
{
    "name": "MyDataFlowActivity",
    "type": "ExecuteDataFlow",
    "typeProperties": {
      "dataflow": {
         "referenceName": "MyDataFlow",
         "type": "DataFlowReference"
      },
      "staging": {
          "linkedService": {
              "referenceName": "MyStagingLinkedService",
              "type": "LinkedServiceReference"
          },
          "folderPath": "my-container/my-folder"
      },
      "integrationRuntime": {
          "referenceName": "MyDataFlowIntegrationRuntime",
          "type": "IntegrationRuntimeReference"
      }
}

```

## <a name="type-properties"></a>Propiedades de tipo

Propiedad | DESCRIPCIÓN | Valores permitidos | Obligatorio
-------- | ----------- | -------------- | --------
dataflow | Referencia al flujo de datos que se está ejecutando. | DataFlowReference | Sí
integrationRuntime | Entorno de proceso en el que se ejecuta el flujo de datos. | IntegrationRuntimeReference | Sí
staging.linkedService | Si usa un origen o un receptor de Azure SQL Data Warehouse, es la cuenta de almacenamiento que se utiliza como almacenamiento provisional de PolyBase. | LinkedServiceReference | Solo si el flujo de datos lee o escribe en una instancia de Azure SQL Data Warehouse.
staging.folderPath | Si usa un origen o un receptor de Azure SQL Data Warehouse, es la ruta de la carpeta de la cuenta de almacenamiento de blobs que se utiliza como almacenamiento provisional de PolyBase. | Cadena | Solo si el flujo de datos lee o escribe en una instancia de Azure SQL Data Warehouse.

![Ejecución de flujo de datos](media/data-flow/activity-data-flow.png "Ejecución de flujo de datos")

### <a name="data-flow-integration-runtime"></a>Entorno de ejecución de integración de Data Flow

Seleccione el entorno de ejecución de integración que desee usar con la ejecución de la actividad de Data Flow. De forma predeterminada, Data Factory usará la función de resolución automática de Azure Integration Runtime con cuatro núcleos de trabajo y sin período de vida (TTL). Este entorno de ejecución de integración tendrá un tipo de proceso de uso general y se ejecutará en la misma región que la fábrica. Puede crear entornos de ejecución de integración de Azure propios que definan regiones específicas, el tipo de proceso, el número de núcleos y el período de vida de la ejecución de actividades del flujo de datos.

En el caso de las ejecuciones de canalización, el clúster que se utiliza es de trabajo y tarda varios minutos en arrancar antes de que se inicie la ejecución. Si no se especifica ningún período de vida, cada ejecución de la canalización tardará ese tiempo en iniciarse. Si especifica un período de vida, habrá un grupo de clústeres semiactivos preparados durante el tiempo que se especifique después de la última ejecución, lo que reducirá los tiempos de inicio. Por ejemplo, si tiene un período de vida de 60 minutos y ejecuta un flujo de datos cada hora, el grupo de clústeres permanecerá activo. Para más información, consulte [Azure Integration Runtime](concepts-integration-runtime.md).

![Azure Integration Runtime](media/data-flow/ir-new.png "Azure Integration Runtime")

> [!NOTE]
> La selección del entorno de ejecución de integración en la actividad de Data Flow solo se aplica a las *ejecuciones activadas* de su canalización. La canalización con flujos de datos se depurará en el clúster que se haya especificado en la sesión de depuración.

### <a name="polybase"></a>PolyBase

Si utiliza Azure SQL Data Warehouse como origen o receptor, debe elegir una ubicación de almacenamiento provisional para la carga por lotes de PolyBase. PolyBase utiliza la carga por lotes en lugar de cargar los datos fila a fila. PolyBase reduce considerablemente el tiempo de carga en Azure SQL Data Warehouse.

## <a name="parameterizing-data-flows"></a>Flujos de datos con parámetros

### <a name="parameterized-datasets"></a>Conjuntos de datos con parámetros

Si el flujo de datos utiliza conjuntos de datos con parámetros, establezca los valores de los parámetros en la pestaña **Configuración**.

![Parámetros de ejecución de flujo de datos](media/data-flow/params.png "Parámetros")

### <a name="parameterized-data-flows"></a>Flujos de datos con parámetros

Si el flujo de datos tiene parámetros, establezca los valores dinámicos de los parámetros de flujo de datos en la pestaña **Parámetros**. Puede usar el lenguaje de expresiones de canalización de ADF (solo para los tipos de cadena) o el lenguaje de expresión de Data Flow para asignar valores dinámicos o literales a los parámetros. Para más información, consulte [Parámetros de Data Flow](parameters-data-flow.md).

![Ejemplo de parámetros de ejecución de flujo de datos](media/data-flow/parameter-example.png "Ejemplo de parámetro")

## <a name="pipeline-debug-of-data-flow-activity"></a>Depuración de la canalización de actividades de Data Flow

Para ejecutar una canalización de depuración con una actividad de Data Flow, debe activar el modo de depuración del flujo de datos a través del control deslizante **Data Flow Debug** (Depuración de flujo de datos) en la barra superior. El modo de depuración permite ejecutar el flujo de datos en un clúster de Spark activo. Para más información, consulte [Modo de depuración](concepts-data-flow-debug-mode.md).

![Botón Depurar](media/data-flow/debugbutton.png "Botón Depurar")

La canalización de depuración se ejecuta en el clúster de depuración activo, no en el entorno de ejecución de integración especificado en la configuración de la actividad de Data Flow. Puede elegir el entorno de proceso de depuración al iniciar el modo de depuración.

## <a name="monitoring-the-data-flow-activity"></a>Supervisión de la actividad de Data Flow

La supervisión de la actividad de Data Flow se realiza de forma especial, ya que permite ver información sobre las particiones, el tiempo de cada fase y el linaje de datos. Abra el panel supervisión utilizando el icono con forma de gafas que encontrará en **Acciones**. Para más información, consulte [Supervisión de flujos de datos](concepts-data-flow-monitoring.md).

## <a name="next-steps"></a>Pasos siguientes

Consulte las actividades de flujo de control compatibles con Data Factory: 

- [Actividad If Condition](control-flow-if-condition-activity.md)
- [Actividad de ejecución de canalización](control-flow-execute-pipeline-activity.md)
- [Para cada actividad](control-flow-for-each-activity.md)
- [Actividad de obtención de metadatos](control-flow-get-metadata-activity.md)
- [Actividad Lookup](control-flow-lookup-activity.md)
- [Actividad web](control-flow-web-activity.md)
- [Actividad Until](control-flow-until-activity.md)
