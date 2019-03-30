---
title: Enviar los mensajes de seguridad a ASC para la versión preliminar de IoT | Microsoft Docs
description: Obtenga información sobre cómo enviar los mensajes de seguridad con ASC para IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: c611bb5c-b503-487f-bef4-25d8a243803d
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: d8eefff5a5ea03237eea60a63b63d70f605f10b3
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650162"
---
# <a name="send-security-messages-sdk"></a>Enviar mensajes de seguridad SDK

> [!IMPORTANT]
> ASC para IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no se recomienda para las cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Esta guía explica ASC para las funcionalidades del servicio IoT cuando opta por recopilar y enviar mensajes de seguridad de su dispositivo sin utilizar un ASC para el agente de IoT y explica cómo hacerlo.  

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Usar el mensaje de seguridad de envío API paraC#
> * Utilice el envío de mensajes de seguridad API para C

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
> Envían los mensajes que no son compatibles con el esquema se omiten. Asegúrese de comprobar el esquema antes de iniciar el envío de datos como mensajes omitidos no están almacenados actualmente. 
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

## <a name="send-security-messages"></a>Enviar mensajes de seguridad 

Enviar mensajes de seguridad sin usar la ASC para el agente de IoT, mediante el [Azure IoT C# SDK de dispositivo](https://github.com/Azure/azure-iot-sdk-csharp) o [SDK de dispositivos Azure IoT C](https://github.com/Azure/azure-iot-sdk-c).

Para enviar los datos del dispositivo desde los dispositivos para el procesamiento por ASC para IoT, use una de las siguientes API para marcar mensajes para el enrutamiento correcto a ASC para canalización de procesamiento de IoT. Los mensajes enviados de esta manera se procesará y se muestran como información de seguridad en ASC para IoT dentro de ambos IoT Hub o Azure Security Center. 

Todos los datos que se envían, incluso si se marca con el encabezado correcto, también deben cumplir con la [ASC para el esquema de mensaje IoT](https://aka.ms/iot-security-schemas). 

### <a name="send-security-message-api"></a>API de mensaje de seguridad de envío

El **enviar mensajes de seguridad** API está actualmente disponible en C y C#.  

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

#### <a name="c-api"></a>C API

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
- Leer los procesos de ASC para el servicio IoT [información general](overview.md)
- Más información sobre los procesos de ASC para IoT [arquitectura](architecture.md)
- Habilitar la [servicio](quickstart-onboard-iot-hub.md)
- Leer el [preguntas más frecuentes](resources-frequently-asked-questions.md)
- Obtenga información sobre cómo obtener acceso a [datos sin procesar de seguridad](how-to-security-data-access.md)
- Comprender [recomendaciones](concept-recommendations.md)
- Comprender [alertas](concept-security-alerts.md)
