---
title: Desarrollo de módulos para Azure IoT Edge | Microsoft Docs
description: Desarrollo de módulos personalizados para Azure IoT Edge que pueden comunicarse con el tiempo de ejecución y IoT Hub
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 50e9c0667761e43c63b03fbaf5f8ce93eb49e749
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100096"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>Descripción de los requisitos y las herramientas para desarrollar módulos de IoT Edge

En este artículo se explica qué funcionalidades están disponibles al escribir aplicaciones que se ejecutan como un módulo de IoT Edge y cómo sacar provecho de ellas.

## <a name="iot-edge-runtime-environment"></a>Entorno de tiempo de ejecución de IoT Edge
El entorno de tiempo de ejecución de IoT Edge proporciona la infraestructura para integrar la funcionalidad de varios módulos de IoT Edge e implementarlos en dispositivos IoT Edge. En un nivel alto, todos los programas se pueden empaquetar como un módulo de IoT Edge. Sin embargo, para aprovechar al máximo las funcionalidades de comunicación y administración de IoT Edge, se puede conectar un programa que se ejecute en un módulo al centro local de IoT Edge, integrado en el entorno de tiempo de ejecución de IoT Edge.

## <a name="using-the-iot-edge-hub"></a>Uso del centro de IoT Edge
El centro de IoT Edge proporciona dos funcionalidades principales: proxy a IoT Hub y comunicaciones locales.

### <a name="iot-hub-primitives"></a>Primitivos de IoT Hub
IoT Hub considera una instancia de módulo de forma análoga a un dispositivo, en el sentido de que:

* tiene un módulo gemelo, que es distinto y está aislado del [dispositivo gemelo](../iot-hub/iot-hub-devguide-device-twins.md) y de los otros módulos gemelos de ese dispositivo;
* puede enviar [mensajes del dispositivo a la nube](../iot-hub/iot-hub-devguide-messaging.md);
* puede recibir [métodos directos](../iot-hub/iot-hub-devguide-direct-methods.md) dirigidos específicamente a su identidad.

Actualmente, un módulo no puede recibir mensajes de la nube al dispositivo ni usar la característica de carga de archivos.

Al escribir un módulo, puede usar el [SDK de dispositivo IoT de Azure](../iot-hub/iot-hub-devguide-sdks.md) para conectarse al centro de IoT Edge y utilizar la funcionalidad anterior igual que lo haría al usar IoT Hub con una aplicación de dispositivo; la única diferencia es que, desde el back-end de la aplicación, tiene que hacer referencia a la identidad del módulo en lugar de a la identidad del dispositivo.

Consulte [Desarrollo e implementación de un módulo de IoT Edge en un dispositivo simulado](tutorial-csharp-module.md) para obtener un ejemplo de una aplicación de módulo que envía mensajes del dispositivo a la nube y usa el módulo gemelo.

### <a name="device-to-cloud-messages"></a>Mensajes de dispositivo a nube
Para permitir el procesamiento complejo de mensajes de dispositivo a nube, el centro de IoT Edge proporciona enrutamiento declarativo de mensajes entre los módulos, y entre los módulos e IoT Hub. Este enrutamiento declarativo permite a los módulos interceptar y procesar los mensajes enviados por otros módulos y propagarlos en canalizaciones complejas. En el artículo [Composición de los módulos](module-composition.md) se explica cómo organizar los módulos en canalizaciones complejas mediante rutas.

Un módulo de IoT Edge, a diferencia de una aplicación normal de dispositivo IoT Hub, puede recibir mensajes de dispositivo a nube que se redirigen mediante proxy a través de su centro de IoT Edge local, con el fin de procesarlos.

El centro de IoT Edge propaga los mensajes al módulo en función de las rutas declarativas que se describen en el artículo [Composición de los módulos](module-composition.md). Al desarrollar un módulo IoT Edge, puede recibir estos mensajes mediante el establecimiento de los controladores de mensajes.

Para simplificar la creación de rutas, IoT Edge agrega el concepto de puntos de conexión de *entrada* y *salida* del módulo. Un módulo puede recibir todos los mensajes de dispositivo a nube enrutados a él sin especificar ninguna entrada y puede enviar mensajes de dispositivo a nube sin especificar ninguna salida.
Sin embargo, el uso explícito de entradas y salidas facilita la comprensión de las reglas de enrutamiento. Consulte [Composición de los módulos](module-composition.md) para obtener más información sobre las reglas de enrutamiento y los puntos de conexión de entrada y salida de los módulos.

Por último, los mensajes de dispositivo a nube que se gestionan mediante el centro de Edge están marcados con las siguientes propiedades del sistema:

| Propiedad | DESCRIPCIÓN |
| -------- | ----------- |
| $connectionDeviceId | El identificador de dispositivo del cliente que envió el mensaje. |
| $connectionModuleId | El identificador de módulo del módulo que envió el mensaje. |
| $inputName | La entrada que recibió este mensaje. Puede estar vacía. |
| $outputName | La salida usada para enviar el mensaje. Puede estar vacía. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Conexión al centro de IoT Edge desde un módulo
La conexión al centro de IoT Edge local desde un módulo implica dos pasos: 
1. Use la cadena de conexión proporcionada por el tiempo de ejecución de IoT Edge cuando se inicia el módulo.
2. Asegúrese de que la aplicación acepta el certificado presentado por el centro de IoT Edge en ese dispositivo.

La cadena de conexión que se va a usar se proporciona mediante el entorno de tiempo de ejecución de IoT Edge en la variable de entorno `EdgeHubConnectionString`. De esta forma, está disponible para cualquier programa que quiera usarla.

De igual forma, el certificado usado para validar la conexión del centro de IoT Edge se proporciona en el entorno de tiempo de ejecución de IoT Edge en un archivo cuya ruta de acceso está disponible en la variable de entorno `EdgeModuleCACertificateFile`.

## <a name="next-steps"></a>Pasos siguientes

Después de desarrollar un módulo, aprenda a [implementar y supervisar los módulos de IoT Edge a escala](how-to-deploy-monitor.md).

