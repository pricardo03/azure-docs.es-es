---
title: 'Tutorial: Implementación de trabajos ASA en dispositivos Azure IoT Edge | Microsoft Docs'
description: En este tutorial, va a implementar Azure Stream Analytics como un módulo en un dispositivo de Iot Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 2d22e995f489a6379a2f134b461b8bb17160919e
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49113842"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module-preview"></a>Tutorial: Implementación de Azure Stream Analytics como un módulo de IoT Edge (versión preliminar)

Muchas soluciones de IoT utilizan servicios de análisis para obtener información sobre los datos a medida que llegan a la nube desde los dispositivos de IoT. Con Azure IoT Edge, puede tomar la lógica de [Azure Stream Analytics][azure-stream] y llevarla al mismo dispositivo. Al procesar los flujos de telemetría en el perímetro, puede reducir la cantidad de datos cargados y reducir el tiempo que se tarda en reaccionar ante la información procesable.

Azure IoT Edge y Azure Stream Analytics están integrados para que pueda crear un trabajo de Azure Stream Analytics en Azure Portal y después implementarlo como un módulo de IoT Edge sin código adicional.  

Azure Stream Analytics proporciona una sintaxis de consulta con una estructura compleja para el análisis de datos tanto en la nube como en los dispositivos de IoT Edge. Para más información sobre Azure Stream Analytics en IoT Edge, consulte la [documentación de Azure Stream Analytics](../stream-analytics/stream-analytics-edge.md).

El módulo de Stream Analytics de este tutorial calcula la temperatura media en una ventana gradual de 30 segundos. Cuando dicho promedio alcanza el valor de 70, el módulo envía una alerta para que el dispositivo tome medidas. En este caso, esa acción es restablecer el sensor de temperatura simulado. En un entorno de producción, podría usar esta funcionalidad para apagar una máquina o tomar medidas preventivas cuando la temperatura alcance niveles peligrosos. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un trabajo de Azure Stream Analytics para procesar los datos en los dispositivos perimetrales.
> * Conectar el trabajo de Azure Stream Analytics nuevo con otros módulos de IoT Edge.
> * Implemente el trabajo de Azure Stream Analytics en un dispositivo IoT Edge desde Azure Portal.

<center>
![Diagrama de arquitectura del tutorial](./media/tutorial-deploy-stream-analytics/ASATutorialDiagram.png)
</center>

>[!NOTE]
>Los módulos de Azure Stream Analytics para IoT Edge están en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para dispositivos de [Linux](quickstart-linux.md) o de [Windows](quickstart.md).

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure. 


## <a name="create-an-azure-stream-analytics-job"></a>Creación de un trabajo de Azure Stream Analytics

En esta sección, se crea un trabajo de Azure Stream Analytics para tomar datos de su centro de IoT, realizar consultas en los datos de telemetría enviados desde su dispositivo y luego reenviar los resultados a un contenedor de Azure Blob Storage. 

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Cuando se crea un trabajo de Azure Stream Analytics para ejecutarse en un dispositivo de IoT Edge, se debe almacenar de forma que se pueda llamar desde el dispositivo. Puede usar una cuenta de Azure Storage o crear una nueva ahora. 

1. En Azure Portal, vaya a **Crear un recurso** > **Storage** > **Cuenta de almacenamiento: blob, archivo, tabla, cola**. 

1. Especifique los siguientes valores para crear una cuenta de almacenamiento:

   | Campo | Valor |
   | ----- | ----- |
   | NOMBRE | Especifique un nombre único para la cuenta de almacenamiento. | 
   | Ubicación | Elija una ubicación cercana a usted. |
   | Subscription | Elija la misma suscripción que IoT Hub. |
   | Grupos de recursos | Se recomienda usar el mismo grupo de recursos para todos los recursos de prueba que se crean en las guías de inicio rápido y los tutoriales de IoT Edge. Por ejemplo, **IoTEdgeResources**. |

1. Mantenga los valores predeterminados en los restantes campos y seleccione **Crear**. 

### <a name="create-a-new-job"></a>Crear un nuevo trabajo

1. En Azure Portal, vaya a **Crear un recurso** > **Internet de las cosas** > **Stream Analytics Job**.

