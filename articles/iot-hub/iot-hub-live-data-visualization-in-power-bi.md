---
title: Visualización de información en tiempo real de los datos del sensor desde Azure IoT Hub – Power BI | Microsoft Docs
description: Use Power BI para visualizar datos de temperatura y humedad que se recopilan desde el sensor y se le envían a Azure IoT Hub.
author: rangv
keywords: visualización de datos en tiempo real, visualización de datos en directo, visualización de datos de sensor
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: 240895f4e8567426fec8e270b96b9e6528392a46
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58103084"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Visualización de datos del sensor en tiempo real desde Azure IoT Hub mediante Power BI

![Diagrama integral](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Conocimientos que adquirirá

Aprenderá a visualizar los datos del sensor en tiempo real que la instancia de Azure IoT Hub recibe mediante Power BI. Si desea intentar visualizar los datos en IoT Hub con Web Apps, consulte [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md) (Uso de Azure Web Apps para visualizar datos del sensor en tiempo real desde Azure IoT Hub).

## <a name="what-you-do"></a>Qué debe hacer

* Prepare el IoT Hub para el acceso a datos mediante la adición de un grupo de consumidores.

* Cree, configure y ejecute un trabajo de Stream Analytics para la transferencia de datos desde su IoT Hub a su cuenta de Power BI.

* Cree y publique un informe de Power BI para visualizar los datos.

## <a name="what-you-need"></a>Lo que necesita

* Tutorial [Instalación de su dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) completado donde se abordan los siguientes requisitos:
  
  * Una suscripción de Azure activa.
  * Un centro de Azure IoT en su suscripción.
  * Una aplicación cliente que envía mensajes a su centro de Azure IoT.

* Una cuenta de Power BI ([pruebe Power BI de manera gratuita](https://powerbi.microsoft.com/)).

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Creación, configuración y ejecución de un trabajo de Stream Analytics

Comencemos creando un trabajo de Stream Analytics. Después de crear el trabajo, puede definir las entradas, las salidas y la consulta que se usa para recuperar los datos.

### <a name="create-a-stream-analytics-job"></a>Creación de un trabajo de Stream Analytics

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso** > **Internet de las cosas** > **Trabajo de Stream Analytics**.

2. Escriba la siguiente información para el trabajo.

   **Nombre del trabajo**: Nombre del trabajo. El nombre debe ser único globalmente.

   **Grupo de recursos**: use el mismo grupo de recursos que usa el centro de IoT.

   **Ubicación**: use la misma ubicación que el grupo de recursos.

   **Anclar al panel**: active esta opción para facilitar el acceso al centro de IoT desde el panel.

   ![Creación de un trabajo de Stream Analytics en Azure](./media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

3. Haga clic en **Create**(Crear).

### <a name="add-an-input-to-the-stream-analytics-job"></a>Adición de una entrada al trabajo de Stream Analytics

1. Abra el trabajo de Stream Analytics.

2. En **Topología de trabajo**, haga clic en **Entradas**.

3. En el panel **Entradas**, haga clic en **Agregar entrada de flujo** y, a continuación, escriba la siguiente información:

   **Alias de entrada**: alias único de la entrada; seleccione **Proporcionar configuración de IoT Hub de forma manual** a continuación.

   **Origen**: seleccione **Centro de IoT**.
   
   **Punto de conexión**: Haga clic en **Mensajería**.

   **Grupo de consumidores**: seleccione el grupo de consumidores que acaba de crear.

4. Haga clic en **Create**(Crear).

   ![Adición de una entrada a un trabajo de Stream Analytics en Azure](./media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Adición de una salida al trabajo de Stream Analytics

1. En **Topología de trabajo**, haga clic en **Salidas**.

2. En el panel **Salidas**, haga clic en **Agregar** y **Power BI** y, a continuación, escriba la siguiente información:

   **Alias de salida**: el alias único para la salida.

   **Área de trabajo de grupo**: seleccione el área de trabajo de grupo de destino.

   **Nombre del conjunto de datos**: escriba un nombre para el conjunto de datos.

   **Nombre de la tabla**: Escriba un nombre de tabla.

3. Haga clic en **Autorizar** y, a continuación, inicie sesión en su cuenta de Power BI.

4. Haga clic en **Create**(Crear).

   ![Adición de una salida a un trabajo de Stream Analytics en Azure](./media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Configuración de la consulta del trabajo de Stream Analytics

1. En **Topología de trabajo**, haga clic en **Consulta**.

2. Reemplace `[YourInputAlias]` por el alias de entrada del trabajo.

3. Reemplace `[YourOutputAlias]` por el alias de salida del trabajo.

4. Haga clic en **Save**(Guardar).

   ![Adición de una consulta a un trabajo de Stream Analytics en Azure](./media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Ejecución del trabajo de Stream Analytics

En el trabajo de Stream Analytics, haga clic en **Iniciar** > **Ahora** > **Iniciar**. Una vez que el trabajo se inicia correctamente, su estado cambia de **Detenido** a **En ejecución**.

![Ejecución de un trabajo de Stream Analytics en Azure](./media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Creación y publicación de un informe de Power BI para visualizar los datos

1. Asegúrese de que la aplicación de ejemplo se ejecuta en el dispositivo. Si no es así, puede hacer referencia a los tutoriales en [Configurar su dispositivo](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).

2. Inicie sesión en su cuenta de [Power BI](https://powerbi.microsoft.com/en-us/).

3. Haga clic en el área de trabajo que se ha usado, **Mi área de trabajo**.

4. Ahora haga clic en **Conjuntos de datos**.

   Debería ver el conjunto de datos que especificó en el momento de crear la salida para el trabajo de Stream Analytics.

5. Seleccione el conjunto de datos que creó y haga clic en **Agregar informe** (el primer icono a la derecha del nombre del conjunto de datos).

   ![Creación de informe de Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

6. Cree un gráfico de líneas para mostrar la temperatura en tiempo real en un período determinado.

   1. En la página de creación de informes, agregue un gráfico de líneas.

   2. En el panel **Campos**, expanda la tabla que especificó en el momento de crear la salida para el trabajo de Stream Analytics.
   
   3. Arrastre **EventEnqueuedUtcTime** (Hora UTC de evento en cola) al **Eje** en el panel **Visualizaciones**.
   
   4. Arrastre **temperature** (temperatura) a **Valores**.

      Se ha creado un gráfico de líneas. El eje x muestra la fecha y hora en la zona horaria UTC. El eje y muestra la temperatura del sensor.

      ![Adición de un gráfico de líneas de temperatura a un informe de Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

7. Cree otro gráfico de líneas para mostrar la humedad en tiempo real en un período determinado. Para ello, siga los mismos pasos anteriores y coloque **EventEnqueuedUtcTime** (Hora UTC de evento en cola) en el eje x y **humidity** (humedad) en el eje y.

   ![Adición de un gráfico de líneas de humedad a un informe de Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

8. Haga clic en **Guardar** para guardar el informe.

9. Haga clic en **Informes** en el panel izquierdo y, a continuación, haga clic en el informe recién creado.

10. Haga clic en **Archivo** > **Publicar en Web**.

11. Haga clic en **Crear código para insertar** y, a continuación, haga clic en **Publicar**.

Se le ofrecerá el vínculo del informe, que puede compartir con cualquiera para concederle acceso a este, y un fragmento de código para que pueda integrarlo en su blog o sitio web.

![Publicación de informe de Microsoft Power BI](./media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft también ofrece las [aplicaciones móviles de Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) para ver e interactuar con los informes y paneles de Power BI desde su dispositivo móvil.

## <a name="next-steps"></a>Pasos siguientes

Ha utilizado correctamente Power BI para visualizar datos de sensor en tiempo real desde su centro de Azure IoT.

Hay otra manera de visualizar datos desde Azure IoT Hub. Consulte [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md) (Uso de Azure Web Apps para visualizar datos del sensor en tiempo real desde Azure IoT Hub).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
