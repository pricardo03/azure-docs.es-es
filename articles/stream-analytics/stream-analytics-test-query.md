---
title: Probar un trabajo en Azure Stream Analytics con datos de ejemplo
description: En este artículo se describe cómo usar Azure Portal para probar un trabajo de Azure Stream Analytics, crear entradas de ejemplo y cargar datos de ejemplo.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/23/2019
ms.custom: seodec18
ms.openlocfilehash: d68c1ee084efc146d69d8404aadb8acbb4cc33d1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73891748"
---
# <a name="test-an-azure-stream-analytics-job-with-sample-data"></a>Probar un trabajo en Azure Stream Analytics con datos de ejemplo

En Azure Stream Analytics, puede probar la consulta sin iniciar o detener el trabajo. Puede probar consultas en los datos entrantes del receptor de entrada o en los datos de ejemplo cargados desde un archivo local en Azure Portal. También puede probar las consultas de forma local desde los datos de ejemplo locales o los datos en directo en [Visual Studio](stream-analytics-live-data-local-testing.md) y [Visual Studio Code](vscode-local-run.md). 

## <a name="sample-incoming-data-from-input"></a>Datos entrantes de ejemplo de entrada

Azure Stream Analytics captura automáticamente los eventos de la entrada de streaming. Puede ejecutar consultas en el ejemplo predeterminado o establecer un período de tiempo específico para el ejemplo.

1. Inicie sesión en el Portal de Azure.

2. Busque y seleccione el trabajo de Stream Analytics existente.

3. En la página del trabajo de Stream Analytics, en el encabezado **Topología del trabajo**, seleccione **Consulta** para abrir la ventana del editor de consultas. 

4. Para ver una lista de ejemplos de eventos de entrada, seleccione la entrada con el icono de archivo, y los eventos de ejemplo aparecerán automáticamente en la **vista previa de entrada**. 

   a. El tipo de serialización de los datos se detecta automáticamente si es JSON o CSV. También puede cambiarlo manualmente a JSON, CSV y AVRO con solo cambiar la opción en el menú desplegable.
    
   b. Use el selector para ver los datos en los formatos de **tabla** o **sin procesar**.
    
   c. Si los datos que aparecen no están actualizados, seleccione **Actualizar** para ver los eventos más recientes.

   La siguiente tabla es un ejemplo de datos en **formato de tabla**:

   ![Entrada de ejemplo de Azure Stream Analytics en formato de tabla](./media/stream-analytics-test-query/asa-sample-table.png)

   La tabla siguiente es un ejemplo de datos en **formato sin procesar**:

   ![Entrada de ejemplo de Azure Stream Analytics en formato sin procesar](./media/stream-analytics-test-query/asa-sample-raw.png)

5. Para probar la consulta con los datos de entrada, seleccione **Test query** (Probar consulta). Los resultados aparecen en la pestaña **Test query** (Probar consulta). También puede seleccionar **Download results** (Descargar resultados) para descargar los resultados.

   ![Resultados de prueba de consulta de ejemplo de Azure Stream Analytics](./media/stream-analytics-test-query/asa-test-query.png)

6. Para probar la consulta con un intervalo de tiempo específico de eventos entrantes, seleccione **Select time range** (Seleccionar intervalo de tiempo).
   
   ![Intervalo de tiempo de Azure Stream Analytics para los eventos de ejemplo entrantes](./media/stream-analytics-test-query/asa-select-time-range.png)

7. Establezca el intervalo de tiempo de los eventos que quiere usar para probar la consulta y seleccione **Sample** (Ejemplo). Dentro de ese período de tiempo, puede recuperar hasta 1000 eventos o 1 MB, lo que suceda primero.

   ![Intervalo de tiempo establecido de Azure Stream Analytics para los eventos de ejemplo entrantes](./media/stream-analytics-test-query/asa-set-time-range.png)

8. Una vez que se muestrean los eventos en el intervalo de tiempo seleccionado, aparecen en la pestaña **Input preview** (Vista previa de entrada).

   ![Resultados de la prueba de vista de Azure Stream Analytics](./media/stream-analytics-test-query/asa-view-test-results.png)

9. Seleccione **Reset** (Restablecer) para ver la lista de ejemplos de eventos entrantes. Al seleccionar esta opción, la selección del intervalo de tiempo se perderá. Seleccione **Test query** (Probar consulta) para probar la consulta y revise los resultados de la pestaña **Test results** (Resultados de prueba).

10. Cuando realice cambios en la consulta, seleccione **Save query** (Guardar consulta) para probar la nueva lógica de consulta. Esto le permite modificar iterativamente la consulta y probarla repetidamente para ver cómo cambia la salida.

11. Después de comprobar los resultados que se muestran en el explorador, está listo para **iniciar** el trabajo.

## <a name="upload-sample-data-from-a-local-file"></a>Carga de datos de ejemplo desde un archivo local

En lugar de usar datos en vivo, puede usar datos de ejemplo de un archivo local para probar la consulta de Azure Stream Analytics.

1. Inicie sesión en el Portal de Azure.
   
2. Busque el trabajo de Stream Analytics existente y selecciónelo.

3. En la página del trabajo de Stream Analytics, en el encabezado **Topología del trabajo**, seleccione **Consulta** para abrir la ventana del editor de consultas.

4. Para probar la consulta con un archivo local, seleccione **Upload sample input** (Cargar entrada de ejemplo) en la pestaña **Input preview** (Vista previa de entrada). 

   ![Carga del archivo de ejemplo de Azure Stream Analytics](./media/stream-analytics-test-query/asa-upload-sample-file.png)

5. Cargue el archivo local para probar la consulta. Solo se pueden cargar archivos con los formatos JSON, CSV o AVRO. Seleccione **Aceptar**.

   ![Carga del archivo de ejemplo de Azure Stream Analytics](./media/stream-analytics-test-query/asa-upload-sample-json-file.png)

6. En cuanto cargue el archivo, también puede ver su contenido en formato de tabla o en su formato sin procesar. Si selecciona **Reset** (Restablecer), los datos de ejemplo volverán a los datos de entrada entrantes explicados en la sección anterior. Puede cargar cualquier otro archivo para probar la consulta en cualquier momento.

7. Seleccione **Test query** (Probar consulta) para probar la consulta con el archivo de ejemplo cargado.

8. Los resultados de la prueba se muestran en función de la consulta. Puede cambiar la consulta y seleccionar **Save query** (Guardar consulta) para probar la nueva lógica de consulta. Esto le permite modificar iterativamente la consulta y probarla repetidamente para ver cómo cambia la salida.

9. Cuando se usan varias salidas en la consulta, los resultados se muestran en función de la salida seleccionada. 

   ![Salida seleccionada de Azure Stream Analytics](./media/stream-analytics-test-query/asa-sample-test-selected-output.png)

10. Después de comprobar los resultados mostrados en el explorador, puede **iniciar** el trabajo.

## <a name="next-steps"></a>Pasos siguientes

* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)

* [Ejemplos de consulta para patrones de uso comunes de Stream Analytics](stream-analytics-stream-analytics-query-patterns.md)

* [Información sobre las entradas de Azure Stream Analytics](stream-analytics-add-inputs.md)

* [Información sobre las salidas desde Azure Stream Analytics](stream-analytics-define-outputs.md)