1. Especifique los siguientes valores para crear un trabajo:

   | Campo | Valor |
   | ----- | ----- |
   | Nombre del trabajo | Especifique el nombre del trabajo. Por ejemplo, **IoTEdgeJob** | 
   | Subscription | Elija la misma suscripción que IoT Hub. |
   | Grupos de recursos | Se recomienda usar el mismo grupo de recursos para todos los recursos de prueba que se crean en las guías de inicio rápido y los tutoriales de IoT Edge. Por ejemplo, **IoTEdgeResources**. |
   | Ubicación | Elija una ubicación cercana a usted. | 
   | Entorno de hospedaje | Seleccionar **Edge**. |
 
1. Seleccione **Crear**.

### <a name="configure-your-job"></a>Configuración del trabajo

Una vez que se ha creado el trabajo de Stream Analytics en Azure Portal, puede configurarlo con una entrada, una salida y una consulta para ejecutarlo en los datos que lo atraviesan. 

Mediante los tres elementos de entrada, salida y consulta, esta sección crea un trabajo que recibe los datos de temperatura del dispositivo IoT Edge. Analiza dichos datos en una ventana de 30 segundos con desplazamiento. Si la temperatura media en dicha ventana supera los 70 grados, se envía una alerta al dispositivo IoT Edge. En la siguiente sección, en la que implementa el trabajo, especificará exactamente de dónde proceden los datos y a dónde se dirigen.  

1. Vaya al trabajo de Stream Analytics en Azure Portal. 

1. En **Topología de trabajo**, seleccione **Entradas** y, después, **Agregar entrada de flujo**.

   ![Entrada de Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_input.png)

1. Elija **Centro de Microsoft Edge** en la lista desplegable.

1. En el panel **Nueva entrada**, escriba **Temperatura** como alias de entrada. 

1. Mantenga los valores predeterminados en los restantes campos y seleccione **Guardar**.

1. En **Topología de trabajo**, abra **Salidas** y seleccione **Agregar**.

   ![Salida de Azure Stream Analytics](./media/tutorial-deploy-stream-analytics/asa_output.png)

1. Elija **Centro de Microsoft Edge** en la lista desplegable.

1. En el panel **Nueva salida**, escriba **alerta** como alias de salida. 

1. Mantenga los valores predeterminados en los restantes campos y seleccione **Guardar**.

1. En **Topología de trabajo**, seleccione **Consulta**.

1. Reemplace el texto predeterminado por la siguiente consulta. El código SQL envía un comando de restablecimiento a la salida de la alerta si la temperatura media del equipo en una ventana de 30 segundos alcanza los 70 grados. El comando de restablecimiento se ha programado previamente en el sensor como una acción que se puede realizar. 

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

1. Seleccione **Guardar**.

### <a name="configure-iot-edge-settings"></a>Configuración de los valores de IoT Edge

Para preparar el trabajo de Stream Analytics que se va a implementar en un dispositivo IoT Edge, es preciso asociar el trabajo con un contenedor en una cuenta de almacenamiento. Al importar un trabajo, la definición del mismo se exporta en el contenedor de almacenamiento. 

1. En **Configurar**, seleccione **Configuración de cuenta de almacenamiento**.

1. Seleccione **Agregar cuenta de almacenamiento**. 

1. Seleccione la **cuenta de Storage**  en el menú desplegable.

1. Para el campo **Contenedor**, seleccione **Crear nuevo** y proporcione un nombre para el contenedor de almacenamiento. 

1. Seleccione **Guardar**. 

## <a name="deploy-the-job"></a>Implementación del trabajo

Ya está listo para implementar el trabajo de Azure Stream Analytics en el dispositivo de IoT Edge. 

En esta sección, se usa el asistente de **Establecer módulos** en Azure Portal para crear un *manifiesto de implementación*. Un manifiesto de implementación es un archivo JSON que describe todos los módulos que se implementarán en un dispositivo, los registros de contenedores que almacenan las imágenes del módulo, cómo se deben administrar los módulos y la forma en que los módulos pueden comunicarse entre sí. El dispositivo IoT Edge recupera su manifiesto de implementación de IoT Hub y, después, usa su información para implementar y configurar todos los módulos que tiene asignados. 

