---
title: Información de los SDK de IoT de Azure | Microsoft Docs
description: 'Guía del desarrollador: Información y vínculos a los diversos SDK de dispositivos y servicios IoT de Azure que puede usar para compilar aplicaciones de back-end y de aplicaciones de dispositivo.'
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dobett
ms.openlocfilehash: 9daca1876a291cb1f726e7c0eb4840c57f5c54c8
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741324"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Información y uso de los SDK de Azure IoT Hub

Hay tres categorías de kits de desarrollo de software (SDK) para trabajar con IoT Hub:

* Los **SDK de dispositivo** le permiten compilar aplicaciones que se ejecutan en los dispositivos IoT mediante clientes de dispositivo o módulo. Estas aplicaciones envían datos de telemetría a IoT Hub y, de manera opcional, reciben mensajes, trabajos, métodos o actualizaciones gemelas de dicho servicio.  También puede usar el cliente de módulo para crear [módulos](../iot-edge/iot-edge-modules.md) para el [entorno de ejecución de Azure IoT Edge](../iot-edge/about-iot-edge.md).

* Los **SDK de servicio** le permiten administrar su centro de IoT y, opcionalmente, enviar mensajes, programar trabajos, invocar métodos directos o enviar actualizaciones de propiedades deseada a los dispositivos o módulos IoT.

* Los **SDK de aprovisionamiento de dispositivos** le permiten aprovisionar dispositivos en IoT Hub mediante el [servicio Device Provisioning](../iot-dps/about-iot-dps.md).

