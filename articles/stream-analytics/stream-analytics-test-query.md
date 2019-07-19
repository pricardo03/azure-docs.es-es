---
title: Probar un trabajo en Azure Stream Analytics con datos de ejemplo
description: En este artículo se describe cómo usar Azure Portal para probar un trabajo de Azure Stream Analytics, crear entradas de ejemplo y cargar datos de ejemplo.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 6/21/2019
ms.custom: seodec18
ms.openlocfilehash: 1a9caf83c6f4cd4ed15290afc872043c11234552
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508706"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Probar una consulta de Stream Analytics con datos de ejemplo

Si usa Azure Stream Analytics, puede muestrear los datos de las entradas o cargar datos de ejemplo para probar las consultas de Azure Portal sin iniciar o detener ningún trabajo.

## <a name="upload-or-sample-data-from-a-live-source-to-test-the-query"></a>Carga o muestreo de datos de un origen en directo para probar la consulta

1. Inicie sesión en el Portal de Azure. 

2. Busque el trabajo de Stream Analytics existente y selecciónelo.

3. En la página del trabajo de Stream Analytics, en el encabezado **Topología del trabajo**, seleccione **Consulta** para abrir la ventana del editor de consultas. 

4. Para probar la consulta, puede muestrear los datos de una entrada en directo o cargarlos desde un archivo. Los datos se deben serializar en JSON, CSV o AVRO. La entrada de ejemplo debe estar codificada en UTF-8 y sin comprimir. Solo se admite la coma (,) como delimitador para probar las entrada CSV en el portal.

    1. Uso de la entrada en directo: haga clic con el botón derecho en cualquiera de las entradas. A continuación, seleccione **Datos de ejemplo de la entrada**. En la siguiente pantalla, puede establecer la duración de la muestra. Al muestrear los eventos de un origen en directo se recuperan hasta 1000 eventos o 1 MB (lo que suceda primero), por lo que los datos muestreados no pueden representar el intervalo de tiempo completo especificado.

    1. Uso del archivo: haga clic con el botón derecho en cualquiera de las entradas. A continuación, seleccione **Cargar datos de ejemplo desde un archivo**. 

    ![consulta de prueba del editor de consultas de Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Una vez finalizado el muestreo o la carga, seleccione **Probar** para probar esta consulta con los datos de ejemplo que ha proporcionado.

    ![datos de prueba de ejemplo del editor de consultas de Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Si desea guardar la salida de prueba para un uso posterior, el resultado de la consulta se muestra en el explorador mediante un vínculo a los resultados de la descarga. 

7. Ahora puede modificar iterativamente la consulta y probarla repetidamente para ver cómo cambia la salida.

   ![Salida de ejemplo del editor de consultas de Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Cuando use varias salidas en una consulta, podrá ver los resultados en fichas separadas y alternar fácilmente entre ellas.

8. Después de comprobar los resultados mostrados en el explorador, **guarde** la consulta. A continuación, **inicie** el trabajo y deje que procese los eventos de entrada.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Referencia del lenguaje de consulta de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
