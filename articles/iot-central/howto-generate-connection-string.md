---
title: Generar una cadena de conexión de dispositivo de Azure IoT Central | Microsoft Docs
description: Como desarrollador del dispositivo, ¿cómo puedo generar una cadena de conexión para el dispositivo que necesita para conectarse a una aplicación IoT Central?
author: dominicbetts
ms.author: dobett
ms.date: 04/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: f302cbfa7152ae30be434f560c0c39056d40f9f4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60885645"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Generar una cadena de conexión de dispositivo para conectarse a una aplicación de Azure IoT Central

Este artículo se describe cómo, como un desarrollador de dispositivos, para generar una cadena de conexión para el dispositivo que necesita para conectarse a una aplicación IoT Central. El procedimiento descrito en este artículo muestra cómo conectarse rápidamente a un único dispositivo con una firma de acceso compartido (SAS). Este enfoque es útil cuando está experimentando con IoT Central o dispositivos de prueba. Enfoques alternativos utilizar en un entorno de producción, consulte [conectividad de dispositivos en Azure IoT Central](concepts-connectivity.md).

## <a name="prerequisites"></a>Requisitos previos

Necesitará lo siguiente para completar los pasos de este artículo:

- Una aplicación de Azure IoT Central. Para más información, consulte la [guía de inicio rápido para crear una aplicación](quick-deploy-iot-central.md).
- Una máquina de desarrollo con [Node.js](https://nodejs.org/) versión 8.0.0 o posterior instalado. Puede ejecutar `node --version` en la línea de comandos para comprobar la versión. Node.js está disponible para una amplia variedad de sistemas operativos.

## <a name="get-connection-information"></a>Obtención de información sobre la conexión

Los pasos siguientes describen cómo obtener la información que necesita generar una cadena de conexión de SAS para un dispositivo:

1. En el **Device Explorer**, busque el dispositivo real que desea conectarse a la aplicación:

    ![Seleccione un dispositivo real](media/howto-generate-connection-string/real-devices.png)

1. En el **dispositivo** página, seleccione **Connect**:

    ![Seleccione Conectar](media/howto-generate-connection-string/connect.png)

1. Tome nota de los detalles de conexión, **Id. de ámbito**, **Id. de dispositivo**, y **clave principal de dispositivo**, para usar en los pasos siguientes:

    ![Detalles de conexión](media/howto-generate-connection-string/device-connect.png)

    Puede copiar los valores de esta página para guardar.

## <a name="generate-the-connection-string"></a>Generación de la cadena de conexión

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha generado una cadena de conexión para un dispositivo real para conectarse a la aplicación de Azure IoT Central, estos son los siguientes pasos sugeridos:

* [Preparación y conexión de un dispositivo DevKit (C)](howto-connect-devkit.md)
* [Preparación y conexión de un dispositivo Raspberry Pi (Python)](howto-connect-raspberry-pi-python.md)
* [Preparación y conexión de un dispositivo Raspberry Pi (C#)](howto-connect-raspberry-pi-csharp.md)
* [Preparación y conexión de un dispositivo Windows 10 IoT Core (C#)](howto-connect-windowsiotcore.md)
* [Conexión de un cliente de Node.js genérico a una aplicación de Azure IoT Central](howto-connect-nodejs.md)