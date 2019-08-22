---
title: Modo de depuración de flujos de datos de asignación de Azure Data Factory
description: Inicio de una sesión de depuración interactiva al crear flujos de datos
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 945d123c0901722a527e7cc8181c91f09e4e95ec
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69014521"
---
# <a name="mapping-data-flow-debug-mode"></a>Modo de depuración de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="overview"></a>Información general

La asignación de Data Flow de Azure Data Factory tiene un modo de depuración, que puede activarse con el botón "Data Flow Debug" (Depurar Data Flow) de la parte superior de la superficie de diseño. Al diseñar flujos de datos, si se activa el modo de depuración, puede ver interactivamente la transformación de la forma de los datos mientras crea y depura los flujos de datos. La sesión de depuración se puede usar tanto en sesiones de diseño de Data Flow como durante la ejecución de depuración de la canalización de flujos de datos.

![Botón de depuración](media/data-flow/debugbutton.png "Debug button")

Cuando el modo de depuración está activado, creará interactivamente el flujo de datos con un clúster de Spark activo. Una vez que desactive la depuración en Azure Data Factory, se cerrará la sesión. Debe tener en cuenta los gastos por hora que genera Azure Databricks durante el tiempo que tiene la sesión de depuración activa.

En la mayoría de los casos, se recomienda crear instancias de Data Flow en modo de depuración para poder validar la lógica de negocios y ver las transformaciones de datos antes de publicar el trabajo en Azure Data Factory. Use el botón "Depurar" del panel de la canalización para probar el flujo de datos en una canalización.

> [!NOTE]
> Mientras la luz del modo de depuración esté iluminada de color verde en la barra de herramientas de Data Factory, se le cobrará la tarifa de depuración de Data Flow de 8 núcleos por hora de proceso general con un período de vida de 60 minutos. 

## <a name="cluster-status"></a>Estado del clúster

El indicador de estado del clúster en la parte superior de la superficie de diseño se pone de color verde cuando el clúster esté listo para realizar la depuración. Si el clúster ya está activo, el indicador verde aparecerá casi al instante. Si el clúster no se está ejecutando cuando se entra en el modo de depuración, tendrá que esperar 5-7 minutos para que se ponga en marcha. El indicador girará hasta que esté listo.

Cuando haya terminado la depuración, desactive el conmutador de depuración para que el clúster de Azure Databricks pueda finalizar y no se le cobre por la actividad de depuración.

## <a name="debug-settings"></a>Configuración de depuración

Para editar las opciones de depuración, haga clic en "Configuración de depuración" en la barra de herramientas del lienzo de Data Flow. Puede seleccionar los límites de fila o el origen de archivos que se usarán en cada transformación de origen. Los límites de fila de esta configuración solo son para la sesión de depuración actual. También puede seleccionar el servicio vinculado de almacenamiento provisional para usarlo con un origen de SQL Data Warehouse. 

![Configuración de depuración](media/data-flow/debug-settings.png "Debug settings")

Si tiene parámetros en su instancia de Data Flow o cualquiera de los conjuntos de datos a los que se hace referencia, puede especificar los valores que se van usar durante la depuración seleccionando la pestaña **Parámetros**.

![Parámetros de configuración de depuración](media/data-flow/debug-settings2.png "Debug settings parameters")

## <a name="data-preview"></a>Vista previa de datos

Con la depuración activada, la pestaña Vista previa de datos se ilumina en el panel inferior. Sin el modo de depuración activado, Data Flow mostrará solo los metadatos actuales dentro y fuera de cada una de las transformaciones de la pestaña Inspeccionar. La vista previa de datos solo consultará el número de filas que se ha establecido como el límite en la configuración de depuración. Haga clic en **Actualizar** para obtener la vista previa de los datos.

![Vista previa de datos](media/data-flow/datapreview.png "Data Preview")

Cuando se ejecuta en el modo de depuración en Data Flow, no se escribirán los datos en el receptor de transformación. Una sesión de depuración está diseñada para que funcione como una herramienta de ejecución de pruebas para las transformaciones. Los receptores no son necesarios durante la depuración y se omiten en el flujo de datos. Si quiere probar a escribir los datos en el receptor, ejecute la instancia de Data Flow desde una canalización de Azure Data Factory y use la ejecución de la depuración desde una canalización.

### <a name="quick-actions"></a>Acciones rápidas

Una vez obtenida la vista previa de los datos, puede generar una transformación rápida para quitar una columna o realizar en ella una conversión de tipo o una modificación. Haga clic en el encabezado de la columna y seleccione una de las opciones de la barra de herramientas de vista previa de datos.

![Acciones rápidas](media/data-flow/quick-actions1.png "Quick actions")

Una vez que seleccione una modificación, la vista previa de los datos se actualizará inmediatamente. Haga clic en **Confirmar** en la esquina superior derecha para generar una nueva transformación.

![Acciones rápidas](media/data-flow/quick-actions2.png "Quick actions")

Con las opciones **Conversión de tipo** y **Modificar** se generará una transformación Columna derivada, mientras que con la opción **Quitar** se generará una transformación Select.

![Acciones rápidas](media/data-flow/quick-actions3.png "Quick actions")

> [!NOTE]
> Si edita su instancia de Data Flow, deberá volver a obtener la vista previa de los datos para poder agregar una transformación rápida.

### <a name="data-profiling"></a>Generación de perfiles de los datos

Si selecciona una columna en la pestaña Vista previa de datos y hace clic en **Estadísticas** en la barra de herramientas de la vista previa de datos, aparecerá un gráfico en el extremo derecho de la cuadrícula de datos con estadísticas detalladas sobre cada campo. Azure Data Factory realizará una determinación basada en el muestreo de datos de qué tipo de gráfico mostrar. Los campos de alta cardinalidad establecerán como valor predeterminado los gráficos NULL o NOT NULL, mientras que los datos categóricos y numéricos que tienen una cardinalidad baja mostrarán gráficos de barras con la frecuencia de valor de los datos. También verá la longitud máxima y mínima de los campos de cadena, los valores máximos y mínimos en los campos numéricos, la desviación estándar, los percentiles, los recuentos y el promedio.

![Estadísticas de columna](media/data-flow/stats.png "Column statistics")

## <a name="next-steps"></a>Pasos siguientes

* Una vez que haya terminado de compilar y depurar el flujo de datos, [ejecútelo desde una canalización.](control-flow-execute-data-flow-activity.md)
* Al probar la canalización con un flujo de datos, use la [opción de ejecución de depuración](iterative-development-debugging.md) de la canalización.
