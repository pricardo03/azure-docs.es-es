---
title: 'Obtenga información sobre cómo los módulos ejecutan la lógica en los dispositivos: Azure IoT Edge | Microsoft Docs'
description: Los módulos de Azure IoT Edge son unidades de lógica en contenedores que se pueden implementar y administrar de forma remota para que pueda ejecutar la lógica de negocios en dispositivos IoT Edge
author: kgremban
manager: philmea
ms.author: v-yiso
origin.date: 03/21/2019
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: d1e2e35dafd90c16e9d0dbf38afb1e981653d1fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60445050"
---
# <a name="understand-azure-iot-edge-modules"></a>Información sobre los módulos de Azure IoT Edge

Azure IoT Edge permite implementar y administrar lógica de negocios en el perímetro a través de *módulos*. Los módulos de Azure IoT Edge son la unidad más pequeña de cálculo que administra IoT Edge y pueden contener servicios de Azure (por ejemplo, Azure Stream Analytics) o su propio código específico de la solución. Para entender cómo se desarrolló, implementa y mantiene, resulta útil para pensar en los cuatro elementos conceptuales de un módulo de módulos:

* Una **imagen de módulo** es un paquete que contiene el software que define un módulo.
* Una **instancia de módulo** es la unidad específica del cálculo que ejecuta la imagen de módulo en un dispositivo IoT Edge. La instancia de módulo se inicia mediante la instancia de IoT Edge en tiempo de ejecución.
* Una **identidad de módulo** es un fragmento de información (incluidas las credenciales de seguridad) almacenada en IoT Hub que está asociada a cada instancia de módulo.
* Un **módulo gemelo** es un documento JSON almacenado en IoT Hub, que contiene información de los estados de una instancia de módulo, incluidos los metadatos, las configuraciones y las condiciones. 

## <a name="module-images-and-instances"></a>Instancias e imágenes de módulo

Las imágenes de módulo de IoT Edge contienen las aplicaciones que aprovechan las características de administración, seguridad y comunicación de la instancia de IoT Edge en tiempo de ejecución. Puede desarrollar sus propias imágenes de módulo o exportar una desde un servicio de Azure compatible, como Azure Stream Analytics.
Las imágenes se encuentran en la nube y pueden actualizarse, cambiarse e implementarse en diferentes soluciones. Por ejemplo, un módulo que utiliza el aprendizaje automático para predecir la salida de la línea de producción actúa como una imagen independiente de un módulo que utiliza Computer Vision para controlar un dron. 

Cada vez que una imagen de módulo se implementa en un dispositivo y se inicia mediante la instancia de IoT Edge en tiempo de ejecución, se crea una instancia de ese módulo. Dos dispositivos en distintas partes del mundo podrían usar la misma imagen de módulo. Sin embargo, cada dispositivo tendría su propia instancia de módulo cuando este se inicie en el dispositivo. 

![Diagrama: imágenes de módulo en la nube e instancias de módulo en dispositivos](./media/iot-edge-modules/image_instance.png)

En la implementación, las imágenes de módulos actúan como imágenes de contenedor en un repositorio y las instancias de módulo son contenedores en dispositivos. 

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Identidades de módulo

Cuando la instancia de IoT Edge en tiempo de ejecución crea una nueva instancia de módulo, se asocia con la identidad de módulo correspondiente. La identidad de módulo se almacena en IoT Hub y se emplea como el ámbito seguridad y direccionamiento en todas las comunicaciones locales y en la nube correspondientes a esa instancia de módulo.
La identidad asociada con una instancia de módulo depende de la identidad del dispositivo en que se ejecuta la instancia y del nombre que proporcione para ese módulo en la solución. Por ejemplo, si llama `insight` a un módulo que utiliza Azure Stream Analytics y lo implementa en un dispositivo denominado `Hannover01`, la instancia de IoT Edge en tiempo de ejecución crea una identidad de módulo correspondiente llamada `/devices/Hannover01/modules/insight`.

Obviamente, en escenarios cuando necesite implementar una imagen de módulo varias veces en el mismo dispositivo, puede implementar la misma imagen varias veces con nombres diferentes.

