---
title: Administración de la conectividad y mensajería confiable mediante los SDK de dispositivo de Azure IoT Hub
description: Obtenga información sobre cómo mejorar la conectividad de dispositivos y la mensajería al usar los SDK de dispositivo de Azure IoT Hub
services: iot-hub
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 07/07/2018
ms.topic: article
ms.service: iot-hub
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 13e9eff9f7a9298db64b40d0fefcd7f1498137da
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44052863"
---
# <a name="how-to-manage-connectivity-and-reliable-messaging-using-azure-iot-hub-device-sdks"></a>Administración de la conectividad y mensajería confiable mediante los SDK de dispositivo de Azure IoT Hub

En esta guía se proporcionan instrucciones de alto nivel para diseñar aplicaciones de dispositivos resistentes, aprovechando las ventajas de las características de conectividad y mensajería confiable de los SDK de dispositivo IoT de Azure. El objetivo de este artículo es ayudarle a responder preguntas y a controlar estos escenarios:

- Administración de una conexión de red descartada
- Administración del cambio entre distintas conexiones de red
- Administración de la reconexión debido a errores de conexión transitorios del servicio

Los detalles de la implementación pueden variar según el lenguaje. Para más detalles, consulte la documentación de API vinculada o el SDK específico.

- [SDK de C/Python/iOS](https://github.com/azure/azure-iot-sdk-c)
- [SDK de .NET](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [SDK de Java](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [SDK de Node](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)


## <a name="designing-for-resiliency"></a>Diseño para lograr resistencia

A menudo, los dispositivos IoT se basan en conexiones de red no continuas o inestables, como GSM o conexiones por satélite. Además, al interactuar con servicios basados en la nube, pueden producirse errores debido a condiciones temporales como una disponibilidad intermitente del servicio y errores a nivel de infraestructura (los que comúnmente se conocen como errores transitorios). Una aplicación que se ejecuta en un dispositivo debe administrar los mecanismos de conexión y reconexión, así como la lógica de reintento para enviar o recibir mensajes. Además, los requisitos de la estrategia de reintento dependen en gran medida del escenario de IoT en que participa el dispositivo y del contexto y las funcionalidades del dispositivo.

Los SDK de dispositivo de Azure IoT Hub apuntan a simplificar la conexión y la comunicación de la nube al dispositivo y del dispositivo a la nube al proporcionar una manera sólida e integral de conectarse y de enviar o recibir mensajes desde y hacia Azure IoT Hub. Los desarrolladores también pueden modificar la implementación existente para desarrollar la estrategia de reintento adecuada para un escenario determinado.

Las características pertinentes del SDK que admiten la conectividad y la mensajería de confianza se tratan en las secciones siguientes.

## <a name="connection-and-retry"></a>Conexión y reintento

En esta sección se proporciona información general sobre los patrones de reconexión y reintento disponibles cuando se administran las conexiones, instrucciones de implementación para usar una directiva de reintentos diferente en la aplicación de dispositivo y las API pertinentes para los SDK de dispositivo.

### <a name="error-patterns"></a>Patrones de error
Los errores de conexión pueden producirse en muchos niveles:

-  Errores de red como un socket desconectado y errores de resolución de nombres
- Errores a nivel de protocolo para transporte HTTP, AMQP y MQTT, como vínculos desasociados o sesiones expiradas
- Errores a nivel de aplicación que se generan a partir de errores locales, como credenciales no válidas o un comportamiento de servicio, como exceder una cuota o una limitación

Los SDK de dispositivo detectan errores en los tres niveles.  Los errores relacionados con el sistema operativo y los errores de hardware no se detectan ni se controlan mediante los SDK de dispositivo.  El diseño se basa en [la guía para controlar errores transitorios](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) de Azure Architecture Center.

### <a name="retry-patterns"></a>Patrones de reintentos

El proceso general de reintento cuando se detectan errores de conexión es el siguiente: 
1. El SDK detecta el error y el error asociado en la red, el protocolo o la aplicación.
2. Según el tipo de error, el SDK usa el filtro de error para decidir si es necesario realizar el reintento.  Si el SDK identifica un **error irrecuperable**, las operaciones (conexión y envío/recepción) se detendrán y el SDK enviará una notificación al usuario. Un error irrecuperable es un error que el SDK puede identificar y determinar que no se puede recuperar, por ejemplo, un error de punto de conexión incorrecto o de autenticación.
3. Si se identifica un **error recuperable**, el SDK empieza a reintentar usar la directiva de reintentos especificada hasta que expira el tiempo de espera definido.
4. Cuando el tiempo de espera definido expira, el SDK deja de tratar de conectarse o de enviar y notifica al usuario.
5.  El SDK permite que el usuario adjunte una devolución de llamada para recibir los cambios de estado de la conexión. 

Se proporcionan tres directivas de reintentos:
- **Retroceso exponencial con vibración**: la directiva de reintentos predeterminada aplicada.  Tiende a ser agresiva al comienzo, se ralentiza y luego llega a un retroceso máximo que no se supera.  El diseño se basa en la [guía de reintentos de Azure Architecture Center](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific).
- **Reintento personalizado**: puede implementar una directiva de reintentos personalizada e insertarla en RetryPolicy en función del lenguaje que selecciona. Puede diseñar una directiva de reintentos adecuada para el escenario.  Esto no está disponible en el SDK de C.
- **Sin reintento**: hay una opción para establecer la directiva de reintentos en "sin reintento" que deshabilita la lógica de reintentos.  El SDK intenta conectarse una vez y envía un mensaje una vez, suponiendo que se establece la conexión. Habitualmente, esta directiva se usaría en casos donde hay problemas de costos o de ancho de banda.   Si se elige esta opción, los mensajes que no se envían se pierden y no se pueden recuperar. 

### <a name="retry-policy-apis"></a>API de directiva de reintentos

   | SDK | Método SetRetryPolicy | Implementaciones de directivas | Guía de implementación |
   |-----|----------------------|--|--|
   |  C/Python/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Valor predeterminado**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Personalizado:** use la [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies) disponible<BR>**Sin reintento:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [Implementación de C/Python/iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.sdk.iot.device._device_client_config.setretrypolicy?view=azure-java-stable)        | **Valor predeterminado**: [clase ExponentialBackoffWithJitter](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Personalizado:** implemente la [interfaz RetryPolicy](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java)<BR>**Sin reintento:** [clase NoRetry](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Implementación de Java](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |[SDK de .NET](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_SetRetryPolicy_Microsoft_Azure_Devices_Client_IRetryPolicy) | **Valor predeterminado**: [clase ExponentialBackoff](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Personalizado:** implemente la [interfaz IRetryPolicy](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet)<BR>**Sin reintento:** [clase NoRetry](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [Implementación de C#](https://github.com/Azure/azure-iot-sdk-csharp) |
   | Nodo| [setRetryPolicy](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest#azure_iot_device_Client_setRetryPolicy) | **Valor predeterminado**: [clase ExponentialBackoffWithJitter](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Personalizado:** implemente la [interfaz RetryPolicy](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest)<BR>**Sin reintento:** [clase NoRetry](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Implementación de Node](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |
   

A continuación, aparecen ejemplo de código que ilustran este flujo. 

#### <a name="net-implementation-guidance"></a>Guía de implementación de .NET

El ejemplo de código siguiente muestra cómo definir y establecer la directiva de reintentos predeterminada:

   ```csharp
   # define/set default retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Para evitar el uso elevado de CPU, los reintentos se limitan si el código presenta un error de inmediato (por ejemplo, cuando no hay red ni ruta al destino), por tanto, el tiempo mínimo para ejecutar el reintento siguiente es de 1 segundo. 

Si el servicio responde con un error de limitación, la directiva de reintentos es diferente y no se puede cambiar a través de la API pública:

   ```csharp
   # throttled retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5));
   SetRetryPolicy(retryPolicy);
   ```

El mecanismo de reintento se detendrá después de `DefaultOperationTimeoutInMilliseconds`, que actualmente está establecido en 4 minutos.

#### <a name="other-languages-implementation-guidance"></a>Guía de implementación de otros lenguajes
Revise la documentación de implementación que aparece a continuación para información sobre otros lenguajes.  En el repositorio aparecen ejemplos que muestran el uso de las API de directiva de reintentos.
- [SDK de C/Python/iOS](https://github.com/azure/azure-iot-sdk-c)
- [SDK de .NET](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [SDK de Java](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [SDK de Node](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="next-steps"></a>Pasos siguientes
- [Uso de los SDK de dispositivos y servicios](.\iot-hub-devguide-sdks.md)
- [Uso del SDK de dispositivo IoT para C](.\iot-hub-device-sdk-c-intro.md)
- [Desarrollo para dispositivos restringidos](.\iot-hub-devguide-develop-for-constrained-devices.md)
- [Desarrollo para dispositivos móviles](.\iot-hub-how-to-develop-for-mobile-devices.md)
- [Solución de problemas de desconexión de dispositivos](iot-hub-troubleshoot-connectivity.md)