Conozca las [ventajas de desarrollar con los SDK de Azure IoT](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>SDK de dispositivos IoT de Azure

Los SDK de dispositivos IoT de Microsoft Azure contienen código que facilita la creación dispositivos y aplicaciones que se conectan a servicios de Azure IoT Hub y que este administra.

SDK de dispositivos Azure IoT Hub para .NET: 

* Se instala desde [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-csharp)
* [Referencia de API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Referencia para los módulos](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

SDK de dispositivos Azure IoT Hub para C: escrito en ANSI C (C99) para la portabilidad y la compatibilidad con un gran número de plataformas:

* Se instala desde [apt get, MBED, Arduino IDE o NuGet](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-c)
* [Referencia de API](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)
* [Referencia para los módulos](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_module_client.h)

SDK de dispositivos Azure IoT Hub para Java: 

* Se agrega al proyecto [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-java)
* [Referencia de API](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Referencia para los módulos](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._module_client?view=azure-java-stable)

SDK de dispositivos Azure IoT Hub para Node.js: 

* Se instala desde [npm](https://www.npmjs.com/package/azure-iot-device)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-node)
* [Referencia de API](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [Referencia para los módulos](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

SDK de dispositivos Azure IoT Hub para Python: 

* Se instala desde [pip](https://pypi.python.org/pypi/azure-iothub-device-client/)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-python)
* Referencia de API: consulte [referencia de API de C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

SDK de dispositivos Azure IoT Hub para iOS: 

* Se instala desde [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient)
* [Muestras](https://github.com/Azure-Samples/azure-iot-samples-ios)
* Referencia de API: consulte [referencia de API de C](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/)

> [!NOTE]
> Consulte los archivos Léame en los repositorios de GitHub para obtener información sobre el uso de administradores de paquetes específicos de la plataforma y el lenguaje para instalar los archivos binarios y dependencias en el equipo de desarrollo.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Compatibilidad de hardware y de plataformas de sistema operativo

Para encontrar las plataformas admitidas para los SDK, consulte [Compatibilidad de plataformas de SDK de IoT de Azure](iot-hub-device-sdk-platform-support.md).

Para más información sobre la compatibilidad del SDK con dispositivos de hardware concretos, consulte el [catálogo de dispositivos Azure Certified for IoT](https://catalog.azureiotsuite.com/) o el repositorio individual.

## <a name="azure-iot-service-sdks"></a>SDK de servicios IoT de Azure

Los SDK de servicios IoT de Azure contienen código que facilitan la creación de aplicaciones que interactúan directamente con IoT Hub para administrar dispositivos y seguridad.

SDK de servicios Azure IoT Hub para .NET:

* Se descarga de [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices/)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-csharp)
* [Referencia de API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

SDK de servicios Azure IoT Hub para Java: 

* Se agrega al proyecto [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-java)
* [Referencia de API](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service)

SDK de servicios Azure IoT Hub para Node.js: 

* Se descarga de [npm](https://www.npmjs.com/package/azure-iothub)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-node)
* [Referencia de API](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

SDK de servicios Azure IoT Hub para Python: 

* Se descarga de [pip](https://pypi.python.org/pypi/azure-iothub-service-client/)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-python)

SDK de servicios Azure IoT Hub para C: 

* Se descarga de [apt get, MBED, Arduino IDE o NuGet](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Código fuente](https://github.com/Azure/azure-iot-sdk-c)

SDK de servicios de Azure IoT Hub para iOS: 

* Se instala desde [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient)
* [Muestras](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> Consulte los archivos Léame en los repositorios de GitHub para obtener información sobre el uso de administradores de paquetes específicos de la plataforma y el lenguaje para instalar los archivos binarios y dependencias en el equipo de desarrollo.

## <a name="device-provisioning-sdks"></a>SDK de aprovisionamiento de dispositivos

Los **SDK de aprovisionamiento de Microsoft Azure** le permiten aprovisionar dispositivos en IoT Hub mediante el [servicio Device Provisioning](../iot-dps/about-iot-dps.md).

SDK de aprovisionamiento de dispositivos y servicios de Azure para C#:

* [SDK de cliente de aprovisionamiento de dispositivos](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/device)
* [SDK de cliente de aprovisionamiento de servicios](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/service)

SDK de aprovisionamiento de dispositivos y servicios de Azure para Java:

* [SDK de cliente de aprovisionamiento de dispositivos](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-device-client)
* [SDK de cliente de aprovisionamiento de servicios](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-service-client)

SDK de aprovisionamiento de dispositivos y servicios de Azure para Node.js:

* [SDK de cliente de aprovisionamiento de dispositivos](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device)
* [SDK de cliente de aprovisionamiento de servicios](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service)

SDK de aprovisionamiento de dispositivos y servicios de Azure para Python:

* [SDK de cliente de aprovisionamiento de dispositivos](https://github.com/Azure/azure-iot-sdk-python/blob/master/provisioning_device_client)
* [SDK de cliente de aprovisionamiento de servicios](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client)

SDK de aprovisionamiento de dispositivos y servicios de Azure para C:

* [SDK de cliente de aprovisionamiento de dispositivos](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [SDK de cliente de aprovisionamiento de servicios](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_service_client)

## <a name="next-steps"></a>Pasos siguientes

Los SDK de Azure IoT también proporcionan un conjunto de herramientas para asistir en las tareas de desarrollo:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): una herramienta de línea de comandos multiplataforma para ayudar a diagnosticar problemas relacionados con la conexión con IoT Hub.
* [device-explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): una aplicación de escritorio de Windows para conectarse a IoT Hub.

Otros temas de referencia en la Guía del desarrollador de IoT Hub son:

* [Puntos de conexión de IoT Hub](iot-hub-devguide-endpoints.md)
* [Lenguaje de consulta de IoT Hub para dispositivos gemelos, trabajos y enrutamiento de mensajes](iot-hub-devguide-query-language.md)
* [Cuotas y limitación](iot-hub-devguide-quotas-throttling.md)
* [Compatibilidad con MQTT de IoT Hub](iot-hub-mqtt-support.md)
* [Referencia de la API REST de IoT Hub](/rest/api/iothub/)
* [Compatibilidad con plataformas de SDK de Azure IoT](iot-hub-device-sdk-platform-support.md)
