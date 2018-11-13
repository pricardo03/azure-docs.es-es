---
title: Implementación de Azure Machine Learning con Azure IoT Edge | Microsoft Docs
description: En este tutorial, va a implementar Azure Machine Learning como módulo en un dispositivo perimetral
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 59ce51ba4b015c7236df9e20416832e877dd26c6
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748381"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Tutorial: Implementación de Azure Machine Learning como un módulo de IoT Edge (versión preliminar)

Los módulos de IoT Edge se pueden usar para implementar código que, a su vez, implementa una lógica de negocios directamente en los dispositivos de IoT Edge. Este tutorial le guía por la implementación de un módulo de Azure Machine Learning que predice cuándo un dispositivo produce un error basándose en datos de temperatura de la máquina simulada. Para más información sobre Azure ML en IoT Edge, consulte la [documentación de Azure Machine Learning](../machine-learning/desktop-workbench/use-azure-iot-edge-ai-toolkit.md).

El módulo de Azure Machine Learning que crea en este tutorial lee los datos de entorno que el dispositivo genera y etiqueta los mensajes como anómalos o no.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de un módulo de Azure Machine Learning
> * Insertar un contenedor de módulo en Azure Container Registry
> * Implementar un modelo de Azure Machine Learning en un dispositivo de IoT Edge
> * Visualización de datos generados

>[!NOTE]
>Los módulos Azure Machine Learning en Azure IoT Edge están en versión preliminar pública. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Requisitos previos

Un dispositivo de Azure IoT Edge:

* Puede usar la máquina de desarrollo o una máquina virtual como dispositivo Edge siguiendo los pasos que se indican en la guía de inicio rápido para dispositivos de [Linux](quickstart-linux.md) o de [Windows](quickstart.md).
* El módulo de Azure Machine Learning no admite procesadores ARM.

Recursos en la nube:

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) de nivel estándar o gratis en Azure. 
* Un área de trabajo de Azure Machine Learning. Para crear una, siga las instrucciones que se indican en [Preparación de la implementación de modelos en IoT Edge](../machine-learning/service/how-to-deploy-to-iot.md).


### <a name="disable-process-identification"></a>Deshabilitación de la identificación del proceso

>[!NOTE]
>
> Mientras está en la versión preliminar, Azure Machine Learning no admite la característica de seguridad de identificación de procesos habilitada de forma predeterminada con IoT Edge. 
> A continuación se muestran los pasos necesarios para deshabilitarlo. Sin embargo, esto no es adecuado para su uso en producción. Estos pasos solo son necesarios en Linux, ya que esta tarea ya la habrá realizado durante la instalación en tiempo de ejecución de Edge en Windows.

Para deshabilitar la identificación del proceso en el dispositivo de IoT Edge, deberá proporcionar la dirección IP y el puerto para **workload_uri** y **management_uri** en la sección **connect** de la configuración del demonio de IoT Edge.

Obtenga primero la dirección IP. Escriba `ipconfig` en la línea de comandos y copie la dirección IP de la interfaz **docker0**.

Edite el archivo de configuración del demonio de IoT Edge:

```cmd/sh
sudo nano /etc/iotedge/config.yaml
```

Actualice la sección **connect** de la configuración con la dirección IP. Por ejemplo:
```yaml
connect:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Escriba las mismas direcciones de la sección **listen** de la configuración. Por ejemplo:

```yaml
listen:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Cree la variable de entorno IOTEDGE_HOST con la dirección management_uri (para establecerlo de forma permanente, agréguela a `/etc/environment`). Por ejemplo:

```cmd/sh
export IOTEDGE_HOST="http://172.17.0.1:15580"
```


## <a name="create-the-azure-ml-container"></a>Creación del contenedor de Azure Machine Learning
En esta sección, descargará los archivos de modelo entrenados y los convertirá en un contenedor de Azure Machine Learning.

