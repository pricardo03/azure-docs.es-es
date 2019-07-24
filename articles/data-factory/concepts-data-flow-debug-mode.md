---
title: Modo de depuración de flujos de datos de asignación de Azure Data Factory
description: Inicio de una sesión de depuración interactiva al crear flujos de datos
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: d86725718217caf7fd1d9dd6d5d67362e5de7270
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147394"
---
# <a name="mapping-data-flow-debug-mode"></a>Modo de depuración de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

La asignación de Data Flow de Azure Data Factory tiene un modo de depuración, que puede activarse con el botón "Data Flow Debug" (Depurar Data Flow) de la parte superior de la superficie de diseño. Al diseñar flujos de datos, si activa el modo de depuración podrá ver interactivamente la transformación de la forma de los datos mientras crea y depura los flujos de datos. La sesión de depuración se puede usar tanto en sesiones de diseño de Data Flow como durante la ejecución de depuración de la canalización de flujos de datos.

![Botón de depuración](media/data-flow/debugbutton.png "Debug button")

## <a name="overview"></a>Información general
Cuando el modo de depuración está activado, creará interactivamente el flujo de datos con un clúster de Spark activo. Una vez que desactive la depuración en Azure Data Factory, se cerrará la sesión. Debe tener en cuenta los gastos por hora que genera Azure Databricks durante el tiempo que tiene la sesión de depuración activa.

En la mayoría de los casos, se recomienda crear instancias de Data Flow en modo de depuración para poder validar la lógica de negocios y ver las transformaciones de datos antes de publicar el trabajo en Azure Data Factory. Use el botón "Depurar" del panel de la canalización para probar el flujo de datos dentro de una canalización.

> [!NOTE]
> Mientras la luz del modo de depuración esté iluminada de color verde en la barra de herramientas de Data Factory, se le cobrará la tarifa de depuración de Data Flow de 8 núcleos por hora de proceso general con un período de vida de 60 minutos. 

> [!NOTE]
>Cuando se ejecuta en el modo de depuración en Data Flow, no se escribirán los datos en el receptor de transformación. Una sesión de depuración está diseñada para que funcione como una herramienta de ejecución de pruebas para las transformaciones. Los receptores no son necesarios durante la depuración y se omiten en el flujo de datos. Si quiere probar a escribir los datos en el receptor, ejecute la instancia de Data Flow desde una canalización de Azure Data Factory y use la ejecución de la depuración desde una canalización.

## <a name="debug-settings"></a>Configuración de depuración
Para editar las opciones de depuración, haga clic en "Configuración de depuración" en la barra de herramientas del lienzo de Data Flow. Puede seleccionar los límites o el origen de archivos que se usarán para cada transformación de origen. Los límites de fila de esta configuración solo son para la sesión de depuración actual. También puede seleccionar el servicio vinculado de almacenamiento provisional para usarlo con un origen de SQL Data Warehouse. 

![Configuración de depuración](media/data-flow/debug-settings.png "Debug settings")

## <a name="cluster-status"></a>Estado del clúster
Hay un indicador de estado del clúster en la parte superior de la superficie de diseño que se pondrá verde cuando el clúster esté listo para realizar la depuración. Si el clúster ya está activo, el indicador verde aparecerá casi al instante. Si el clúster no se está ejecutando cuando se entra en el modo de depuración, tendrá que esperar 5-7 minutos para que se ponga en marcha. El indicador girará hasta que esté listo.

Cuando haya terminado la depuración, desactive el conmutador de depuración para que el clúster de Azure Databricks pueda finalizar y no se le cobre por la actividad de depuración.

## <a name="data-preview"></a>Vista previa de datos
Con la depuración activada, la pestaña Vista previa de datos se ilumina en el panel inferior. Sin el modo de depuración activado, Data Flow mostrará solo los metadatos actuales dentro y fuera de cada una de las transformaciones de la pestaña Inspeccionar. La vista previa de datos solo consultará el número de filas que se ha establecido como el límite en la configuración de depuración. Es posible que deba haga clic en "Obtener datos" para actualizar la vista previa de datos.

![Vista previa de datos](media/data-flow/datapreview.png "Data Preview")

## <a name="data-profiles"></a>Perfiles de datos
Al seleccionar columnas individuales en la pestaña Vista previa de datos, aparecerá un gráfico en el extremo derecho de la cuadrícula de datos con estadísticas detalladas sobre cada campo. Azure Data Factory realizará una determinación basada en el muestreo de datos de qué tipo de gráfico mostrar. Los campos de alta cardinalidad establecerán como valor predeterminado los gráficos NULL o NOT NULL, mientras que los datos categóricos y numéricos que tienen una cardinalidad baja mostrarán gráficos de barras con la frecuencia de valor de los datos. También verá la longitud máxima y mínima de los campos de cadena, los valores máximos y mínimos en los campos numéricos, la desviación estándar, los percentiles, los recuentos y el promedio. 

![Estadísticas de columna](media/data-flow/stats.png "Column statistics")

## <a name="next-steps"></a>Pasos siguientes

Una vez que haya terminado de compilar y depurar el flujo de datos, [ejecútelo desde una canalización.](control-flow-execute-data-flow-activity.md)

Al probar la canalización con un flujo de datos, use la [opción de ejecución de depuración](iterative-development-debugging.md) de la canalización.
