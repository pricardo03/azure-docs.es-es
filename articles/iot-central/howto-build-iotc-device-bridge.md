---
title: Creación del puente de dispositivos de Azure IoT Central | Microsoft Docs
description: Cree el puente de dispositivos de IoT Central para conectar otras nubes de IoT (Sigfox, Particle, The Things Network, etc.) a la aplicación IoT Central.
services: iot-central
ms.service: iot-central
author: viv-liu
ms.author: viviali
ms.date: 11/8/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 83f053a8815f31803f536920497fdc42e72d2a2d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628814"
---
# <a name="build-the-iot-central-device-bridge-to-connect-other-iot-clouds-to-iot-central"></a>Creación del puente de dispositivos de IoT Central para conectar otras nubes de IoT a IoT Central

*Este tema se aplica a administradores*.

El puente de dispositivos de IoT Central es una solución de código abierto que se conecta su nube de Sigfox, Particle, The Things Network y otras nubes a la aplicación IoT Central. Si usa dispositivos de seguimiento de activos, conectados a la red de área amplia de baja potencia de Sigfox, o usa dispositivos de control de la calidad del aire en Device Cloud de Particle, o usa dispositivos de supervisión de la humedad de suelo de TTN, puede aprovechar directamente el potencial de IoT Central mediante el puente de dispositivos de IoT Central. El puente de dispositivos conecta otras nubes de IoT con IoT Central mediante el reenvío de los datos que los dispositivos envían a las demás nubes a través a la aplicación IoT Central. En la aplicación IoT Central, puede crear reglas y ejecutar análisis en esos datos, crear flujos de trabajo en aplicaciones de Microsoft Flow y Azure Logic, exportar los datos y mucho más. Obtenga el [puente de dispositivo IoT Central](https://aka.ms/iotcentralgithubdevicebridge) desde Github.

## <a name="what-is-it-and-how-does-it-work"></a>¿Qué es y cómo funciona?
El puente de dispositivos de IoT Central es una solución de código abierto en Github. Funciona con un botón "Implementar en Azure" que implementa una plantilla de Azure Resource Manager personalizada con varios recursos de Azure en su suscripción de Azure. Los recursos incluyen:
-   Aplicación de función de Azure
-   Cuenta de Azure Storage
-   Plan de App Service (nivel S1)
-   Azure Key Vault La aplicación de función es la pieza fundamental del puente de dispositivos. Recibe las solicitudes HTTP POST desde otras plataformas de IoT o desde cualquier plataforma personalizada a través de una integración de webhook sencilla. Se incluyen ejemplos que muestran cómo conectarse a las nubes de Sigfox Particle y TTN. Puede ampliar fácilmente esta solución para conectarse a su nube de IoT personalizada si su plataforma puede enviar solicitudes HTTP POST a la aplicación de función.
La aplicación de función transforma los datos en un formato aceptado por IoT Central y los reenvía a través de las API de DPS.

![Captura de pantalla de Azure Functions](media/howto-build-iotc-device-bridge/azfunctions.png)

Si la aplicación de IoT Central reconoce el dispositivo por identificador de dispositivo en el mensaje reenviado, aparecerá una nueva medición para dicho dispositivo. Si la aplicación de IoT Central nunca ha visto el identificador de dispositivo, la aplicación de función intentará registrar un dispositivo nuevo con ese identificador de dispositivo, y aparecerá como "dispositivo no asociado" en la aplicación de IoT Central. 

## <a name="how-do-i-set-it-up"></a>¿Cómo se configura?
Las instrucciones se muestran en detalle en el archivo README en el repositorio de Github. 

## <a name="pricing"></a>Precios
Todo se hospeda en su suscripción de Azure. La mayoría de los costos estimados de los recursos aprovisionados procede del [precio de un Plan de servicio de aplicación estándar]( https://azure.microsoft.com/en-us/pricing/details/app-service/windows/). Puede aprender más sobre esto y las posibles formas de reducirlo en el archivo README.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a crear un puente de dispositivos de IoT Central, este es el paso siguiente sugerido:

> [!div class="nextstepaction"]
> [Administración de dispositivos](howto-manage-devices.md)