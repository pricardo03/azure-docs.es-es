---
title: 'Azure IoT Hub: empezar a conectar dispositivos de IoT con la nube | Microsoft Docs'
description: Aprenda a conectar paneles y kits de inicio de IoT con Azure IoT Hub. Los dispositivos pueden enviar datos de telemetría a IoT Hub y este servicio supervisa y administra los dispositivos.
author: dominicbetts
manager: timlt
keywords: tutorial de azure iot hub
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 76f427204f0ad31196ce8b995b9e4ed9676209ca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634709"
---
# <a name="azure-iot-hub-get-started-with-real-devices"></a>Introducción a Azure IoT Hub con dispositivos reales

Puede usar Azure IoT Hub y los SDK de dispositivo IoT de Azure para crear soluciones de Internet de las cosas (IoT):

* Azure IoT Hub es un servicio completamente administrado en la nube que conecta, supervisa y administra los dispositivos IoT de forma segura. Use los SDK de dispositivo IoT de Azure para implementar los dispositivos de IoT.
* Use una puerta de enlace de IoT en escenarios IoT más complejos. Por ejemplo, donde sea necesario tener en cuenta factores como dispositivos antiguos, costos de ancho de banda, directivas de seguridad y privacidad o procesamiento de datos perimetrales. En estos escenarios, se usa [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) para implementar una puerta de enlace que conecta dispositivos con IoT Hub.

## <a name="what-the-how-to-articles-cover"></a>Qué se trata en los artículos de procedimientos

Estos artículos le presentan Azure IoT Hub y los SDK de dispositivo. Los artículos cubren escenarios comunes de IoT para demostrar las funcionalidades de IoT Hub. Los artículos también muestran cómo combinar IoT Hub con otras herramientas y servicios de Azure para crear soluciones de IoT más eficaces. En los artículos, se usan dispositivos IoT reales.

## <a name="set-up-your-device"></a>Configuración del dispositivo

Conecte un dispositivo IoT o una puerta de enlace a Azure IoT Hub:

| Dispositivo IoT                       | Lenguaje de programación |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino en VSCode][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Adafruit Feather HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev       | [Arduino][Th_Ard]              |
| Adafruit Feather M0              | [Arduino][M0_Ard]              |
| Simulador de dispositivos en línea         | [Raspberry Pi (Node.js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