En este tutorial se implementan dos módulos. El primero es **tempSensor**, que es un módulo que simula un sensor de temperatura y humedad. El segundo es su trabajo de Stream Analytics. El módulo del sensor proporciona el flujo de datos que se analizará en la consulta de trabajo. 

1. En Azure Portal, en la instancia de IoT Hub, vaya a **IoT Edge** y abra la página de detalles del dispositivo IoT Edge.

1. Seleccione **Set modules** (Establecer módulos).  

1. Si anteriormente implementó el módulo tempSensor en este dispositivo, puede que se rellene automáticamente. Si no lo hace, siga estos pasos para agregar el módulo:

   1. Haga clic en **Add** (Agregar) y seleccione **IoT Edge Module** (Módulo de IoT Edge).
   1. Para el nombre, escriba **tempSensor**.
   1. Para el URI de la imagen, escriba **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0**. 
   1. Deje los restantes valores tal cual y seleccione **Guardar**.

1. Agregue el trabajo de Edge de Azure Stream Analytics con los siguientes pasos:

   1. Haga clic en **Add** (Agregar) y seleccione **Azure Stream Analytics Module** (Módulo de Azure Stream Analytics).
   1. Seleccione la suscripción y el trabajo Edge de Azure Stream Analytics que creó. 
   1. Seleccione **Guardar**.

1. Una vez que el trabajo de Stream Analytics se publica en el contenedor de almacenamiento que creó, haga clic en el nombre del módulo para ver cómo se estructura un módulo de Stream Analytics. 

   El identificador URI de la imagen apunta a una imagen de Azure Stream Analytics estándar. Se trata de la misma imagen que se usa para todos los trabajos que se implementan en un dispositivo IoT Edge. 

   El módulo gemelo se configura con una propiedad deseada llamada **ASAJobInfo**. El valor de dicha propiedad apunta a la definición del trabajo en el contenedor de almacenamiento. Esta propiedad es la forma en que la imagen de Stream Analytics se configura con la información específica de su trabajo. 

1. Cierre la página del módulo.

1. Anote el nombre de su módulo de Stream Analytics porque lo necesitará en el paso siguiente y, después, seleccione **Siguiente** para continuar.

1. Reemplace el valor predeterminado de **Rutas** por el código siguiente. Actualice las tres instancias de _{moduleName}_ con el nombre del módulo de Azure Stream Analytics. 

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

   Las rutas que se declaran aquí definen el flujo de datos que atraviesa el dispositivo IoT Edge. Los datos de telemetría de tempSensor se envían a IoT Hub y a la entrada de **temperatura** que se configuró en el trabajo de Stream Analytics. Los mensajes de salida de **alerta** se envían a IoT Hub y al módulo tempSensor para desencadenar el comando de restablecimiento. 

1. Seleccione **Next** (Siguiente).

1. En el paso **Review Deployment** (Revisar implementación), seleccione **Submit** (Enviar).

1. Vuelva a la página de detalles del dispositivo y, luego, seleccione **Actualizar**.  

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
1. Vea todos los registros del sistema y datos de métrica. Use el nombre del módulo de Stream Analytics:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

Podrá observar que la temperatura de la máquina sube gradualmente hasta alcanzar 70 grados durante 30 segundos. Después, el módulo de Stream Analytics desencadena un restablecimiento y la temperatura de la máquina vuelve a bajar a 21. 

   ![Registro de Docker][9]

## <a name="clean-up-resources"></a>Limpieza de recursos 

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 
 
[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha configurado un trabajo de Azure Stream Analytics para analizar datos desde un dispositivo IoT Edge. A continuación, ha cargado este módulo de Azure Stream Analytics en su dispositivo IoT Edge para procesar y reaccionar al aumento de temperatura localmente, así como para enviar el flujo de datos agregado a la nube. Para ver cómo Azure IoT Edge puede crear más soluciones para la empresa, siga con los otros tutoriales.

> [!div class="nextstepaction"] 
> [Implementación de un modelo de Azure Machine Learning como módulo][lnk-ml-tutorial]

<!-- Images. -->
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

