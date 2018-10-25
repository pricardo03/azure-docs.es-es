---
title: Solución de problemas en las consultas de Azure Stream Analytics
description: En este artículo se describen técnicas para solucionar problemas de las consultas en los trabajos de Azure Stream Analytics.
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 10/11/2018
ms.openlocfilehash: c437f350e394dc8c264903508a2a5a66fa8225a7
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49346573"
---
# <a name="troubleshoot-azure-stream-analytics-queries"></a>Solución de problemas en las consultas de Azure Stream Analytics

En este artículo se describen problemas comunes con el desarrollo de consultas a Stream Analytics y cómo solucionarlos.

## <a name="query-is-not-producing-expected-output"></a>La consulta no produce el resultado esperado 
1.  Examine los errores mediante la comprobación local:
    - En la pestaña **Consulta**, seleccione **Probar**. Use los datos de ejemplo que descargó para [probar la consulta](stream-analytics-test-query.md). Examine los errores e intente corregirlos.   
    - También puede [probar la consulta directamente con entradas en vivo](stream-analytics-live-data-local-testing.md) mediante las herramientas de Stream Analytics para Visual Studio.

2.  Si usa [**Marca de tiempo por**](https://msdn.microsoft.com/library/azure/mt573293.aspx), asegúrese de que los eventos tengan marcas de tiempo mayores que la [hora de inicio del trabajo](stream-analytics-out-of-order-and-late-events.md).

3.  Elimine los errores comunes, como:
    - Una cláusula [**WHERE**](https://msdn.microsoft.com/library/azure/dn835048.aspx) en la consulta filtrada por todos los eventos, que impide que se genere alguna salida.
    - Un error de una función [**CAST**](https://msdn.microsoft.com/azure/stream-analytics/reference/cast-azure-stream-analytics), que provoca que el trabajo no se realice. Para evitar errores de conversión de tipo, utilice [**TRY_CAST**](https://msdn.microsoft.com/azure/stream-analytics/reference/try-cast-azure-stream-analytics) en su lugar.
    - Cuando use funciones de ventana, espere la duración entera de la ventana para ver una salida de la consulta.
    - La marca de tiempo de los eventos es anterior a la hora de inicio del trabajo y, por tanto, los eventos se descartan.

4.  Asegúrese de que las directivas de ordenación de eventos están configuradas según lo previsto. Vaya a la **Configuración** y seleccione [**Ordenación de eventos**](stream-analytics-out-of-order-and-late-events.md). La directiva *no* se aplica cuando usa el botón **Probar** para probar la consulta. El resultado es una diferencia entre probar en el explorador frente a ejecutar el trabajo de producción. 

5. Depure mediante auditorías y registros de diagnóstico:
    - Use [Registros de auditoría](../azure-resource-manager/resource-group-audit.md) y filtre para identificar y depurar errores.
    - Use [registros de diagnóstico de trabajo](stream-analytics-job-diagnostic-logs.md) para identificar y depurar errores.

## <a name="job-is-consuming-too-many-streaming-units"></a>El trabajo consume demasiadas unidades de streaming
Asegúrese de estar aprovechando la paralelización en Azure Stream Analytics. Puede aprender a [escalar con la paralelización de consultas](stream-analytics-parallelization.md) de los trabajos de Stream Analytics mediante la configuración de particiones de entrada y el ajuste de la definición de consultas de análisis.

## <a name="debug-queries-progressively"></a>Depuración gradual de consultas

En el procesamiento de datos en tiempo real, puede ser útil conocer qué aspecto tendrán los datos en el medio de la consulta. Como las entradas o pasos de un trabajo de Azure Stream Analytics se pueden leer muchas veces, puede escribir instrucciones SELECT INTO adicionales. Si lo hace, se generan daros intermedios en el almacenamiento y puede inspeccionar la integridad de los datos, igual que hacen las *variables de inspección* cuando se depura un programa.

La siguiente consulta de ejemplo en un trabajo de Azure Stream Analytics tiene una entrada de secuencias, dos entradas de datos de referencia y una salida a Azure Table Storage. La consulta combina datos del centro de eventos y dos blobs de referencia para obtener la información de nombre y categoría:

![Ejemplo de consulta SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-query1.png)

Tenga en cuenta que el trabajo está en ejecución, pero no se genera ningún evento en la salida. En el icono **Supervisión**, que se muestra aquí, puede ver que la entrada genera datos, pero no sabe qué paso de la cláusula **JOIN** ha hecho que se descarten todos los eventos.

![Icono de Supervisión](./media/stream-analytics-select-into/stream-analytics-select-into-monitor.png)
 
En esta situación, puede agregar unas cuantas instrucciones SELECT INTO adicionales para "registrar" los resultados intermedios de JOIN y los datos que se leen de la entrada.

En este ejemplo, hemos agregado dos nuevas "salidas temporales". Estas pueden ser cualquier receptor que desee. Aquí usamos Azure Storage como ejemplo:

![Adición de instrucciones SELECT INTO adicionales](./media/stream-analytics-select-into/stream-analytics-select-into-outputs.png)

A continuación, puede volver a escribir la consulta del modo siguiente:

![Consulta SELECT INTO reescrita](./media/stream-analytics-select-into/stream-analytics-select-into-query2.png)

Ahora, inicie de nuevo el trabajo y deje que se ejecute durante unos minutos. A continuación, consulte temp1 y temp2 con Visual Studio Cloud Explorer para generar las siguientes tablas:

**tabla temp1**
![SELECT INTO tabla temp1](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-1.png)

**tabla temp2**
![SELECT INTO tabla temp2](./media/stream-analytics-select-into/stream-analytics-select-into-temp-table-2.png)

Como puede ver, temp1 y temp2 tienen ambas datos, y la columna de nombre está rellenada correctamente en temp2. Sin embargo, dado que todavía no hay ningún dato en la salida, algo sucede:

![Tabla SELECT INTO output1 sin datos](./media/stream-analytics-select-into/stream-analytics-select-into-out-table-1.png)

Mediante el muestreo de los datos, puede estar casi seguro de que el problema está relacionado con la segunda cláusula JOIN. Puede descargar los datos de referencia del blob y echar un vistazo:

![Tabla de referencia de SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-1.png)

Como puede ver, el formato del GUID en los datos de esta referencia es distinto del formato de la columna [from] en temp2. Por eso los datos no llegan a output1 como estaba previsto.

Puede corregir el formato de datos, cargarlo para que haga referencia al blob e intentarlo de nuevo:

![Tabla temporal de SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-ref-table-2.png)

Esta vez, los datos en la salida tienen el formato adecuado y se han rellenado según lo previsto.

![Tabla final de SELECT INTO](./media/stream-analytics-select-into/stream-analytics-select-into-final-table.png)

## <a name="get-help"></a>Obtener ayuda

Para obtener ayuda adicional, pruebe nuestro [foro de Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referencia de API de REST de administración de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)