---
title: 'Implementación de módulos desde Visual Studio Code: Azure IoT Edge | Microsoft Docs'
description: Uso de Visual Studio Code para implementar módulos en un dispositivo IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.reviewer: ''
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 212f103a356dc3663795a5e5453869c99da3fff9
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100827"
---
# <a name="deploy-azure-iot-edge-modules-from-visual-studio-code"></a>Implementación de módulos de Azure IoT Edge desde Visual Studio Code

Una vez que cree módulos de IoT Edge con su lógica empresarial, querrá implementarlos en sus dispositivos para usarlos en el perímetro. Si tiene varios módulos que funcionan conjuntamente para recopilar y procesar datos, puede implementarlos todos a la vez y declarar las reglas de enrutamiento que los conectan. 

En este artículo se muestra cómo crear un manifiesto de implementación de JSON y, luego, usar ese archivo para insertar la implementación en un dispositivo IoT Edge. Para información sobre la creación de una implementación dirigida a varios dispositivos en función de sus etiquetas compartidas, consulte [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor.md).

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure. 
* Un [dispositivo de IoT Edge](how-to-register-device-portal.md) que tenga instalado el entorno de ejecución de Azure IoT Edge. 
* [Visual Studio Code](https://code.visualstudio.com/)
* [Extensión de Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 

## <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. Para más información sobre los manifiestos de implementación y cómo crearlos, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

Para implementar módulos con Visual Studio Code, guarde el manifiesto de implementación localmente como un archivo .JSON. Usará la ruta de acceso al archivo en la sección siguiente, al ejecutar el comando para aplicar la configuración al dispositivo.

Este es un manifiesto de implementación básico con un módulo como ejemplo:

   ```json
   {
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
                 "createOptions": "{}"
               }
             },
             "edgeHub": {
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                 "createOptions": "{}"
               }
             }
           },
           "modules": {
             "tempSensor": {
               "version": "1.0",
               "type": "docker",
               "status": "running",
               "restartPolicy": "always",
               "settings": {
                 "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                 "createOptions": "{}"
               }
             }
           }
         }
       },
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
               "route": "FROM /* INTO $upstream"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       },
       "tempSensor": {
         "properties.desired": {}
       }
     }
   }
   ```
   
## <a name="sign-in-to-access-your-iot-hub"></a>Inicio de sesión para acceder al centro de IoT

Puede usar las extensiones de Azure IoT para Visual Studio Code para realizar operaciones con IoT Hub. Para que estas operaciones funcionen, debe iniciar sesión en su cuenta de Azure y seleccionar la instancia de IoT Hub en la que está trabajando.

1. En Visual Studio Code, abra la vista **Explorer** (Explorador).

2. En la parte inferior de esta vista, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

   ![Sección Expansión de los dispositivos de Azure IoT Hub](./media/how-to-deploy-modules-vscode/azure-iot-hub-devices.png)

3. Haga clic en los puntos suspensivos **...** en el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Si no ve el botón de los puntos suspensivos, mantenga el mouse sobre el encabezado. 

4. Elija **Select IoT Hub** (Seleccionar IoT Hub).

5. Si no ha iniciado sesión en su cuenta de Azure, siga las indicaciones para hacerlo. 

6. Seleccione su suscripción a Azure. 

7. Seleccione IoT Hub. 


## <a name="deploy-to-your-device"></a>Implementación en el dispositivo

Para implementar los módulos en el dispositivo, aplique el manifiesto de implementación que configuró con la información del módulo. 

1. En la vista del explorador de Visual Studio Code, expanda la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). 

2. Haga clic con el botón derecho en el dispositivo que quiere configurar con el manifiesto de implementación. 

3. Seleccione **Create Deployment for Single Device** (Crear una implementación para un dispositivo individual). 

4. Vaya al archivo JSON de manifiesto de implementación que quiere usar y haga clic en **Select Edge Deployment Manifest** (Seleccionar manifiesto de implementación de Edge). 

   ![Selección del manifiesto de implementación de Edge](./media/how-to-deploy-modules-vscode/select-deployment-manifest.png)


Los resultados de la implementación se imprimen en la salida de VS Code. Las implementaciones correctas se aplican a los pocos minutos si el dispositivo de destino está en ejecución y conectado a Internet. 

## <a name="view-modules-on-your-device"></a>Visualización de módulos en el dispositivo

Una vez que los módulos se han implementado en el dispositivo, puede verlos todos en la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub). Seleccione la flecha situada junto a su dispositivo IoT Edge para expandirlo. Se muestran todos los módulos actualmente en ejecución. 

Si ha implementado recientemente nuevos módulos en un dispositivo, mantenga el mouse sobre el encabezado de la sección **Azure IoT Hub Devices** (Dispositivos de Azure IoT Hub) y seleccione el icono de actualización para actualizar la vista. 

Haga clic con el botón derecho en el nombre del módulo para ver y editar el módulo gemelo. 

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar y supervisar módulos de IoT Edge a escala](how-to-deploy-monitor.md)
