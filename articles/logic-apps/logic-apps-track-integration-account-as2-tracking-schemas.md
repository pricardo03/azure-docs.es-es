---
title: 'Esquemas de seguimiento de AS2 para mensajes B2B: Azure Logic Apps | Microsoft Docs'
description: Cree esquemas de seguimiento de AS2 que supervise los mensajes B2B en cuentas de integración para Azure Logic Apps con Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: f169c411-1bd7-4554-80c1-84351247bf94
ms.date: 01/27/2017
ms.openlocfilehash: 180d90450497b38f107f3601944385a003f50282
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60845790"
---
# <a name="create-schemas-for-tracking-as2-messages-and-mdns-in-integration-accounts-for-azure-logic-apps"></a>Creación de esquemas para el seguimiento de mensajes AS2 y MDN en cuentas de integración para Azure Logic Apps

Para ayudarle a supervisar éxitos, errores y propiedades de mensaje para transacciones de negocio a negocio (B2B), puede usar estos esquemas de seguimiento de AS2 en su cuenta de integración:

* Esquema de seguimiento de mensaje AS2
* Esquema de seguimiento de MDN AS2

## <a name="as2-message-tracking-schema"></a>Esquema de seguimiento de mensaje AS2

```json
{
   "agreementProperties": {  
      "senderPartnerName": "",  
      "receiverPartnerName": "",  
      "as2To": "",  
      "as2From": "",  
      "agreementName": ""  
   },  
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "dispositionType": "",
      "fileName": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isMessageEncrypted": "",
      "isMessageCompressed": "",
      "correlationMessageId": "",
      "incomingHeaders": {
       },
      "outgoingHeaders": {
       },
      "isNrrEnabled": "",
      "isMdnExpected": "",
      "mdnType": ""
    }
}
```

| Propiedad | Escriba | DESCRIPCIÓN |
| --- | --- | --- |
| senderPartnerName | String | Nombre de asociado del remitente del mensaje AS2. (Opcional) |
| receiverPartnerName | String | Nombre de asociado del destinatario del mensaje AS2. (Opcional) |
| as2To | String | Nombre del destinatario del mensaje AS2, de los encabezados del mensaje AS2. (Obligatorio) |
| as2From | String | Nombre del remitente del mensaje AS2, de los encabezados del mensaje AS2. (Obligatorio) |
| agreementName | String | Nombre del contrato de AS2 en el que se resuelven los mensajes. (Opcional) |
| dirección | String | Dirección del flujo de mensajes, recibidos o enviados. (Obligatorio) |
| messageId | String | Identificador del mensaje AS2, de los encabezados del mensaje AS2 (opcional) |
| dispositionType |String | Valor del tipo de disposición de notificación de disposición del mensaje (MDN). (Opcional) |
| fileName | String | Nombre de archivo, del encabezado del mensaje AS2. (Opcional) |
| isMessageFailed |Boolean | Si el mensaje AS2 genera error. (Obligatorio) |
| isMessageSigned | Boolean | Si el mensaje AS2 está firmado. (Obligatorio) |
| isMessageEncrypted | Boolean | Si el mensaje AS2 está cifrado. (Obligatorio) |
| isMessageCompressed |Boolean | Si el mensaje AS2 está comprimido. (Obligatorio) |
| correlationMessageId | String | Identificador de mensajes AS2, para correlacionar mensajes con MDN. (Opcional) |
| incomingHeaders |Diccionario de JToken | Detalles del encabezado del mensaje AS2 entrante. (Opcional) |
| outgoingHeaders |Diccionario de JToken | Detalles del encabezado del mensaje AS2 saliente. (Opcional) |
| isNrrEnabled | Boolean | Si no se conoce el valor, use el valor predeterminado. (Obligatorio) |
| isMdnExpected | Boolean | Si no se conoce el valor, use el valor predeterminado. (Obligatorio) |
| mdnType | Enum | Los valores permitidos son **NotConfigured**, **Sync** y **Async**. (Obligatorio) |
||||

## <a name="as2-mdn-tracking-schema"></a>Esquema de seguimiento de MDN AS2

```json
{
   "agreementProperties": {
      "senderPartnerName": "",
      "receiverPartnerName": "",
      "as2To": "",
      "as2From": "",
      "agreementName": "g"
   },
   "messageProperties": {
      "direction": "",
      "messageId": "",
      "originalMessageId": "",
      "dispositionType": "",
      "isMessageFailed": "",
      "isMessageSigned": "",
      "isNrrEnabled": "",
      "statusCode": "",
      "micVerificationStatus": "",
      "correlationMessageId": "",
      "incomingHeaders": {
      },
      "outgoingHeaders": {
      }
   }
}
```

| Propiedad | Escriba | DESCRIPCIÓN |
| --- | --- | --- |
| senderPartnerName | String | Nombre de asociado del remitente del mensaje AS2. (Opcional) |
| receiverPartnerName | String | Nombre de asociado del destinatario del mensaje AS2. (Opcional) |
| as2To | String | Nombre del asociado que recibe el mensaje AS2. (Obligatorio) |
| as2From | String | Nombre del asociado que envía el mensaje AS2. (Obligatorio) |
| agreementName | String | Nombre del contrato de AS2 en el que se resuelven los mensajes. (Opcional) |
| dirección |String | Dirección del flujo de mensajes, recibidos o enviados. (Obligatorio) |
| messageId | String | Identificador del mensaje AS2. (Opcional) |
| originalMessageId |String | Identificador del mensaje original AS2. (Opcional) |
| dispositionType | String | Valor de tipo de disposición de MDN. (Opcional) |
| isMessageFailed |Boolean | Si el mensaje AS2 genera error. (Obligatorio) |
| isMessageSigned |Boolean | Si el mensaje AS2 está firmado. (Obligatorio) |
| isNrrEnabled | Boolean | Si no se conoce el valor, use el valor predeterminado. (Obligatorio) |
| statusCode | Enum | Los valores permitidos son **Accepted**, **Rejected**, y **AcceptedWithErrors**. (Obligatorio) |
| micVerificationStatus | Enum | Los valores permitidos son **NotApplicable**, **Succeeded** y **Failed**. (Obligatorio) |
| correlationMessageId | String | Id. de correlación. Identificador del mensaje original (identificador del mensaje para el que está configurando MDN). (Opcional) |
| incomingHeaders | Diccionario de JToken | Indica los detalles del encabezado del mensaje entrante. (Opcional) |
| outgoingHeaders |Diccionario de JToken | Indica los detalles del encabezado del mensaje saliente. (Opcional) |
||||

## <a name="b2b-protocol-tracking-schemas"></a>Esquemas de seguimiento de protocolos B2B

Para obtener información sobre los esquemas de seguimiento de protocolos B2B, consulte:

* [Esquemas de seguimiento de X12](logic-apps-track-integration-account-x12-tracking-schema.md)
* [Esquemas de seguimiento personalizado de B2B](logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Pasos siguientes

* Información acerca de la [supervisión de mensajes B2B](logic-apps-monitor-b2b-message.md)
* Obtenga información sobre el [seguimiento de mensajes B2B en registros de Azure Monitor](../logic-apps/logic-apps-track-b2b-messages-omsportal.md)