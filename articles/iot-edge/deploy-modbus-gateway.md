---
title: 'Traducción de protocolos modbus con puertas de enlace: Azure IoT Edge | Microsoft Docs'
description: Permitir dispositivos que usan Modbus TCP para comunicarse con Azure IoT Hub mediante la creación de un dispositivo de puerta de enlace con IoT Edge
author: kgremban
manager: philmea
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: kgremban
ms.openlocfilehash: 649c7f620b83464d1bb56cf4b8191b0747105f01
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2019
ms.locfileid: "74457228"
---
# <a name="connect-modbus-tcp-devices-through-an-iot-edge-device-gateway"></a>Conexión de dispositivos que usan Modbus TCP a través de una puerta de enlace de dispositivos IoT Edge

Si desea conectar dispositivos de IoT que usan los protocolos Modbus TCP o RTU con Azure IoT Hub, use un dispositivo de IoT Edge como puerta de enlace. Este dispositivo de puerta de enlace permite leer los datos de los dispositivos de Modbus y, posteriormente, comunica esos datos a la nube mediante un protocolo compatible.

![Los dispositivos de Modbus se conectan con IoT Hub mediante una puerta de enlace de IoT Edge](./media/deploy-modbus-gateway/diagram.png)

En este artículo se explica cómo crear su propia imagen de contenedor para un módulo Modbus (o puede usar un ejemplo creado previamente), y cómo implementarlo después en el dispositivo de IoT Edge que actuará como puerta de enlace.

En este artículo se da por supuesto que va a usar el protocolo Modbus TCP. Para más información acerca de cómo configurar el módulo para que admita la RTU de Modbus, consulte el proyecto [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) (Módulo Modbus de Azure IoT Edge) en GitHub.

## <a name="prerequisites"></a>Requisitos previos
* Un dispositivo de Azure IoT Edge. Para ver un tutorial sobre cómo configurar uno, consulte [Implementación de Azure IoT Edge en Windows](quickstart.md) o [Linux](quickstart-linux.md).
* La cadena de conexión de clave principal para el dispositivo de IoT Edge.
* Un dispositivo de Modbus físico o simulado que admita Modbus TCP.

## <a name="prepare-a-modbus-container"></a>Preparación de un contenedor Modbus

Si desea probar la funcionalidad de puerta de enlace de Modbus, Microsoft tiene un módulo de ejemplo que puede usar. Puede acceder al módulo desde Azure Marketplace, [Modbus](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot.edge-modbus?tab=Overview), o con el URI de la imagen **mcr.microsoft.com/azureiotedge/modbus:1.0**.

Si desea crear su propio módulo y personalizarlo para su entorno, hay un proyecto de código abierto llamado [Azure IoT Edge Modbus module](https://github.com/Azure/iot-edge-modbus) (Módulo Modbus de Azure IoT Edge) en GitHub. Siga las instrucciones de ese proyecto para crear su propia imagen de contenedor. Para crear su propia imagen de contenedor, consulte [Desarrollar módulos de C# en Visual Studio](how-to-visual-studio-develop-csharp-module.md) o [Desarrollar módulos en Visual Studio Code](how-to-vs-code-develop-module.md). Esos artículos proporcionan instrucciones sobre cómo crear nuevos módulos y publicar imágenes de contenedor en un registro.

## <a name="try-the-solution"></a>Prueba de la solución

En esta sección se describe cómo implementar el módulo Modbus de ejemplo de Microsoft en el dispositivo IoT Edge.

1. En [Azure Portal](https://portal.azure.com/), vaya hasta la instancia de IoT Hub.

2. Vaya a **IoT Edge** y haga clic en el dispositivo IoT Edge.

3. Seleccione **Set modules** (Establecer módulos).

4. Agregue el módulo Modbus:

   1. Haga clic en **Add** (Agregar) y seleccione **IoT Edge Module** (Módulo de IoT Edge).

   2. En el campo **Nombre**, escriba "modbus".

   3. En el campo **Imagen**, escriba el URI de la imagen del contenedor de ejemplo: `mcr.microsoft.com/azureiotedge/modbus:1.0`.

   4. Marque la casilla **Habilitar** para actualizar las propiedades deseadas del módulo gemelo.

   5. Copie el siguiente JSON en el cuadro de texto. Cambie el valor de **SlaveConnection** a la dirección IPv4 del dispositivo Modbus.

      ```JSON
      {
        "properties.desired":{
          "PublishInterval":"2000",
          "SlaveConfigs":{
            "Slave01":{
              "SlaveConnection":"<IPV4 address>","HwId":"PowerMeter-0a:01:01:01:01:01",
              "Operations":{
                "Op01":{
                  "PollingInterval": "1000",
                  "UnitId":"1",
                  "StartAddress":"40001",
                  "Count":"2",
                  "DisplayName":"Voltage"
                }
              }
            }
          }
        }
      }
      ```

   6. Seleccione **Guardar**.

5. Vuelva al paso **Agregar módulos** y seleccione **Siguiente**.

7. En el paso **Specify Routes** (Especificar rutas), copie el siguiente archivo JSON en el cuadro de texto. Esta ruta envía todos los mensajes recopilados por el módulo Modbus a IoT Hub. En esta ruta, **modbusOutput** es el punto de conexión que usa el módulo Modbus para los datos de salida y **$upstream** es un destino especial que indica al centro de IoT Edge que envíe mensajes a IoT Hub.

   ```JSON
   {
     "routes": {
       "modbusToIoTHub":"FROM /messages/modules/modbus/outputs/modbusOutput INTO $upstream"
     }
   }
   ```

8. Seleccione **Next** (Siguiente).

9. En el paso **Review Deployment** (Revisar implementación), seleccione **Submit** (Enviar).

10. Vuelva a la página de detalles del dispositivo y seleccione **Actualizar**. Verá que el nuevo módulo **modbus** se ejecuta junto con el entorno de ejecución de IoT Edge.

## <a name="view-data"></a>Visualización de datos
Vea los datos que entran a través del módulo modbus:
```cmd/sh
iotedge logs modbus
```

Puede ver también la telemetría que el dispositivo envía mediante la [extensión de Azure IoT Hub Toolkit para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (anteriormente Azure IoT Toolkit).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre cómo pueden actuar los dispositivos IoT Edge como puertas de enlace, consulte [Creación de un dispositivo IoT Edge que actúa como una puerta de enlace transparente](./how-to-create-transparent-gateway.md).
- Para obtener más información sobre cómo funcionan los módulos de IoT Edge, consulte [Información de los módulos de Azure IoT Edge](iot-edge-modules.md).
