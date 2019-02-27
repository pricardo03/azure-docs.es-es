---
title: Modo de depuración de flujos de datos de asignación de Azure Data Factory
description: Inicio de una sesión de depuración interactiva al crear flujos de datos
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 567f64b8b720588807caeb5e49bae15f14c5b0a7
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329803"
---
# <a name="mapping-data-flow-debug-mode"></a>Modo de depuración de Mapping Data Flow

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Data Flow de Azure Data Factory tiene un modo de depuración, que puede activarse con el botón Depurar de la parte superior de la superficie de diseño. Al diseñar flujos de datos, activar el modo de depuración permitirá ver interactivamente la transformación de la forma de los datos mientras crea y depura sus flujos de datos.

<img src="media/data-flow/debugbutton.png" width="400">

## <a name="overview"></a>Información general
Cuando el modo de depuración está activado, se compilará interactivamente el flujo de datos con un clúster interactivo en Azure Databricks en ejecución. Una vez que desactive la depuración en Azure Data Factory, se cerrará la sesión. Debe tener en cuenta los gastos por hora que genera Azure Databricks durante el tiempo que tiene la sesión de depuración activa.

En la mayoría de los casos, se recomienda crear flujos de datos en modo de depuración para que pueda validar la lógica de negocios y ver las transformaciones de datos antes de publicar su trabajo en Azure Data Factory.

## <a name="debug-mode-on"></a>Activación del modo de depuración
Cuando active el modo de depuración, se le pedirá a través de un formulario en el panel lateral que seleccione su clúster de Azure Databricks interactivo y las opciones para el muestreo de origen. Debe utilizar un clúster interactivo de Azure Databricks y seleccionar un tamaño de muestreo de cada una de sus transformaciones de origen, o bien elegir un archivo de texto que se usará para los datos de prueba.

<img src="media/data-flow/upload.png" width="400">

> [!NOTE]
>Cuando se ejecuta en el modo de depuración en Data Flow, no se escribirán los datos en el receptor de transformación. Una sesión de depuración está diseñada para que funcione como una herramienta de ejecución de pruebas para las transformaciones. Los receptores no son necesarios durante la depuración y se omiten en el flujo de datos. Si desea probar a escribir los datos en el receptor, ejecute el flujo de datos desde una canalización de Azure Data Factory ejecute la depuración desde una canalización.

## <a name="debug-settings"></a>Configuración de depuración
La configuración de depuración puede ser cada origen de su flujo de datos y aparecerá en el panel lateral; también se puede editar seleccionando "Configuración de origen" en la barra de herramientas de diseño de Data Flow. Puede seleccionar los límites o el origen de archivos que se usarán para cada transformación de origen. También puede seleccionar qué clúster de Databricks usar para la depuración.

## <a name="cluster-status"></a>Estado del clúster
Hay un indicador de estado del clúster en la parte superior de la superficie de diseño que se pondrá verde cuando el clúster esté listo para realizar la depuración. Si el clúster ya está activo, el indicador verde aparecerá casi al instante. Si el clúster no se está ejecutando cuando se entra en el modo de depuración, tendrá que esperar 5-7 minutos para que el clúster se ponga en marcha. La luz del indicador estará amarilla hasta que esté listo. Una vez que el clúster esté listo para realizar la depuración de Data Flow, la luz del indicador se pondrá verde.

Cuando haya terminado la depuración, desactive la depuración para que pueda finalizar el clúster de Azure Databricks.

<img src="media/data-flow/datapreview.png" width="400">

## <a name="data-preview"></a>Vista previa de datos
Con la depuración activada, la pestaña Vista previa de datos se ilumina en el panel inferior. Sin el modo de depuración activado, Data Flow mostrará solo los metadatos actuales dentro y fuera de cada una de las transformaciones de la pestaña Inspeccionar. La vista previa de datos solo consultará el número de filas que se ha establecido como el límite en la configuración de origen. Es posible que deba haga clic en "Obtener datos" para actualizar la vista previa de datos.

<img src="media/data-flow/stats.png" width="400">

## <a name="data-profiles"></a>Perfiles de datos
Al seleccionar columnas individuales en la pestaña Vista previa de datos aparecerá un gráfico en el extremo derecho de la cuadrícula de datos con estadísticas detalladas sobre cada campo. Azure Data Factory realizará una determinación basada en el muestreo de datos de qué tipo de gráfico mostrar. Los campos de alta cardinalidad establecerán como valor predeterminado los gráficos NULL o NOT NULL, mientras que los datos categóricos y numéricos que tiene una cardinalidad baja mostrarán los gráficos de barras que muestra la frecuencia de valor de los datos. También verá la longitud máxima/mínima de los campos de cadena, valores máximo y mínimo en los campos numéricos, desviación estándar, percentiles, recuentos y promedio. 

<img src="media/data-flow/chart.png" width="400">
