---
title: 'Esquemas de seguimiento personalizado para mensajes B2B: Azure Logic Apps | Microsoft Docs'
description: Cree esquemas de seguimiento que supervisen los mensajes B2B en cuentas de integración para Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 433ae852-a833-44d3-a3c3-14cca33403a2
ms.date: 01/27/2017
ms.openlocfilehash: 76a9ece9e925543e856136a798a60038316caad9
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203037"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-apps"></a>Creación de esquemas de seguimiento personalizado que supervisen los flujos de trabajo completos en Azure Logic Apps

Hay disponible una característica de seguimiento integrada que puede habilitar para las distintas partes de su flujo de trabajo de negocio a negocio, por ejemplo, para realizar un seguimiento de mensajes AS2 o X12. Cuando cree flujos de trabajo que incluyan una lógica de aplicación, BizTalk Server, SQL Server o cualquier otra capa, podrá habilitar el seguimiento personalizado que registre los eventos desde el principio hasta el final del flujo de trabajo. 

En este artículo se proporciona un código personalizado que puede usar en las capas externas de la aplicación lógica. 

## <a name="custom-tracking-schema"></a>Esquema de seguimiento personalizado

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": "",
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {                
         }
      }
   ]
}
```

| Propiedad | Obligatorio | type | DESCRIPCIÓN |
| --- | --- | --- | --- |
| sourceType | Sí |   | Tipo del origen de ejecución. Los valores permitidos son **Microsoft.Logic/workflows** y **custom**. |
| source | Sí |   | Si el tipo de origen es **Microsoft.Logic/workflows**, la información de origen tiene que ir detrás de este esquema. Si el tipo de origen es **custom**, el esquema es un JToken |
| systemId | Sí | String | Id. de sistema de aplicación lógica. |
| runId | Sí | String | Id. de ejecución de aplicación lógica. |
| operationName | Sí | String | Nombre de la operación (por ejemplo, acción o desencadenador). |
| repeatItemScopeName | Sí | String | Repita el nombre del elemento si la acción se encuentra dentro de un bucle `foreach`/`until`. |
| repeatItemIndex | Sí | Integer | Si la acción se encuentra dentro de un bucle `foreach`/`until`. Indica el índice del elemento repetido. |
| trackingId | Sin | String | Id. de seguimiento para correlacionar los mensajes. |
| correlationId | Sin | String | Id. de correlación para correlacionar los mensajes. |
| clientRequestId | Sin | String | El cliente puede rellenarlo para correlacionar mensajes. |
| eventLevel | Sí |   | Nivel del evento. |
| eventTime | Sí |   | Hora del evento, en formato UTC: AAAA-MM-DDTHH:MM:SS.00000Z. |
| recordType | Sí |   | Tipo del registro. El valor permitido es **custom**. |
| record | Sí |   | Tipo de registro personalizado. El formato permitido es JToken. |
||||

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de seguimiento de protocolos B2B

Para obtener información sobre los esquemas de seguimiento de protocolos B2B, consulte:

* [Esquemas de seguimiento de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)   
* [Esquemas de seguimiento de X12](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la [supervisión de mensajes B2B](logic-apps-monitor-b2b-message.md)
* Obtenga información sobre el [seguimiento de mensajes B2B en registros de Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)
