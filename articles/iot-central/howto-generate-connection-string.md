---
title: Generación de una cadena de conexión de dispositivos para Azure IoT Central | Microsoft Docs
description: Como desarrollador de dispositivos, ¿cómo puedo generar una cadena de conexión para un dispositivo que es necesario conectar a una aplicación de IoT Central?
author: dominicbetts
ms.author: dobett
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f302cbfa7152ae30be434f560c0c39056d40f9f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60885645"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Generación de una cadena de conexión de dispositivo para conectarse a una aplicación de Azure IoT Central

En este artículo se describe cómo, como desarrollador de dispositivos, generar una cadena de conexión para un dispositivo que es necesario conectar a una aplicación de IoT Central. El procedimiento descrito en este artículo muestra cómo conectar rápidamente un único dispositivo mediante una firma de acceso compartido (SAS). Este enfoque es útil cuando está experimentando con IoT Central o probado dispositivos. Para ver enfoques alternativos que se pueden usar en un entorno de producción, consulte [Conectividad de dispositivos en Azure IoT Central](concepts-connectivity.md).

## <a name="prerequisites"></a>Requisitos previos

Necesitará lo siguiente para completar los pasos de este artículo:

- Una aplicación de Azure IoT Central. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
- Una máquina de desarrollo que tenga instalado [Node.js](https://nodejs.org/) versión 8.0.0 o posterior. Puede ejecutar `node --version` en la línea de comandos para comprobar la versión. Node.js está disponible para una amplia variedad de sistemas operativos.

## <a name="get-connection-information"></a>Obtención de información sobre la conexión

En los pasos siguientes se describe cómo obtener la información que necesita para generar una cadena de conexión de SAS para un dispositivo:

1. En el **Explorador de dispositivos**, busque el dispositivo real que quiere conectar a la aplicación:

    ![Seleccionar un dispositivo real](media/howto-generate-connection-string/real-devices.png)

1. En la página **Dispositivo**, seleccione **Conectar**:

    ![Seleccionar Conectar](media/howto-generate-connection-string/connect.png)

1. Anote los detalles de la conexión, **Id. de ámbito**, **Id. de dispositivo** y **Device Primary key** (Clave principal de dispositivo); los necesitará en los pasos siguientes:

    ![Detalles de conexión](media/howto-generate-connection-string/device-connect.png)

    Puede copiar los valores de esta página para guardarlos.

## <a name="generate-the-connection-string"></a>Generación de la cadena de conexión

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha generado una cadena de conexión para un dispositivo real para conectar a la aplicación de Azure IoT Central, estos son los siguientes pasos sugeridos:

* [Preparación y conexión de un dispositivo DevKit (C)](howto-connect-devkit.md)
* [Preparación y conexión de un dispositivo Raspberry Pi (Python)](howto-connect-raspberry-pi-python.md)
* [Preparación y conexión de un dispositivo Raspberry Pi (C#)](howto-connect-raspberry-pi-csharp.md)
* [Preparación y conexión de un dispositivo Windows 10 IoT Core (C#)](howto-connect-windowsiotcore.md)
* [Conexión de un cliente de Node.js genérico a una aplicación de Azure IoT Central](howto-connect-nodejs.md)