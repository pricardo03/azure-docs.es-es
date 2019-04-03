---
title: Enviar los mensajes de seguridad a Azure Security Center para la versión preliminar de IoT | Microsoft Docs
description: Obtenga información sobre cómo enviar los mensajes de seguridad con Azure Security Center para IoT.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: c611bb5c-b503-487f-bef4-25d8a243803d
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: a9974fd15ae9c8c420992c3ae1084feebae0f57d
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862410"
---
# <a name="send-security-messages-sdk"></a>SDK de Send Security Message

> [!IMPORTANT]
> Azure Security Center para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esta guía explica Azure Security Center (ASC) para las funcionalidades del servicio IoT cuando opta por recopilar y enviar mensajes de seguridad de su dispositivo sin utilizar un ASC para el agente de IoT y explica cómo hacerlo.  

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Usar Send Security Message API para C#
> * Usar Send Security Message API para C

## <a name="asc-for-iot-capabilities"></a>ASC para las funcionalidades de IoT

ASC para IoT pueda procesar y analizar cualquier tipo de datos de mensaje de seguridad siempre y cuando los datos enviados se ajustan a la [ASC para IoT esquema](https://aka.ms/iot-security-schemas) y el mensaje se establece como un mensaje de seguridad.

## <a name="security-message"></a>Mensaje de seguridad

ASC para IoT define un mensaje de seguridad con los siguientes criterios:
- Si el mensaje se envió con Azure IoT C /C# SDK
- Si el mensaje se ajusta a la [esquema de mensaje de seguridad](https://aka.ms/iot-security-schemas)
- Si el mensaje se ha establecido como un mensaje de seguridad antes de enviar

Cada mensaje de seguridad incluye los metadatos del remitente como `AgentId`, `AgentVersion`, `MessageSchemaVersion` y una lista de eventos de seguridad.
El esquema define las propiedades válidas y requiere del mensaje de seguridad, incluidos los tipos de eventos.

[!NOTE]
> Los mensajes enviados que no cumplan el esquema se ignoran. Asegúrese de comprobar el esquema antes de iniciar el envío de datos, ya que los mensajes ignorados no se almacenan. 
> Envían los mensajes que no se establecieron como un mensaje de seguridad con Azure IoT C /C# SDK no se enrutarán a la ASC para canalización de IoT

## <a name="valid-message-example"></a>Ejemplo de mensaje válido

El ejemplo siguiente muestra un objeto de mensaje de seguridad válido. El ejemplo contiene los metadatos del mensaje y otro `ProcessCreate` eventos de seguridad.

Una vez establecido como un mensaje de seguridad y la envía, este mensaje se procesará por ASC para IoT.

```json
"AgentVersion": "0.0.1",
"AgentId": "e89dc5f5-feac-4c3e-87e2-93c16f010c25",
"MessageSchemaVersion": "1.0",
"Events": [
    {
        "EventType": "Security",
        "Category": "Triggered",
        "Name": "ProcessCreate",
        "IsEmpty": false,
        "PayloadSchemaVersion": "1.0",
        "Id": "21a2db0b-44fe-42e9-9cff-bbb2d8fdf874",
        "TimestampLocal": "2019-01-27 15:48:52Z",
        "TimestampUTC": "2019-01-27 13:48:52Z",
        "Payload":
            [
                {
                    "Executable": "/usr/bin/echo",
                    "ProcessId": 11750,
                    "ParentProcessId": 1593,
                    "UserName": "nginx",
                    "CommandLine": "./backup .htaccess"
                }
            ]
    }
]
```

## <a name="send-security-messages"></a>Envío de mensajes de seguridad 

Enviar mensajes de seguridad sin usar la ASC para el agente de IoT, mediante el [Azure IoT C# SDK de dispositivo](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview) o [SDK de dispositivos Azure IoT C](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview).

Para enviar los datos de los dispositivos para que los procese ASC for IoT, use una de las siguientes API para marcar los mensajes para que se enruten correctamente a la canalización de procesamiento de ASC for IoT. Los mensajes enviados de esta manera se procesarán y se mostrarán como información de seguridad de ASC for IoT tanto en IoT Hub como en Azure Security Center. 

Todos los datos que se envíen, aunque se hayan marcado con el encabezado correcto, también deben cumplir el [esquema de mensajes de ASC for IoT](https://aka.ms/iot-security-schemas). 

### <a name="send-security-message-api"></a>Send Security Message API

**Send Security Message** API está disponible actualmente en C y C#.  

#### <a name="c-api"></a>API de C#

```cs

private static async Task SendSecurityMessageAsync(string messageContent)
{
    ModuleClient client = ModuleClient.CreateFromConnectionString("<connection_string>");
    Message  securityMessage = new Message(Encoding.UTF8.GetBytes(messageContent));
    securityMessage.SetAsSecurityMessage();
    await client.SendEventAsync(securityMessage);
}
```

#### <a name="c-api"></a>API de C

```c
bool SendMessageAsync(IoTHubAdapter* iotHubAdapter, const void* data, size_t dataSize) {
 
    bool success = true;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;
 
    messageHandle = IoTHubMessage_CreateFromByteArray(data, dataSize);
 
    if (messageHandle == NULL) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubMessage_SetAsSecurityMessage(messageHandle) != IOTHUB_MESSAGE_OK) {
        success = false;
        goto cleanup;
    }
 
    if (IoTHubModuleClient_SendEventAsync(iotHubAdapter->moduleHandle, messageHandle, SendConfirmCallback, iotHubAdapter) != IOTHUB_CLIENT_OK) {
        success = false;
        goto cleanup;
    }
 
cleanup:
    if (messageHandle != NULL) {
        IoTHubMessage_Destroy(messageHandle);
    }
 
    return success;
}
 
static void SendConfirmCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback) {
    if (userContextCallback == NULL) {
        //error handling
        return;
    }
 
    if (result != IOTHUB_CLIENT_CONFIRMATION_OK){
        //error handling
    }
}
```

## <a name="next-steps"></a>Pasos siguientes
- Lea la [información general](overview.md) del servicio ASC for IoT
- Obtenga más información acerca de la [arquitectura](architecture.md) de ASC for IoT
- Habilite el [servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Aprenda a acceder a [datos de seguridad sin procesar](how-to-security-data-access.md)
- Obtenga información acerca de las [recomendaciones](concept-recommendations.md)
- Obtenga información acerca de las [alertas](concept-security-alerts.md)
