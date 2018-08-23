---
title: Información de los módulos de Azure IoT Edge | Microsoft Docs
description: Información sobre los módulos de Azure IoT Edge y cómo se configuran
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 02/15/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9064e0da6dde6c4b30235adf771f06a4f25d709a
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2018
ms.locfileid: "42144531"
---
# <a name="understand-azure-iot-edge-modules"></a>Información sobre los módulos de Azure IoT Edge

Azure IoT Edge permite implementar y administrar lógica de negocios en el perímetro a través de *módulos*. Los módulos de Azure IoT Edge son la unidad más pequeña de cálculo que administra IoT Edge y pueden contener servicios de Azure (por ejemplo, Azure Stream Analytics) o su propio código específico de la solución. Para entender cómo se desarrolla, implementa y mantiene un módulo, imagínese que está compuesto de cuatro partes conceptuales:

* Una **imagen de módulo** es un paquete que contiene el software que define un módulo.
* Una **instancia de módulo** es la unidad específica del cálculo que ejecuta la imagen de módulo en un dispositivo IoT Edge. La instancia de módulo se inicia mediante la instancia de IoT Edge en tiempo de ejecución.
* Una **identidad de módulo** es un fragmento de información (incluidas las credenciales de seguridad) almacenada en IoT Hub que está asociada a cada instancia de módulo.
* Un **módulo gemelo** es un documento JSON almacenado en IoT Hub, que contiene información de los estados de una instancia de módulo, incluidos los metadatos, las configuraciones y las condiciones. 

## <a name="module-images-and-instances"></a>Instancias e imágenes de módulo

Las imágenes de módulo de IoT Edge contienen las aplicaciones que aprovechan las características de administración, seguridad y comunicación de la instancia de IoT Edge en tiempo de ejecución. Puede desarrollar sus propias imágenes de módulo o exportar una desde un servicio de Azure compatible, como Azure Stream Analytics.
Las imágenes se encuentran en la nube y pueden actualizarse, cambiarse e implementarse en diferentes soluciones. Por ejemplo, un módulo que utiliza el aprendizaje automático para predecir la salida de la línea de producción actúa como una imagen independiente de un módulo que utiliza Computer Vision para controlar un dron. 

Cada vez que una imagen de módulo se implementa en un dispositivo y se inicia mediante la instancia de IoT Edge en tiempo de ejecución, se crea una instancia de ese módulo. Dos dispositivos en distintas partes del mundo pudieron utilizar la misma imagen de módulo; sin embargo, cada uno tendría su propia instancia de módulo cuando el módulo se inició en el dispositivo. 

![Imágenes de módulo en la nube e instancias de módulo en dispositivos][1]

En la implementación, las imágenes de módulos actúan como imágenes de contenedor en un repositorio y las instancias de módulo son contenedores en dispositivos. 

<!--
As use cases for Azure IoT Edge grow, new types of module images and instances will be created. For example, resource constrained devices cannot run containers so may require module images that exist as dynamic link libraries and instances that are executables. 
-->

## <a name="module-identities"></a>Identidades de módulo

Cuando la instancia de IoT Edge en tiempo de ejecución crea una nueva instancia de módulo, se asocia con la identidad de módulo correspondiente. La identidad de módulo se almacena en IoT Hub y se emplea como el ámbito seguridad y direccionamiento en todas las comunicaciones locales y en la nube correspondientes a esa instancia de módulo.
La identidad asociada con una instancia de módulo depende de la identidad del dispositivo en que se ejecuta la instancia y del nombre que proporcione para ese módulo en la solución. Por ejemplo, si llama `insight` a un módulo que utiliza Azure Stream Analytics y lo implementa en un dispositivo denominado `Hannover01`, la instancia de IoT Edge en tiempo de ejecución crea una identidad de módulo correspondiente llamada `/devices/Hannover01/modules/insight`.

Obviamente, en escenarios cuando necesite implementar una imagen de módulo varias veces en el mismo dispositivo, puede implementar la misma imagen varias veces con nombres diferentes.

![Las identidades de módulo son únicas][2]

## <a name="module-twins"></a>Módulos gemelos

Cada instancia de módulo también tiene un módulo gemelo correspondiente que puede usar para configurar la instancia de módulo. La instancia y el módulo gemelo se asocian entre sí a través de la identidad de módulo. 

Un módulo gemelo es un documento JSON que almacena las propiedades de configuración e información de los módulos. Este concepto se asemeja al del [dispositivo gemelo][lnk-device-twin] de IoT Hub. La estructura de un módulo gemelo es exactamente igual que la de un dispositivo gemelo. La API que se usa para interactuar con los dos tipos de gemelos también es la misma. La única diferencia entre ambas es la identidad utilizada para crear una instancia del SDK del cliente. 

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

Azure IoT Edge permite realizar operaciones sin conexión en los dispositivos IoT Edge. Por el momento, estas funcionalidades están limitadas, pero se irán desarrollando nuevos escenarios. 

Los módulos de IoT Edge pueden permanecer sin conexión durante largos períodos de tiempo siempre que se cumplan los requisitos siguientes: 

* **Que no haya expirado el período de vida (TTL)**. El valor predeterminado para el TTL de un mensaje es de dos horas, pero puede aumentarse o reducirse en Microsoft Azure Store y reenviar la configuración del centro de IoT Edge. 
* **Que los módulos no tengan que volver a autenticarse con el centro de IoT Edge cuando estén sin conexión**. Los módulos solo pueden autenticarse con centros de Edge que tengan una conexión activa con un centro de IoT. Los módulos tienen que volver a autenticarse si se reinician por algún motivo. Los módulos pueden seguir enviando mensajes al centro de Edge aunque el token de SAS haya expirado. Cuando se reanuda la conectividad, el centro de Edge solicita un nuevo token desde el módulo y lo valida con el centro de IoT. Si es correcto, el centro de Edge reenvía los mensajes del módulo que están guardados, incluso lo que se enviaron mientras el token del módulo estaba vencido. 
* **Que el módulo que envía los mensajes mientras no hay conexión siga en funcionamiento cuando se reanude la conectividad**. Cuando se restablece la conexión con IoT Hub, el centro de Edge tiene que validar un nuevo token del módulo (si el anterior venció) para poder reenviar los mensajes del módulo. Si el módulo no es capaz de proporcionar un nuevo token, el centro de Edge no puede realizar ninguna operación con los mensajes almacenados del módulo. 
* **Que el centro de Edge tenga espacio en disco para almacenar los mensajes**. De forma predeterminada, los mensajes se guardan en el sistema de archivos del contenedor del centro de Edge. No obstante, hay una opción de configuración que permite especificar un volumen montado para guardar los mensajes. En cualquier caso, debe haber espacio en disco suficiente para guardar los mensajes que se van a entregar en diferido a IoT Hub.  

## <a name="next-steps"></a>Pasos siguientes
 - [Información de la instancia de Azure IoT Edge en tiempo de ejecución y su arquitectura][lnk-runtime]

<!-- Images -->
[1]: ./media/iot-edge-modules/image_instance.png
[2]: ./media/iot-edge-modules/identity.png

<!-- Links -->
[lnk-device-identity]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
[lnk-runtime]: iot-edge-runtime.md
