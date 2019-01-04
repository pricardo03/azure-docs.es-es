---
title: Probar un trabajo en Azure Stream Analytics con datos de ejemplo
description: En este artículo se describe cómo usar Azure Portal para probar un trabajo de Azure Stream Analytics, crear entradas de ejemplo y cargar datos de ejemplo.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: fca76b632e9bcc27ed762886eaea696a5696ad3f
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53557639"
---
# <a name="test-a-stream-analytics-query-with-sample-data"></a>Probar una consulta de Stream Analytics con datos de ejemplo

Si usa Azure Stream Analytics, puede cargar datos de ejemplo y probar las consultas en Azure Portal sin iniciar ni detener ningún trabajo.

## <a name="upload-sample-data-and-test-the-query"></a>Carga de los datos de ejemplo y prueba de la consulta

1. Inicie sesión en el Portal de Azure. 

2. Busque el trabajo de Stream Analytics existente y selecciónelo.

3. En la página del trabajo de Stream Analytics, en el encabezado **Topología del trabajo**, seleccione **Consulta** para abrir la ventana del editor de consultas. 

4. Para probar la consulta con datos de entrada de ejemplo, haga clic con el botón derecho en cualquiera de las entradas.  A continuación, seleccione **Cargar datos de ejemplo desde un archivo**. Los datos se deben serializar en JSON, CSV o AVRO. La entrada de ejemplo debe estar codificada en UTF-8 y sin comprimir. Solo se admite la coma (,) como delimitador para probar las entrada CSV en el portal.

    ![consulta de prueba del editor de consultas de Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

5. Una vez finalizada la carga, seleccione **Probar** para probar esta consulta con los datos de ejemplo que ha proporcionado.

    ![datos de prueba de ejemplo del editor de consultas de Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

6. Si desea guardar la salida de prueba para un uso posterior, el resultado de la consulta se muestra en el explorador mediante un vínculo a los resultados de la descarga. 

7. Ahora puede modificar iterativamente la consulta y probarla repetidamente para ver cómo cambia la salida.

   ![Salida de ejemplo del editor de consultas de Stream Analytics](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

   Cuando use varias salidas en una consulta, podrá ver los resultados en fichas separadas y alternar fácilmente entre ellas.

8. Después de comprobar los resultados mostrados en el explorador, **guarde** la consulta. A continuación, **inicie** el trabajo y deje que procese los eventos de entrada.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Referencia del lenguaje de consulta de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
