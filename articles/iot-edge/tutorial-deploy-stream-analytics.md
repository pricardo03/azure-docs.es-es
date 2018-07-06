---
title: 'Tutorial: Implementación de trabajos ASA en dispositivos Azure IoT Edge | Microsoft Docs'
description: Implementación de Azure Stream Analytics como un módulo en un dispositivo de Iot Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/25/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0790f504c978b4302812cffc9b655e817c156da3
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081394"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Tutorial: Implementación de Azure Stream Analytics como un módulo de IoT Edge (versión preliminar)

Muchas soluciones de IoT utilizan servicios de análisis para obtener información sobre los datos a medida que llegan a la nube desde los dispositivos de IoT. Con Azure IoT Edge, puede tomar la lógica de [Azure Stream Analytics][azure-stream] y llevarla al mismo dispositivo. Al procesar los flujos de telemetría en el perímetro, puede reducir la cantidad de datos cargados y reducir el tiempo que se tarda en reaccionar ante la información procesable.

Azure IoT Edge y Azure Stream Analytics están integrados para que pueda crear un trabajo de Azure Stream Analytics en Azure Portal y después implementarlo como un módulo de IoT Edge sin código adicional.  

Azure Stream Analytics proporciona una sintaxis de consulta con una estructura compleja para el análisis de datos tanto en la nube como en los dispositivos de IoT Edge. Para más información sobre Azure Stream Analytics en IoT Edge, consulte la [documentación de Azure Stream Analytics](../stream-analytics/stream-analytics-edge.md).

El módulo de Stream Analytics de este tutorial calcula la temperatura media en una ventana gradual de 30 segundos. Cuando ese promedio alcanza 70, el módulo envía una alerta para que el dispositivo tome medidas. En este caso, esa acción es restablecer el sensor de temperatura simulado. En un entorno de producción, podría usar esta funcionalidad para apagar una máquina o tomar medidas preventivas cuando la temperatura alcance niveles peligrosos. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un trabajo de Azure Stream Analytics para procesar los datos en los dispositivos perimetrales.
> * Conectar el trabajo de Azure Stream Analytics nuevo con otros módulos de IoT Edge.
> * Implemente el trabajo de Azure Stream Analytics en un dispositivo IoT Edge desde Azure Portal.

>[!NOTE]
>Los módulos de Azure Stream Analytics para IoT Edge están en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>requisitos previos

* Una instancia de IoT Hub
* El dispositivo IoT Edge que ha creado y configurado en la guía de inicio rápido para [Windows][lnk-quickstart-win] o [Linux][lnk-quickstart-lin]. 

## <a name="create-an-azure-stream-analytics-job"></a>Creación de un trabajo de Azure Stream Analytics

En esta sección, se crea un trabajo de Azure Stream Analytics para tomar datos de su centro de IoT, realizar consultas en los datos de telemetría enviados desde su dispositivo y luego reenviar los resultados a un contenedor de Azure Blob Storage. Para más información, consulte la sección "Introducción" de la [documentación de Stream Analytics][azure-stream]. 

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Se requiere una cuenta de Azure Storage para los trabajos de Azure Stream Analytics que se use como punto de conexión en la salida del trabajo. En el ejemplo de esta sección se usa el tipo de almacenamiento de blobs. Para más información, consulte la sección "Blobs" de [la documentación de Azure Storage][azure-storage].

1. En Azure Portal, vaya a **Crear un recurso**, escriba **Cuenta de almacenamiento** en el cuadro de búsqueda y, luego, seleccione **Cuenta de almacenamiento: blob, archivo, tabla, cola**.

2. En el panel **Crear cuenta de almacenamiento**, escriba un nombre para la cuenta de almacenamiento, seleccione la misma ubicación donde IoT Hub está almacenado, seleccione el mismo grupo de recursos que IoT Hub y, luego, seleccione **Crear**. Anote el nombre para usarlo más adelante.

    ![Crear una cuenta de almacenamiento][1]


### <a name="create-a-stream-analytics-job"></a>Creación de un trabajo de Stream Analytics

1. En Azure Portal, vaya a **Crear un recurso** > **Internet de las cosas** y, luego, seleccione **Trabajo de Stream Analytics**.

2. En el panel **Nuevo trabajo de Stream Analytics**, haga lo siguiente:

   1. En el cuadro **Nombre del trabajo**, escriba un nombre de trabajo.
   
   2. Utilice el mismo **grupo de recursos** y **ubicación** como instancia de IoT Hub. 

      > [!NOTE]
      > Actualmente, no se admiten los trabajos de Azure Stream Analytics en IoT Edge en la región Oeste de EE. UU. 2. 

   3. En **Entorno de hospedaje**, seleccione **Edge**.
    
3. Seleccione **Crear**.

