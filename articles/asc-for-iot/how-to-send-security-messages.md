---
title: Envío de mensajes de seguridad a Azure Security Center for IoT, versión preliminar | Microsoft Docs
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
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 73335773695059b3c2afd121a0dd39ada8d28bb0
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618086"
---
# <a name="send-security-messages-sdk"></a>SDK de Send Security Message

> [!IMPORTANT]
> Azure Security Center for IoT está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En esta guía de procedimientos se describen las capacidades del servicio Azure Security Center (ASC ) for IoT al elegir recopilar y enviar mensajes de seguridad de un dispositivo sin utilizar ningún agente de ASC for IoT, y se explica cómo hacerlo.  

En esta guía, aprenderá a: 
> [!div class="checklist"]
> * Usar Send Security Message API para C#
> * Usar Send Security Message API para C

## <a name="asc-for-iot-capabilities"></a>Capacidades de ASC for IoT

ASC for IoT puede procesar y analizar cualquier tipo de datos de mensajes de seguridad, siempre que los datos enviados se ajusten al [esquema de ASC for IoT](https://aka.ms/iot-security-schemas) y el mensaje en cuestión esté configurado como un mensaje de seguridad.

## <a name="security-message"></a>Mensaje de seguridad

ASC for IoT define un mensaje de seguridad teniendo en cuenta los siguientes criterios:
- Si el mensaje se envió con SDK de C/C# de Azure IoT.
- Si el mensaje se ajusta al [esquema de mensaje de seguridad](https://aka.ms/iot-security-schemas).
- Si el mensaje se ha configurado como un mensaje de seguridad antes de enviarse.

Cada mensaje de seguridad incluye los metadatos del remitente, como `AgentId`, `AgentVersion`, `MessageSchemaVersion` y una lista de eventos de seguridad.
El esquema define las propiedades válidas y obligatorias del mensaje de seguridad, incluidos los tipos de eventos.

[!NOTE]
> Los mensajes enviados que no cumplan el esquema se ignoran. Asegúrese de comprobar el esquema antes de iniciar el envío de datos, ya que los mensajes ignorados no se almacenan. 
> Los mensajes enviados que no estén configurados como mensajes de seguridad con el SDK de C/C# de Azure IoT no se enrutarán a la canalización de ASC for IoT.

## <a name="valid-message-example"></a>Ejemplo de mensaje válido

En el siguiente ejemplo se muestra un objeto de mensaje de seguridad válido. El ejemplo contiene los metadatos del mensaje y un evento de seguridad `ProcessCreate`.

Una vez que el mensaje esté configurado como mensaje de seguridad y se haya enviado, ASC for IoT lo procesará.

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

Envíe mensajes de seguridad sin usar el agente de ASC for IoT mediante el [SDK de dispositivo C# de Azure IoT](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview) o el [SDK de dispositivo C de Azure IoT](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview).

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
