---
title: Envío de mensajes de seguridad a Azure Security Center para IoT | Microsoft Docs
description: Aprenda a enviar mensajes de seguridad mediante Azure Security Center for IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: c611bb5c-b503-487f-bef4-25d8a243803d
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2019
ms.author: mlottner
ms.openlocfilehash: c780eea15b9f064d3279c75ac2f967e8b6099ecb
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596212"
---
# <a name="send-security-messages-sdk"></a>SDK de Send Security Message

En esta guía de procedimientos se describen las funcionalidades del servicio Azure Security Center para IoT al elegir recopilar y enviar mensajes de seguridad de un dispositivo sin utilizar ningún agente de Azure Security Center para IoT, y se explica cómo hacerlo.  

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Usar Send Security Message API para C#
> * Usar Send Security Message API para C

## <a name="azure-security-center-for-iot-capabilities"></a>Funcionalidades de Azure Security Center para IoT

Azure Security Center para IoT puede procesar y analizar cualquier tipo de datos de mensajes de seguridad, siempre que los datos enviados se ajusten al [esquema de Azure Security Center para IoT](https://aka.ms/iot-security-schemas) y el mensaje en cuestión esté configurado como un mensaje de seguridad.

## <a name="security-message"></a>Mensaje de seguridad

Azure Security Center para IoT define un mensaje de seguridad teniendo en cuenta los siguientes criterios:
- Si el mensaje se envió con SDK de C/C# de Azure IoT.
- Si el mensaje se ajusta al [esquema de mensaje de seguridad](https://aka.ms/iot-security-schemas).
- Si el mensaje se ha configurado como un mensaje de seguridad antes de enviarse.

Cada mensaje de seguridad incluye los metadatos del remitente, como `AgentId`, `AgentVersion`, `MessageSchemaVersion` y una lista de eventos de seguridad.
El esquema define las propiedades válidas y obligatorias del mensaje de seguridad, incluidos los tipos de eventos.

>[!Note]
> Los mensajes enviados que no cumplan el esquema se ignoran. Asegúrese de comprobar el esquema antes de iniciar el envío de datos, ya que los mensajes ignorados no se almacenan. 

>[!Note]
> Los mensajes enviados que no estén configurados como mensajes de seguridad con el SDK de C/C# de Azure IoT no se enrutarán a la canalización de Azure Security Center para IoT.

## <a name="valid-message-example"></a>Ejemplo de mensaje válido

En el siguiente ejemplo se muestra un objeto de mensaje de seguridad válido. El ejemplo contiene los metadatos del mensaje y un evento de seguridad `ProcessCreate`.

Una vez que el mensaje esté configurado como mensaje de seguridad y se haya enviado, Azure Security Center para IoT lo procesará.

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
                    "Executable": "/usr/bin/myApp",
                    "ProcessId": 11750,
                    "ParentProcessId": 1593,
                    "UserName": "aUser",
                    "CommandLine": "myApp -a -b"
                }
            ]
    }
]
```

## <a name="send-security-messages"></a>Envío de mensajes de seguridad 

Envíe mensajes de seguridad sin usar el agente de Azure Security Center para IoT mediante el [SDK de dispositivo C# de Azure IoT](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview) o el [SDK de dispositivo C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview).

Para enviar los datos de los dispositivos para que los procese Azure Security Center para IoT, use una de las siguientes API para marcar los mensajes para que se enruten correctamente a la canalización de procesamiento de Azure Security Center para IoT. 

Todos los datos que se envíen, aunque se hayan marcado con el encabezado correcto, también deben cumplir el [esquema de mensajes de Azure Security Center para IoT](https://aka.ms/iot-security-schemas). 

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
- Lea la [introducción](overview.md) del servicio Azure Security Center para IoT.
- Obtenga más información sobre la [arquitectura](architecture.md) de Azure Security Center para IoT.
- Habilite el [servicio](quickstart-onboard-iot-hub.md)
- Lea las [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Aprenda a acceder a [datos de seguridad sin procesar](how-to-security-data-access.md)
- Obtenga información acerca de las [recomendaciones](concept-recommendations.md)
- Obtenga información acerca de las [alertas](concept-security-alerts.md)