4. En el trabajo creado, en **Topología de trabajo**, abra el campo **Entradas**.

   ![Entrada de Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Seleccione **Agregar entrada de la secuencia** y, luego, seleccione **Centro de Microsoft Edge**.

6. En el panel **Nueva entrada**, escriba **Temperatura** como alias de entrada. 

7. Seleccione **Guardar**.

8. En **Topología de trabajo**, abra **Salidas**.

   ![Salida de Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_output.png)

9. Seleccione **Agregar** y, luego, **Centro de Microsoft Edge**.

10. En el panel **Nueva salida**, escriba **alerta** como alias de salida. 

11. Seleccione **Guardar**.

12. En **Topología de trabajo**, seleccione **Consulta** y, a continuación, sustituya el texto predeterminado por la siguiente consulta que crea una alerta si la temperatura media de la máquina en una ventana de 30 segundos alcanza los 70 grados:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```

13. Seleccione **Guardar**.

14. En **Configurar**, seleccione **Configuración de IoT Edge**.

15. Seleccione la **cuenta de Storage**  en el menú desplegable.

16. Para el campo **Contenedor**, seleccione **Crear nuevo** y proporcione un nombre para el contenedor de almacenamiento. 

17. Seleccione **Guardar**. 


## <a name="deploy-the-job"></a>Implementación del trabajo

Ya está listo para implementar el trabajo de Azure Stream Analytics en el dispositivo de IoT Edge.

1. En Azure Portal, en la instancia de IoT Hub, vaya a **IoT Edge** y abra la página de detalles del dispositivo IoT Edge.

2. Seleccione **Set modules** (Establecer módulos).  

   Si anteriormente implementó el módulo tempSensor en este dispositivo, puede que se rellene automáticamente. Si no es así, siga estos pasos para agregar el módulo:

   1. Haga clic en **Add** (Agregar) y seleccione **IoT Edge Module** (Módulo de IoT Edge).
   2. Para el nombre, escriba **tempSensor**.
   3. Para el URI de la imagen, escriba **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0**. 
   4. Deje intactos los valores restantes.
   5. Seleccione **Guardar**.

3. Agregue el trabajo de Edge de Azure Stream Analytics con los siguientes pasos:

   1. Haga clic en **Add** (Agregar) y seleccione **Azure Stream Analytics Module** (Módulo de Azure Stream Analytics).
   2. Seleccione la suscripción y el trabajo Edge de Azure Stream Analytics que creó. 
   3. Seleccione **Guardar**.

4. Seleccione **Next** (Siguiente).

5. Reemplace el valor predeterminado de **Rutas** por el código siguiente. Actualice _{moduleName}_ con el nombre del módulo de Azure Stream Analytics. El módulo debe tener el mismo nombre que el trabajo con el que se creó. 

    ```json
    {
        "routes": {
            "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream",
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")",
            "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")"
        }
    }
    ```

6. Seleccione **Next** (Siguiente).

7. En el paso **Review Deployment** (Revisar implementación), seleccione **Submit** (Enviar).

8. Vuelva a la página de detalles del dispositivo y, luego, seleccione **Actualizar**.  

    Debería ver el nuevo módulo de Stream Analytics en ejecución junto con el módulo del agente de IoT Edge y el centro de IoT Edge.

    ![Salida del módulo][7]

## <a name="view-data"></a>Visualización de datos

Ahora puede ir a su dispositivo de IoT Edge para revisar la interacción entre el módulo Azure Stream Analytics y el módulo tempSensor.

1. Compruebe que todos los módulos se ejecutan en Docker:

   ```cmd/sh
   iotedge list  
   ```
<!--
   ![Docker output][8]
-->
2. Vea todos los registros del sistema y datos de métrica. Use el nombre del módulo de Stream Analytics:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Podrá observar que la temperatura de la máquina sube gradualmente hasta alcanzar 70 grados durante 30 segundos. Después, el módulo de Stream Analytics desencadena un restablecimiento y la temperatura de la máquina vuelve a bajar a 21. 

   ![Registro de Docker][9]

## <a name="clean-up-resources"></a>Limpieza de recursos 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Si va a seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar.

En caso contrario, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo para evitar gastos. 

> [!IMPORTANT]
> La eliminación de los recursos de Azure y del grupo de recursos es un proceso irreversible. Una vez eliminados, el grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. Si ha creado una instancia de IoT Hub en un grupo de recursos ya existente que contiene recursos que desea conservar, puede eliminar solo esa instancia en lugar de eliminar todo el grupo de recursos.
>

Para eliminar solo la instancia de IoT Hub, ejecute el siguiente comando usando el nombre del centro y el nombre del grupo de recursos:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Para eliminar un grupo de recursos entero por el nombre:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com) y haga clic en **Grupos de recursos**.

2. Escriba el nombre del grupo de recursos que contiene la instancia de IoT Hub en el cuadro de texto **Filtrar por nombre...**. 

3. A la derecha del grupo de recursos de la lista de resultados, haga clic en **...** y, a continuación, en **Eliminar grupo de recursos**.

<!--
   ![Delete](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)
-->
4. Se le pedirá que confirme la eliminación del grupo de recursos. Escriba otra vez el nombre del grupo de recursos para confirmar y haga clic en **Eliminar**. Transcurridos unos instantes, el grupo de recursos y todos los recursos que contiene se eliminan.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha configurado un trabajo de Azure Stream Analytics para analizar datos desde un dispositivo IoT Edge. A continuación, ha cargado este módulo de Azure Stream Analytics en su dispositivo IoT Edge para procesar y reaccionar al aumento de temperatura localmente, así como para enviar el flujo de datos agregado a la nube. Para ver cómo Azure IoT Edge puede crear más soluciones para la empresa, siga con los otros tutoriales.

> [!div class="nextstepaction"] 
> [Implementación de un modelo de Azure Machine Learning como módulo][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output2.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

