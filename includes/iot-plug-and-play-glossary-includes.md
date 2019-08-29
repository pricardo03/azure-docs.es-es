---
author: ChrisGMsft
ms.service: iot-pnp
ms.topic: include
ms.date: 06/28/2019
ms.author: chrisgre
ms.openlocfilehash: 7e8174855800bc6beea8750c32a6dd32588ccd9e
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69879204"
---
## <a name="iot-plug-and-play-device"></a>Dispositivo IoT Plug and Play

Un dispositivo IoT Plug and Play suele ser un dispositivo informático independiente y a pequeña escala que puede recopilar datos o controlar otros dispositivos, y que ejecuta software o firmware que implementa las funcionalidades declaradas en un [modelo de funcionalidad del dispositivo](#device-capability-model).  Por ejemplo, un dispositivo IoT Plug and Play podría ser un dispositivo de supervisión ambiental o un controlador de un sistema de riego de agricultura inteligente. Se pueden escribir soluciones de IoT hospedadas en la nube para imponer, controlar y recibir datos de dispositivos IoT Plug and Play. Encontrará los dispositivos IoT Plug and Play en el [catálogo de dispositivos Azure Certified for IoT](https://catalog.azureiotsolutions.com/). Todos los dispositivos IoT Plug and Play del catálogo están validados y tienen un [modelo de funcionalidad del dispositivo](#device-capability-model).

## <a name="digital-twin"></a>Gemelo digital

Los gemelos digitales son modelos de dispositivos IoT Plug and Play.  Los gemelos digitales se modelan mediante el [lenguaje de definición de gemelos digitales (DTDL)](https://aka.ms/DTDL).  Puede usar la API de Azure IoT para interactuar con los gemelos digitales. 

## <a name="digital-twin-definition-language"></a>Lenguaje de definición de gemelos digitales

Lenguaje que sirve para describir los modelos y las interfaces de los [dispositivos IoT Plug and Play](#iot-plug-and-play-device).  Use el [lenguaje de definición de gemelos digitales](https://aka.ms/DTDL) para describir las funcionalidades de un [gemelo digital](#digital-twin) y para habilitar la plataforma IoT y las soluciones IoT con el fin de aprovechar la semántica de la entidad.

## <a name="device-capability-model"></a>Modelo de funcionalidad del dispositivo

Un modelo de funcionalidad del dispositivo describe un dispositivo y define el conjunto de interfaces implementadas por este. Cree un modelo de funcionalidad del dispositivo que se corresponda con un dispositivo físico, un producto o una SKU.

## <a name="interface"></a>Interfaz

Una interfaz describe las funcionalidades relacionadas que un dispositivo o un gemelo digital implementa. Las interfaces se pueden reutilizar en otros modelos de funcionalidad.

## <a name="property"></a>Propiedad

Las propiedades son campos de datos definidos en una [interfaz](#interface) que representan un estado de un gemelo digital. Las propiedades se pueden declarar como de solo lectura o de escritura. Las propiedades de solo lectura se establecen mediante código que se ejecuta en el contexto del propio dispositivo IoT Plug and Play (como un número de serie),  mientras que las de escritura las establecen entidades externas como, por ejemplo, los umbrales de alarma.

## <a name="telemetry"></a>Telemetría

Los campos de telemetría definidos en una [interfaz](#interface) representan medidas. Estas medidas suelen ser valores, como lecturas de un sensor.

## <a name="command"></a>Get-Help

Los comandos definidos en una [interfaz](#interface) representan los métodos que se pueden ejecutar en el gemelo digital. Así, por ejemplo, un comando de restablecimiento para restablecer un dispositivo.