![Diagrama: las identidades de módulo son únicas en cada dispositivos y en todos los dispositivos](./media/iot-edge-modules/identity.png)

## <a name="module-twins"></a>Módulos gemelos

Cada instancia de módulo también tiene un módulo gemelo correspondiente que puede usar para configurar la instancia de módulo. La instancia y el módulo gemelo se asocian entre sí a través de la identidad de módulo. 

Un módulo gemelo es un documento JSON que almacena las propiedades de configuración e información de los módulos. Este concepto se asemeja al del [dispositivo gemelo](../iot-hub/iot-hub-devguide-device-twins.md) de IoT Hub. La estructura de un módulo gemelo es exactamente igual que la de un dispositivo gemelo. La API que se usa para interactuar con los dos tipos de gemelos también es la misma. La única diferencia entre ambas es la identidad utilizada para crear una instancia del SDK del cliente. 

```csharp
// Create a ModuleClient object. This ModuleClient will act on behalf of a 
// module since it is created with a module’s connection string instead 
// of a device connection string. 
ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(settings); 
await client.OpenAsync(); 
 
// Get the module twin 
Twin twin = await client.GetTwinAsync(); 
```

## <a name="offline-capabilities"></a>Funcionalidades sin conexión

Azure IoT Edge permite realizar operaciones sin conexión en los dispositivos IoT Edge. Estas funcionalidades están limitadas por ahora. Hay capacidades sin conexión adicionales disponibles en la versión preliminar pública. Para obtener más información, consulte [Understand extended offline capabilities for IoT Edge devices, modules, and child devices](offline-capabilities.md) (Entender las capacidades sin conexión extendidas para dispositivos IoT Edge, módulos y dispositivos secundarios).

Los módulos de IoT Edge pueden permanecer sin conexión durante largos períodos de tiempo siempre que se cumplan los requisitos siguientes: 

* **Que no haya expirado el período de vida (TTL)**. El valor predeterminado para el TTL de un mensaje es de dos horas, pero puede aumentarse o reducirse en Microsoft Azure Store y reenviar la configuración del centro de IoT Edge. 
* **Que los módulos no tengan que volver a autenticarse con el centro de IoT Edge cuando estén sin conexión**. Los módulos solo pueden autenticarse con centros de IoT Edge que tengan una conexión activa con un centro de IoT. Los módulos tienen que volver a autenticarse si se reinician por algún motivo. Los módulos pueden seguir enviando mensajes al centro de IoT Edge aunque el token de SAS haya expirado. Cuando se reanuda la conectividad, el centro de IoT Edge solicita un nuevo token desde el módulo y lo valida con el centro de IoT. Si es correcto, el centro de IoT Edge reenvía los mensajes del módulo que están guardados, incluso lo que se enviaron mientras el token del módulo estaba vencido. 
* **Que el módulo que envía los mensajes mientras no hay conexión siga en funcionamiento cuando se reanude la conectividad**. Cuando se restablece la conexión con IoT Hub, el centro de IoT Edge tiene que validar un nuevo token del módulo (si el anterior venció) para poder reenviar los mensajes del módulo. Si el módulo no es capaz de proporcionar un nuevo token, el centro de IoT Edge no puede realizar ninguna operación con los mensajes almacenados del módulo. 
* **Que el centro de IoT Edge tenga espacio en disco para almacenar los mensajes**. De forma predeterminada, los mensajes se guardan en el sistema de archivos del contenedor del centro de IoT Edge. No obstante, hay una opción de configuración que permite especificar un volumen montado para guardar los mensajes. En cualquier caso, debe haber espacio en disco suficiente para guardar los mensajes que se van a entregar en diferido a IoT Hub.  


## <a name="next-steps"></a>Pasos siguientes
 - [Descripción de los requisitos y las herramientas para desarrollar módulos de IoT Edge](module-development.md)
 - [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md)

<!-- Images -->
[1]: ./media/iot-edge-modules/image_instance.png
[2]: ./media/iot-edge-modules/identity.png

<!-- Links -->
[lnk-device-identity]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-runtime]: iot-edge-runtime.md
[lnk-mod-dev]: module-development.md