Siga las instrucciones que se indican en la documentación [Preparación de la implementación de modelos en IoT Edge](../machine-learning/service/how-to-deploy-to-iot.md) para crear un contenedor de Docker con el modelo de aprendizaje automático.  Todos los componentes necesarios para la imagen de Docker están en el [repositorio de Git del kit de herramientas de inteligencia artificial para Azure IoT Edge](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

### <a name="view-the-container-repository"></a>Ver el repositorio de contenedores

Compruebe que la imagen de contenedor se creó y almacenó correctamente en Azure Container Registry asociado a su entorno de aprendizaje automático.

1. En [Azure Portal](https://portal.azure.com), vaya a **Todos los servicios** y seleccione **Registros de contenedor**.
2. Seleccione el registro. El nombre debe comenzar con **mlcr** y pertenece al grupo de recursos, a la ubicación y a la suscripción que usó para configurar Administración del módulo.
3. Seleccione **Claves de acceso**.
4. Copie los valores de **Servidor de inicio de sesión**, **Nombre de usuario** y **Contraseña**.  Necesitará estos valores para tener acceso al registro de los dispositivo Edge.
5. Seleccione **Repositorios**.
6. Seleccione **machinelearningmodule**.
7. Ya tiene la ruta completa de la imagen del contenedor. Apúntela para la sección siguiente. Debería tener el aspecto siguiente: **<registry_name>.azurecr.io/machinelearningmodule:1**

## <a name="deploy-to-your-device"></a>Implementación en el dispositivo

1. En [Azure Portal](https://portal.azure.com), navegue hasta su centro de IoT.

1. Vaya a **IoT Edge** y seleccione el dispositivo IoT Edge.

1. Seleccione **Set modules** (Establecer módulos).

1. En la sección **Registry Settings** (Configuración del Registro), agregue las credenciales que copió del registro de contenedores de Azure. 

   ![Adición de las credenciales del Registro](./media/tutorial-deploy-machine-learning/registry-settings.png)

1. Si anteriormente implementó el módulo tempSensor en su dispositivo IoT Edge, puede que se rellene automáticamente. Si todavía no está en la lista de módulos, agréguelo.

    1. Haga clic en **Add** (Agregar) y seleccione **IoT Edge Module** (Módulo de IoT Edge).
    2. En el campo **Nombre**, escriba `tempSensor`.
    3. En el campo **URI de la imagen**, escriba `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`.
    4. Seleccione **Guardar**.

1. Agregue el módulo de Machine Learning que creó.

    1. Haga clic en **Add** (Agregar) y seleccione **IoT Edge Module** (Módulo de IoT Edge).
    1. En el campo **Nombre**, escriba `machinelearningmodule`
    1. En el campo **Imagen**, escriba la dirección de la imagen; por ejemplo, `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. Seleccione **Guardar**.

1. Vuelva al paso **Agregar módulos** y seleccione **Siguiente**.

1. En el paso **Specify Routes** (Especificar rutas), copie el archivo JSON siguiente en el cuadro de texto. La primera ruta transporta los mensajes del sensor de temperatura al módulo de Machine Learning a través del punto de conexión "amlInput", que es el punto de conexión que utilizan todos los módulos de Azure Machine Learning. La segunda ruta transporta mensajes del módulo de Machine Learning a IoT Hub. En esta ruta, ''amlOutput'' es el punto de conexión que usan todos los módulos de Azure Machine Learning para los datos de salida y "$upstream" indica a IoT Hub.

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ```

1. Seleccione **Next** (Siguiente).

1. En el paso **Review Deployment** (Revisar implementación), seleccione **Submit** (Enviar).

1. Vuelva a la página de detalles del dispositivo y seleccione **Actualizar**.  Debería ver el nuevo módulo **machinelearningmodule** en ejecución junto con el módulo **tempSensor** y el módulo runtime de IoT Edge.

## <a name="view-generated-data"></a>Visualización de datos generados

Se pueden ver los mensajes que está generando cada módulo de IoT Edge, y se pueden ver los mensajes que se envían al centro de IoT.

### <a name="view-data-on-your-iot-edge-device"></a>Visualización de datos en un dispositivo IoT Edge

En el dispositivo IoT Edge puede ver los mensajes que se envían desde cada módulo individual. 

Si realiza estos comandos en un dispositivo Linux, puede que necesite usar `sudo` para permisos elevados.

1. Vea todos los módulos en el dispositivo IoT Edge.

   ```cmd/sh
   iotedge list
   ```

2. Vea los mensajes que se envían desde un dispositivo específico. Utilice el nombre del módulo de la salida del comando anterior.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>Visualización de los datos que llegan al centro de IoT

Puede ver los mensajes del dispositivo a la nube que el centro de IoT recibe con la [extensión de Azure IoT Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).

Los siguientes pasos le muestran cómo configurar Visual Studio Code para supervisar los mensajes del dispositivo a la nube que llegan al centro de IoT. 

1. En Visual Studio Code, seleccione **IoT Hub Devices** (Dispositivos de IoT Hub).

2. Seleccione **...**  y, después, seleccione **Set IoT Hub Connection String** (Establecer cadena de conexión de IoT Hub) en el menú.

   ![Menú más de dispositivos de IoT Hub](./media/tutorial-deploy-machine-learning/set-connection.png)

3. En el cuadro de texto que aparece en la parte superior de la página, escriba la cadena de conexión iothubowner para IoT Hub. El dispositivo IoT Edge debe aparecer en la lista de dispositivos de IoT Hub.

4. Vuelva a seleccionar **...**  y, después, **Start monitoring D2C message** (Iniciar la supervisión del mensaje de D2C).

5. Observe los mensajes procedentes de tempSensor cada cinco segundos. El cuerpo del mensaje contiene una propiedad denominada **anomaly** que el módulo de aprendizaje automático proporciona con un valor true o false. La propiedad **AzureMLResponse** contiene el valor "OK" si el modelo se ejecutó correctamente.

   ![Respuesta de Azure Machine Learning en el cuerpo del mensaje](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Limpieza de recursos 

Si prevé seguir con el siguiente artículo recomendado, puede mantener los recursos y las configuraciones que ya ha creado y volverlos a utilizar. También puede seguir usando el mismo dispositivo de IoT Edge como dispositivo de prueba. 

En caso contrario, para evitar gastos, puede eliminar las configuraciones locales y los recursos de Azure que creó en este artículo. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha implementado un módulo de IoT Edge con tecnología de Azure Machine Learning. Puede continuar con cualquiera de los otros tutoriales para aprender otras formas en las que Azure IoT Edge puede ayudarle a convertir los datos en perspectivas empresariales en el perímetro.

> [!div class="nextstepaction"]
> [Clasificación de imágenes con Custom Vision Service](tutorial-deploy-custom-vision.md)

