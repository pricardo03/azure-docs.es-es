---
title: Cómo implementar el módulo gemelo de OPC para Azure desde cero | Microsoft Docs
description: Cómo implementar a OPC gemelo desde cero.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: f0fc3722ee440b6f50b86f916afef7ddc5876eef
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693403"
---
# <a name="deploy-opc-twin-module-and-dependencies-from-scratch"></a>Implementar el módulo gemelo de OPC y las dependencias desde el principio

El módulo gemelo de OPC se ejecuta en IoT Edge y proporciona varios servicios de borde para el dispositivo gemelo OPC y servicios de registro. 

Hay varias opciones para implementar módulos en su [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) puerta de enlace, entre ellos

- [Implementación de IoT Edge hoja de Azure portal](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal)
- [Implementación mediante la CLI de AZ](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-monitor-cli)

> [!NOTE]
> Para obtener más información sobre los detalles de implementación e instrucciones, vea GitHub [repositorio](https://github.com/Azure/azure-iiot-components).

## <a name="deployment-manifest"></a>Manifiesto de implementación

Todos los módulos se implementan mediante un manifiesto de implementación.  Un ejemplo de manifiesto para implementar ambos [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) y [OPC gemelo](https://github.com/Azure/azure-iiot-opc-twin-module) se muestra a continuación.

```json
{
  "content": {
    "modulesContent": {
      "$edgeAgent": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "runtime": {
            "type": "docker",
            "settings": {
              "minDockerVersion": "v1.25",
              "loggingOptions": "",
              "registryCredentials": {}
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": ""
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "opctwin": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-twin:latest",
                "createOptions": "{\"NetworkingConfig\":{\"EndpointsConfig\":{\"host\":{}}},\"HostConfig\":{\"NetworkMode\":\"host\",\"CapAdd\":[\"NET_ADMIN\"]}}"
              }
            },
            "opcpublisher": {
              "version": "2.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/iotedge/opc-publisher:latest",
                "createOptions": "{\"Hostname\":\"publisher\",\"Cmd\":[\"publisher\",\"--pf=./pn.json\",\"--di=60\",\"--to\",\"--aa\",\"--si=0\",\"--ms=0\"],\"ExposedPorts\":{\"62222/tcp\":{}},\"NetworkingConfig\":{\"EndpointsConfig\":{\"host\":{}}},\"HostConfig\":{\"NetworkMode\":\"host\",\"PortBindings\":{\"62222/tcp\":[{\"HostPort\":\"62222\"}]}}}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "opctwinToIoTHub": "FROM /messages/modules/opctwin/* INTO $upstream",
            "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      }
    }
  }
}
```

## <a name="deploying-from-azure-portal"></a>Implementación de Azure portal

La manera más fácil de implementar los módulos en un dispositivo de puerta de enlace de Azure IoT Edge es a través del portal de Azure.  

### <a name="prerequisites"></a>Requisitos previos

1. Implementar el gemelo OPC [dependencias](howto-opc-twin-deploy-dependencies.md) y obtenido resultante `.env` archivo. Tenga en cuenta la implementada `hub name` de la `PCS_IOTHUBREACT_HUB_NAME` variable resultante `.env` archivo.

2. Registrar e iniciar un [Linux](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-linux) o [Windows](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge-windows) puerta de enlace de IoT Edge y anote su `device id`.

### <a name="deploy-to-an-edge-device"></a>Implementar en un dispositivo perimetral

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) y vaya a IoT Hub.

2. Seleccione **IoT Edge** en el menú izquierdo.

3. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.

4. Seleccione **Set modules** (Establecer módulos).

5. En el **módulos de implementación** sección de la página, seleccione **agregar** y **módulo de IoT Edge.**

6. En el **IoT Edge Custom Module** uso del cuadro de diálogo `opctwin` como nombre del módulo, a continuación, especifique el contenedor *URI de la imagen* como

   ```bash
   mcr.microsoft.com/iotedge/opc-twin:latest
   ```

   Como *crear opciones* use el siguiente JSON:

   ```json
   {"HostConfig":{"NetworkMode":"host","CapAdd":["NET_ADMIN"]}}
   ```

   Rellene los campos opcionales si es necesario. Para más información sobre las opciones de creación del contenedor, la directiva de reinicio y el estado deseado, consulte [Propiedades deseadas de EdgeAgent](https://docs.microsoft.com/azure/iot-edge/module-edgeagent-edgehub#edgeagent-desired-properties). Para más información sobre el módulo gemelo, consulte [Definición o actualización de las propiedades deseadas](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

7. Seleccione **guardar** y repita el paso **5**.  

8. En el cuadro de diálogo de IoT Edge Custom Module, utilice `opcpublisher` como nombre para el módulo y el contenedor *URI de la imagen* como 

   ```bash
   mcr.microsoft.com/iotedge/opc-publisher:latest
   ```

   Como *crear opciones* use el siguiente JSON:

   ```json
   {"Hostname":"publisher","Cmd":["publisher","--pf=./pn.json","--di=60","--to","--aa","--si=0","--ms=0"],"ExposedPorts":{"62222/tcp":{}},"HostConfig":{"PortBindings":{"62222/tcp":[{"HostPort":"62222"}] }}}
   ```

9. Seleccione **guardar** y, a continuación, **siguiente** para continuar con la sección de rutas.

10. En la pestaña rutas, pegue el siguiente 

    ```json
    {
      "routes": {
        "opctwinToIoTHub": "FROM /messages/modules/opctwin/outputs/* INTO $upstream",
        "opcpublisherToIoTHub": "FROM /messages/modules/opcpublisher/outputs/* INTO $upstream"
      }
    }
    ```

    y seleccione **siguiente**

11. Revise la información de implementación y el manifiesto.  Debería ser como el manifiesto de implementación anterior.  Seleccione **Submit** (Enviar).

12. Una vez que los módulos se han implementado en el dispositivo, puede verlos todos en la página **Detalles del dispositivo** del portal. Esta página muestra el nombre de cada módulo implementado, así como información útil, como el estado de la implementación y el código de salida.

## <a name="deploying-using-azure-cli"></a>Implementación mediante la CLI de Azure

### <a name="prerequisites"></a>Requisitos previos

1. Instale la versión más reciente de la [interfaz de línea de comandos de Azure (AZ)](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) desde [aquí](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="quickstart"></a>Guía de inicio rápido

1. Guarde el manifiesto de implementación anterior en un `deployment.json` archivo.  

2. Utilice el siguiente comando para aplicar la configuración a un dispositivo IoT Edge:

   ```bash
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content ./deployment.json
   ```

   El `device id` parámetro distingue mayúsculas de minúsculas. El parámetro "content" apunta al archivo del manifiesto de implementación que guardó. 
    ![salida de conjunto de módulos de IoT Edge de AZ](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/set-modules.png)

3. Una vez que los módulos se han implementado en el dispositivo, puede verlos todos con el siguiente comando:

   ```bash
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

   El parámetro de Id. de dispositivo distingue mayúsculas de minúsculas. ![az iot hub module-identity list output](https://docs.microsoft.com/azure/iot-edge/media/how-to-deploy-cli/list-modules.png)

## <a name="run-and-debug-locally"></a>Ejecutar y depurar localmente

Para problemas lanzando y depurarla es útil para ejecutar los módulos de Edge localmente mediante el [simulador de desarrollo de IoT Edge](https://github.com/Azure/iotedgehubdev).  Proporciona una experiencia de desarrollo local con un simulador para crear, desarrollar, probar, ejecutar y depurar módulos de Azure IoT Edge y las soluciones con el mismo código o bits que se usan en producción.

### <a name="prerequisites"></a>Requisitos previos

1. Implementar el gemelo OPC [dependencias](howto-opc-twin-deploy-dependencies.md).

2. Instalar [Docker CE (18.02.0+)](https://www.docker.com/community-edition) en [Windows](https://docs.docker.com/docker-for-windows/install/), [macOS](https://docs.docker.com/docker-for-mac/install/) o [Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce).

3. Instalar [Docker Compose (1.20.0+)](https://docs.docker.com/compose/install/#install-compose) (sólo es necesario para **Linux**. Compose ya se ha incluido en la instalación de Windows y macOS Docker CE)

4. Instalar [Python (2.7 / 3.5+) y la Pip](https://www.python.org/)

5. Instalar iotedgehubdev mediante la ejecución siguiente comando en el terminal

   ```bash
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> Instalar `iotedgehubdev` a **raíz** en Linux y macOS (*no usar '--usuario ' opción en el comando 'pip install'* ).
> Asegúrese de que no hay ningún tiempo de ejecución de Azure IoT Edge que se ejecutan en el mismo equipo con iotedgehubdev, puesto que requieren los mismos puertos.

### <a name="quickstart"></a>Guía de inicio rápido

1. Siga las instrucciones para [crea un dispositivo de Edge en el portal de Azure](https://docs.microsoft.com/azure/iot-edge/how-to-register-device-portal).  Copie la cadena de conexión del dispositivo de edge.

2. Configurar el simulador utilizando la cadena de conexión de edge.

    ```bash
    iotedgehubdev setup -c <edge-device-connection-string>
    ```

3. Copia por encima de manifiesto en un `deployment.json` archivo en la misma carpeta.  Iniciar la implementación en el simulador mediante

    ```bash
    iotedgehubdev start -d deployment.json
    ```

4. Detenga el simulador con

   ```bash
   iotedgehubdev stop
   ```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar a OPC gemelo desde el principio, aquí es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Implementar a OPC gemelo a un proyecto existente](howto-opc-twin-deploy-existing.md)